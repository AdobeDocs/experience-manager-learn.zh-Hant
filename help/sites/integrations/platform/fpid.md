---
title: 使用AEM產生Adobe Experience Platform FPID
description: 了解如何使用AEM產生或重新整理Adobe Experience Platform FPID Cookie。
version: Cloud Service
feature: Integrations, APIs, Dispatcher
topic: Integrations, Personalization, Development
role: Developer
level: Beginner
last-substantial-update: 2022-10-20T00:00:00Z
kt: 11336
thumbnail: kt-11336.jpeg
source-git-commit: aeeed85ec05de9538b78edee67db4d632cffaaab
workflow-type: tm+mt
source-wordcount: '1027'
ht-degree: 0%

---

# 使用AEM產生Experience PlatformFPID

將Adobe Experience Manager(AEM)與Adobe Experience Platform(AEP)整合需要AEM產生並維護唯一的第一方裝置ID(FPID)Cookie，以便對使用者活動進行唯一追蹤。

請閱讀支援檔案，以 [了解第一部分裝置ID與Experience CloudID如何搭配運作的詳細資訊](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html?lang=en).

以下概述使用AEM作為Web主機時FPID的運作方式。

![FPID與ECID搭配AEM](./assets/aem-platform-fpid-architecture.png)

## 使用AEM產生並保留FPID

AEM Publish服務會在CDN和AEM Dispatcher快取中，盡可能快取請求，以最佳化效能。

產生每個使用者唯一FPID Cookie並傳回FPID值的HTTP要求絕不會進行快取，且會直接從AEM Publish提供，以實作邏輯以保證獨特性。

請避免在網頁或其他可快取資源的要求上產生FPID Cookie，因為FPID的獨特性要求組合會讓這些資源變得無法取得。

下圖說明AEM Publish服務如何管理FPID。

![FPID與AEM流程圖](./assets/aem-fpid-flow.png)

1. 網頁瀏覽器會要求由AEM托管的網頁。 您可使用來自CDN或AEM Dispatcher快取的網頁快取副本來提供請求。
1. 如果無法從CDN或AEM Dispatcher快取提供網頁，請求便會送達AEM Publish服務，而AEM Publish服務會產生請求的網頁。
1. 然後網頁會傳回至網頁瀏覽器，填入無法提供請求的快取。 若使用AEM,CDN和AEM Dispatcher快取點擊率將會高於90%。
1. 網頁包含JavaScript，可對AEM Publish服務中的自訂FPID servlet發出不可執行的非同步XHR(AJAX)要求。 由於這是無法執行的請求（因為是隨機查詢參數和快取控制標題），因此CDN或AEM Dispatcher不會快取請求，且一律會到達AEM Publish服務以產生回應。
1. AEM Publish服務中的自訂FPID servlet會處理請求、在找不到現有FPID Cookie時產生新的FPID，或延長任何現有FPID Cookie的使用期限。 Servlet也會傳回回應內文中的FPID，供用戶端JavaScript使用。 幸運的是，自訂FPID servlet邏輯輕量型，可防止此要求影響AEM Publish服務效能。
1. XHR要求的回應會在回應內文中，傳回具有FPID Cookie和FPID作為JSON的瀏覽器，供Platform Web SDK使用。

## 程式碼範例

下列程式碼和設定可部署至AEM Publish服務，以建立端點，產生或延長現有FPID Cookie的使用期限，並將FPID傳回為JSON。

### AEM FPID cookie servlet

必須建立AEM HTTP端點，才能使用 [Sling Servlet](https://sling.apache.org/documentation/the-sling-engine/servlets.html#registering-a-servlet-using-java-annotations-1).

+ Servlet已綁定到 `/bin/aem/fpid` 因為不需要驗證才能存取。 如果需要驗證，請系結至Sling資源類型。
+ Servlet接受HTTPGET請求。 回應會加上 `Cache-Control: no-store` 為防止快取，但也應使用唯一的cache-busting查詢參數來請求此端點。

當HTTP請求到達servlet時，servlet會檢查請求上是否有FPID Cookie:

+ 如果FPID Cookie存在，請延長Cookie的期限，並收集其值以寫入回應。
+ 如果FPID Cookie不存在，請產生新的FPID Cookie，並儲存要寫入回應的值。

然後，Servlet會將FPID以JSON物件的形式寫入回應： `{ fpid: "<FPID VALUE>" }`.

請務必在內文中為用戶端提供FPID，因為已標籤FPID Cookie `HttpOnly`，表示只有伺服器可讀取其值，而用戶端JavaScript則無法讀取。

回應內文的FPID值可用來使用Platform Web SDK參數化呼叫。

以下是AEM servlet端點的范常式式碼(可透過 `HTTP GET /bin/aep/fpid`)產生或重新整理FPID Cookie，並將FPID傳回為JSON。

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

必須將自訂用戶端JavaScript新增至頁面，以非同步叫用servlet、產生或重新整理FPID Cookie，並在回應中傳回FPID。

此JavaScript指令碼類型會使用下列其中一種方法新增至頁面：

+ [Adobe Experience Platform中的標籤](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [AEM用戶端程式庫](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/clientlibs.html?lang=en)

對自訂AEM FPID servlet的XHR呼叫雖然非同步，但速度很快，因此使用者可以造訪AEM提供的網頁，並在請求完成前導覽至其他位置。
如果發生此情況，同一程式會在AEM的網頁下一頁載入時重新嘗試。

AEM FPID servlet的HTTPGET(`/bin/aep/fpid`)是以隨機查詢參數來參數化，以確保瀏覽器與AEM Publish服務之間的任何基礎架構不會快取請求的回應。
同樣地， `Cache-Control: no-store` 已新增要求標題，以支援避免快取。

在呼叫AEM FPID servlet時，會從JSON回應中擷取FPID，並由 [平台Web SDK](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/tags-configuration/install-web-sdk.html?lang=en) 傳送至Experience PlatformAPI。

如需詳細資訊，請參閱Experience Platform檔案 [在identityMap中使用FPID](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html#identityMap)

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

### Dispatcher允許篩選器

最後，必須允許透過AEM Dispatcher的 `filter.any` 設定。

如果此Dispatcher設定未正確實作，HTTPGET會要求 `/bin/aep/fpid` 結果是404。

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
/1099 { /type "allow" /method "GET" /url "/bin/aep/fpid" }
```

## Experience Platform資源

請檢閱下列Experience Platform檔案，取得第一方裝置ID(FPID)，並使用Platform Web SDK管理身分資料。

+ [產生第一方裝置ID](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html)
+ [Platform Web SDK中的第一方裝置ID](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html)
+ [Platform Web SDK中的身分資料](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html)


