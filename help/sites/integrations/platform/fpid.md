---
title: 使用生成Adobe Experience PlatformFPID AEM
description: 瞭解如何使用生成或刷新Adobe Experience PlatformFPID CookieAEM。
version: Cloud Service
feature: Integrations, APIs, Dispatcher
topic: Integrations, Personalization, Development
role: Developer
level: Beginner
last-substantial-update: 2022-10-20T00:00:00Z
kt: 11336
thumbnail: kt-11336.jpeg
exl-id: 18a22f54-da58-4326-a7b0-3b1ac40ea0b5
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '1027'
ht-degree: 0%

---

# 生成Experience PlatformFPID AEM

將Adobe Experience Manager(AEM)與Adobe Experience Platform(AEP)整合AEM需要生成並維護唯一的第一方設備ID(FPID)cookie，以便唯一地跟蹤用戶活動。

閱讀支援文檔以 [瞭解第一部分設備ID和Experience CloudID如何協同工作的詳細資訊](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html?lang=en)。

以下是FPID在用作Web主機時AEM的工作方式概述。

![FPID和ECID AEM](./assets/aem-platform-fpid-architecture.png)

## 生成並保留FPID AEM

AEM發佈服務通過在CDN和Dispatcher快取中盡可能多地快取請求來優化AEM效能。

HTTP請求必須生成唯一的每用戶FPIDcookie並返回FPID值，從不快取，直接從AEM Publish中提供，這樣可以實現邏輯以確保唯一性。

避免在網頁請求或其他可快取資源上生成FPID Cookie，因為FPID的唯一性要求的組合將使這些資源不可訪問。

下圖說明AEM發佈服務如何管理FPID。

![FPID和流AEM程圖](./assets/aem-fpid-flow.png)

1. Web瀏覽器對由托管的網頁發出請AEM求。 可使用來自CDN或Dispatcher快取的網頁的快取副本來AEM提供請求。
1. 如果無法從CDN或AEMDispatcher快取提供網頁服務，則請求將到達AEM發佈服務，該服務生成所請求的網頁。
1. 然後，將網頁返回到Web瀏覽器，填充無法服務該請求的快取。 對AEM於，預期CDN和AEMDispatcher快取命中率大於90%。
1. 該網頁包含JavaScript，該JavaScript對AEM發佈服務中的自定義FPID Servlet發出不AJAX可執行的非同步XHR()請求。 由於這是一個不可執行的請求（由於它的隨機查詢參數和Cache-Control標頭），因此CDN或AEMDispatcher從不對它進行快取，並且始終到達AEM發佈服務以生成響應。
1. AEM發佈服務中的自定義FPID Servlet處理該請求，當未找到現有FPID Cookie時生成新的FPID，或延長任何現有FPID Cookie的壽命。 Servlet還返迴響應體中的FPID供客戶端JavaScript使用。 幸運的是，自定義FPID servlet邏輯輕量，避免了此請求影響AEM發佈服務效能。
1. XHR請求的響應返回到瀏覽器，在響應體中FPIDcookie和FPID作為JSON，供平台Web SDK使用。

## 代碼示例

可以將以下代碼和配置部署到AEM發佈服務，以建立生成或延長現有FPIDcookie的壽命並將FPID返回為JSON的終結點。

### AEM FPID Cookie Servlet

必AEM須建立HTTP終結點以使用 [Sling Servlet](https://sling.apache.org/documentation/the-sling-engine/servlets.html#registering-a-servlet-using-java-annotations-1)。

+ Servlet已綁定到 `/bin/aem/fpid` 因為訪問它不需要身份驗證。 如果需要驗證，請綁定到Sling資源類型。
+ Servlet接受HTTPGET請求。 響應標籤為 `Cache-Control: no-store` 阻止快取，但也應使用唯一的快取突破查詢參數請求此終結點。

當HTTP請求到達Servlet時，Servlet將檢查請求上是否存在FPIDcookie:

+ 如果存在FPID Cookie，請延長Cookie的壽命，並收集其值以寫入響應。
+ 如果FPIDcookie不存在，請生成新的FPIDcookie，並保存要寫入響應的值。

然後，Servlet將FPID以JSON對象的形式寫入響應： `{ fpid: "<FPID VALUE>" }`。

由於FPIDcookie已標籤，因此必須向主體中的客戶端提供FPID `HttpOnly`，表示只有伺服器才能讀取其值，而客戶端JavaScript則不能。

響應體中的FPID值用於使用平台Web SDK參數化調用。

下面是Servlet終結點的示AEM例代碼(可通過 `HTTP GET /bin/aep/fpid`)生成或刷新FPIDcookie，並將FPID返回為JSON。

+ `core/src/main/java/com/adobe/aem/guides/wkndexamples/core/aep/impl/FpidServlet.java`

```java
package com.adobe.aem.guides.wkndexamples.core.aep.impl;

import com.google.gson.JsonObject;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.servlet.Servlet;
import javax.servlet.http.Cookie;
import java.io.IOException;
import java.util.UUID;

import static org.apache.sling.api.servlets.ServletResolverConstants.SLING_SERVLET_PATHS;
import static org.apache.sling.api.servlets.ServletResolverConstants.SLING_SERVLET_METHODS;

@Component(
        service = {Servlet.class},
        property = {
                SLING_SERVLET_PATHS + "=/bin/aep/fpid",
                SLING_SERVLET_METHODS + "=GET"
        }
)
public class FpidServlet extends SlingAllMethodsServlet {
    private static final Logger log = LoggerFactory.getLogger(FpidServlet.class);
    private static final String COOKIE_NAME = "FPID";
    private static final String COOKIE_PATH = "/";
    private static final int COOKIE_MAX_AGE = 60 * 60 * 24 * 30 * 13;
    private static final String JSON_KEY = "fpid";

    @Override
    protected final void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
        // Try to get an existing FPID cookie, this will give us the user's current FPID if it exists
        final Cookie existingCookie = request.getCookie(COOKIE_NAME);

        String cookieValue;

        if (existingCookie == null) {
            //  If no FPID cookie exists, Create a new FPID UUID
            cookieValue = UUID.randomUUID().toString();
        } else {
            // If a FPID cookie exists. get its FPID UUID so it's life can be extended
            cookieValue = existingCookie.getValue();
        }

        // Add the newly generate FPID value, or the extended FPID value to the response
        // Use addHeader(..), as we need to set SameSite=Lax (and addCoookie(..) does not support this)
        response.addHeader("Set-Cookie",
                COOKIE_NAME + "=" + cookieValue + "; " +
                        "Max-Age=" + COOKIE_MAX_AGE + "; " +
                        "Path=" + COOKIE_PATH + "; " +
                        "HttpOnly; " +
                        "Secure; " +
                        "SameSite=Lax");
        
        // Avoid caching the response in any cache
        response.addHeader("Cache-Control", "no-store");

        // Since the FPID is HttpOnly, JavaScript cannot read it (only the server can)
        // Write the FPID to the response as JSON so client JavaScript can access it.
        final JsonObject json = new JsonObject();
        json.addProperty(JSON_KEY, cookieValue);
        
        // The JSON `{ fpid: "11111111-2222-3333-4444-55555555" }` is returned in the response
        response.setContentType("application/json");
        response.getWriter().write(json.toString());
    }
}
```

### HTML指令碼

必須將自定義客戶端JavaScript添加到頁面中，以非同步調用Servlet、生成或刷新FPID Cookie並在響應中返回FPID。

此JavaScript指令碼按以下方法之一典型地添加到頁面：

+ [Adobe Experience Platform標籤](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [客AEM戶端庫](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/clientlibs.html?lang=en)

XHR對自定義AEMFPID servlet的調用速度很快，儘管是非同步的，因此用戶可以訪問由AEM服務的網頁，並在請求完成之前導航。
如果發生這種情況，則同一進程將重新嘗試從載入網頁的下一頁AEM。

到FPID servlet的AEMHTTPGET(`/bin/aep/fpid`)是使用隨機查詢參數參數化的，以確保瀏覽器和AEM發佈服務之間的任何基礎架構不快取請求的響應。
同樣， `Cache-Control: no-store` 請求標頭被添加以支援避免快取。

當調用FPIDAEM Servlet時，從JSON響應中檢索FPID並由 [平台Web SDK](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/tags-configuration/install-web-sdk.html?lang=en) 發送到Experience PlatformAPI。

有關Experience Platform的詳細資訊，請參閱 [在identityMap中使用FPID](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html#identityMap)

```javascript
...
<script>
    // Invoke the AEM FPID servlet, and then do something with the response

    fetch(`/bin/aep/fpid?_=${new Date().getTime() + '' + Math.random()}`, { 
            method: 'GET',
            headers: {
                'Cache-Control': 'no-store'
            }
        })
        .then((response) => response.json())
        .then((data) => { 
            // Get the FPID from JSON returned by AEM's FPID servlet
            console.log('My FPID is: ' + data.fpid);

            // Send the `data.fpid` to Experience Platform APIs            
        });
</script>
```

### 調度程式允許篩選器

最後，必須允許通過Dispatcher的HTTPGET請求到自AEM定義FPID Servlet `filter.any` 配置。

如果此Dispatcher配置未正確實施，則HTTPGET將請求 `/bin/aep/fpid` 結果為404。

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
/1099 { /type "allow" /method "GET" /url "/bin/aep/fpid" }
```

## Experience Platform資源

查看以下Experience Platform文檔，瞭解第一方設備ID(FPID)和使用平台Web SDK管理標識資料。

+ [生成第一方設備ID](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html)
+ [平台Web SDK中的第一方設備ID](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html)
+ [平台Web SDK中的標識資料](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html)
