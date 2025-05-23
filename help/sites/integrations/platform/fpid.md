---
title: 使用AEM Sites產生Adobe Experience Platform FPID
description: 瞭解如何使用AEM Sites產生或重新整理Adobe Experience Platform FPID Cookie。
version: Experience Manager as a Cloud Service
feature: Integrations, APIs, Dispatcher
topic: Integrations, Personalization, Development
role: Developer
level: Beginner
last-substantial-update: 2024-10-09T00:00:00Z
jira: KT-11336
thumbnail: kt-11336.jpeg
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service， AEM Sites 6.5" before-title="false"
exl-id: 18a22f54-da58-4326-a7b0-3b1ac40ea0b5
duration: 266
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1054'
ht-degree: 0%

---

# 使用AEM Sites產生Experience Platform FPID

將透過AEM Publish提供的Adobe Experience Manager (AEM)網站與Adobe Experience Platform (AEP)整合需要AEM產生和維護唯一的第一方裝置ID (FPID) Cookie，以專門追蹤使用者活動。

FPID Cookie應由伺服器(AEM Publish)設定，而非使用JavaScript來建立使用者端Cookie。 這是因為現代化瀏覽器（例如Safari和Firefox）可能會封鎖由JavaScript產生的Cookie或使其快速過期。

閱讀支援檔案，以便[瞭解第一部分裝置ID和Experience Cloud ID如何搭配運作的詳細資訊](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html?lang=zh-Hant)。

以下是使用AEM做為Web主機時，FPID的運作方式概觀。

使用AEM的![FPID與ECID](./assets/aem-platform-fpid-architecture.png)

## 使用AEM產生並保留FPID

AEM Publish服務會透過快取要求(儘可能在CDN和AEM Dispatcher快取中)來最佳化效能。

絕不會快取產生每位使用者不重複FPID Cookie並傳回FPID值的HTTP請求並直接從AEM Publish （可實作邏輯以確保唯一性）提供服務，這點至關重要。

請避免在網頁或其他可快取資源的請求上產生FPID Cookie，因為FPID的唯一性要求組合會造成這些資源無法快取。

下圖說明AEM Publish服務如何管理FPID。

![FPID與AEM流程圖](./assets/aem-fpid-flow.png)

1. 網頁瀏覽器會要求AEM代管的網頁。 CDN或AEM Dispatcher快取的網頁快取復本可為請求提供服務。
1. 如果網頁無法從CDN或AEM Dispatcher快取提供服務，要求會送達AEM Publish服務，該服務會產生要求的網頁。
1. 接著，網頁會傳回至網頁瀏覽器，並填入無法處理請求的快取。 使用AEM時，CDN和AEM Dispatcher快取命中率預計會高於90%。
1. 此網頁包含的JavaScript會對AEM Publish服務中的自訂FPID servlet發出無法快取的非同步XHR (AJAX)請求。 由於這是無法快取的請求（因其隨機查詢引數和快取控制標頭），CDN或AEM Dispatcher絕不會快取它，而是一律會送達AEM Publish服務以產生回應。
1. AEM Publish服務中的自訂FPID servlet會處理要求、在沒有找到現有FPID Cookie時產生新的FPID，或延長任何現有FPID Cookie的生命週期。 此servlet也會傳回回應本文中的FPID，以供使用者端JavaScript使用。 幸運的是，自訂FPID servlet邏輯是輕量級的，防止此請求影響AEM發佈服務效能。
1. XHR要求的回應會傳回給瀏覽器，並在回應本文中以JSON格式顯示FPID Cookie，以供Platform Web SDK使用。

## 程式碼範例

下列程式碼和設定可部署至AEM Publish服務，以建立端點，產生或延長現有FPID Cookie的生命週期，並將FPID傳回JSON。

### AEM發佈FPID Cookie servlet

必須使用[Sling servlet](https://sling.apache.org/documentation/the-sling-engine/servlets.html#registering-a-servlet-using-java-annotations-1)建立AEM發佈HTTP端點，以產生或擴充FPID Cookie。

+ 此servlet已繫結至`/bin/aem/fpid`，因為存取它不需要驗證。 如果需要驗證，則繫結到Sling資源型別。
+ 此servlet接受HTTP GET請求。 回應標示為`Cache-Control: no-store`以防止快取，但應使用唯一的防快取查詢引數來要求此端點。

當HTTP請求到達servlet時，此servlet會檢查請求上是否存在FPID Cookie：

+ 如果FPID Cookie存在，請延長Cookie的生命週期，並收集其值以寫入回應。
+ 如果FPID Cookie不存在，請產生新的FPID Cookie，並儲存值以寫入回應。

然後，Servlet會將FPID以JSON物件的形式寫入回應： `{ fpid: "<FPID VALUE>" }`。

請務必在正文中將FPID提供給使用者端，因為FPID Cookie標示為`HttpOnly`，表示只有伺服器可以讀取其值，使用者端JavaScript則無法讀取。 為了避免在每次載入頁面時都重新擷取FPID，系統也會設定`FPID_CLIENT` Cookie，指出FPID已產生，並將值公開給使用者端JavaScript使用。

FPID值可用來引數化Platform Web SDK的呼叫。

以下是AEM servlet端點的範常式式碼（可透過`HTTP GET /bin/aep/fpid`使用），其會產生或重新整理FPID Cookie，並傳回FPID為JSON。

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
    private static final String CLIENT_COOKIE_NAME = "FPID_CLIENT";
    private static final String COOKIE_PATH = "/";
    private static final int COOKIE_MAX_AGE = 60 * 60 * 24 * 30 * 13; // 13 months
    private static final String JSON_KEY = "fpid";

    @Override
    protected final void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
        // Try to get an existing FPID cookie, this will give us the user's current FPID if it exists
        final Cookie existingCookie = request.getCookie(COOKIE_NAME);

        String cookieValue;

        if (existingCookie == null) {
            //  If no FPID cookie exists, create a new FPID UUID
            cookieValue = UUID.randomUUID().toString();
        } else {
            // If a FPID cookie exists, get its FPID UUID so its life can be extended
            cookieValue = existingCookie.getValue();
        }

        // Add the FPID value to the response, either newly generated or the extended one
        // This can be read by the Server (AEM Publish) due to HttpOnly flag.
        response.addHeader("Set-Cookie",
                COOKIE_NAME + "=" + cookieValue + "; " +
                        "Max-Age=" + COOKIE_MAX_AGE + "; " +
                        "Path=" + COOKIE_PATH + "; " +
                        "HttpOnly; " +
                        "Secure; " +
                        "SameSite=Lax");

        // Also set FPID_CLIENT cookie to avoid further server-side FPID generation
        // This can be read by the client-side JavaScript to check if FPID is already generated
        // or if it needs to be requested from server (AEM Publish)
        response.addHeader("Set-Cookie",
                CLIENT_COOKIE_NAME + "=" + cookieValue + "; " +
                        "Max-Age=" + COOKIE_MAX_AGE + "; " +
                        "Path=" + COOKIE_PATH + "; " +
                        "Secure; " + 
                        "SameSite=Lax");

        // Avoid caching the response
        response.addHeader("Cache-Control", "no-store");

        // Return FPID in the response as JSON for client-side access
        final JsonObject json = new JsonObject();
        json.addProperty(JSON_KEY, cookieValue);

        response.setContentType("application/json");
        response.getWriter().write(json.toString());
```

### HTML指令碼

必須在頁面中新增自訂使用者端JavaScript，以非同步叫用servlet，產生或重新整理FPID Cookie，並在回應中傳回FPID。

此JavaScript指令碼通常使用下列其中一種方法新增至頁面：

+ Adobe Experience Platform中的[標籤](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=zh-Hant)
+ [AEM使用者端資源庫](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/clientlibs.html?lang=zh-Hant)

對自訂AEM FPID servlet的XHR呼叫雖然非同步，但速度很快，因此使用者可以造訪AEM提供的網頁，並在完成請求之前導覽離開。
如果發生這種情形，相同的程式會在從AEM載入網頁的下一個頁面時重新嘗試。

AEM FPID servlet (`/bin/aep/fpid`)的HTTP GET已使用隨機查詢引數加以引數化，以確保瀏覽器與AEM發佈服務之間的任何基礎結構都不會快取要求的回應。
同樣地，已新增`Cache-Control: no-store`要求標頭以支援避免快取。

叫用AEM FPID servlet時，FPID會從JSON回應中擷取，並由[Platform Web SDK](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/tags-configuration/install-web-sdk.html?lang=zh-Hant)用來傳送給Experience Platform API。

請參閱Experience Platform檔案以取得在identityMap[&#128279;](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html?lang=zh-Hant#identityMap)中使用FPID之的詳細資訊

```javascript
...
<script>
    // Wrap in anonymous function to avoid global scope pollution

    (function() {
        // Utility function to get a cookie value by name
        function getCookie(name) {
            const value = `; ${document.cookie}`;
            const parts = value.split(`; ${name}=`);
            if (parts.length === 2) return parts.pop().split(';').shift();
        }

        // Async function to handle getting the FPID via fetching from AEM, or reading an existing FPID_CLIENT cookie
        async function getFpid() {
            let fpid = getCookie('FPID_CLIENT');
            
            // If FPID can be retrieved from FPID_CLIENT then skip fetching FPID from server
            if (!fpid) {
                // Fetch FPID from the server if no FPID_CLIENT cookie value is present
                try {
                    const response = await fetch(`/bin/aep/fpid?_=${new Date().getTime() + '' + Math.random()}`, {
                        method: 'GET',
                        headers: {
                            'Cache-Control': 'no-store'
                        }
                    });
                    const data = await response.json();
                    fpid = data.fpid;
                } catch (error) {
                    console.error('Error fetching FPID:', error);
                }
            }

            console.log('My FPID is: ', fpid);
            return fpid;
        }

        // Invoke the async function to fetch or skip FPID
        const fpid = await getFpid();

        // Add the fpid to the identityMap in the Platform Web SDK
        // and/or send to AEP via AEP tags or direct AEP Web SDK calls (alloy.js)
    })();
</script>
```

### Dispatcher允許篩選器

最後，必須透過AEM Dispatcher的`filter.any`設定，允許對自訂FPID servlet的HTTP GET要求。

如果此Dispatcher設定未正確實作，對`/bin/aep/fpid`發出的HTTP GET請求會導致404錯誤。

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
/1099 { /type "allow" /method "GET" /url "/bin/aep/fpid" }
```

## Experience Platform資源

檢閱下列Experience Platform檔案，瞭解第一方裝置ID (FPID)並透過Platform Web SDK管理身分資料。

+ [產生第一方裝置識別碼](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html?lang=zh-Hant)
+ Platform Web SDK中的[第一方裝置識別碼](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html?lang=zh-Hant)
+ [Platform Web SDK中的身分資料](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html?lang=zh-Hant)
