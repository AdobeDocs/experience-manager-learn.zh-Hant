---
title: 了解 AEM 的跨來源資源共用 (CORS)
description: Adobe Experience Manager 的跨來源資源共用 (CORS) 可協助非 AEM 網頁屬性對 AEM 進行已驗證和未驗證兩種用戶端呼叫，以取得內容或直接與 AEM 互動。
version: Experience Manager 6.4, Experience Manager 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: Security, APIs
doc-type: Article
topic: Security
role: Developer
level: Intermediate
exl-id: 6009d9cf-8aeb-4092-9e8c-e2e6eec46435
duration: 240
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1011'
ht-degree: 100%

---

# 了解跨來源資源共用 ([!DNL CORS])

Adobe Experience Manager 的跨來源資源共用 ([!DNL CORS]) 可協助非 AEM 網頁屬性對 AEM 進行已驗證和未驗證兩種用戶端呼叫，以取得內容或直接與 AEM 互動。

本文件中所列的 OSGI 設定可以滿足以下需求：

1. AEM Publish 上的單一來源資源共用
2. 對於 AEM Author 的 CORS 存取權

若 AEM Publish 上需要多來源 CORS 存取權，請參閱[此文件](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=zh-Hant#dispatcher-configuration)。

## Adobe Granite 跨來源資源共用原則 OSGi 設定

CORS 設定在 AEM 中會以 OSGi 設定工廠的形式進行管理，每個原則都是該工廠的一個實例。

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Adobe Granite 跨來源資源共用原則 OSGi 設定](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### 原則選取

選取原則的方式是比較

* `Allowed Origin` 與 `Origin` 要求標頭
* 以及 `Allowed Paths` 與要求路徑。

使用與這些值相符的第一個原則。若無合適的結果，則會拒絕任何 [!DNL CORS] 要求。

若完全沒有設定任何原則，[!DNL CORS] 要求也同樣不會得到回應，因為處理常式已停用，因此實際上等同被拒絕，只要沒有其他伺服器模組回應 [!DNL CORS] 的話。

### 原則屬性

#### [!UICONTROL 允許的來源]

* `"alloworigin" <origin> | *`
* 指定可以存取資源之 URI 的 `origin` 參數清單。對於不具備認證的要求，伺服器可以指定 &#42; 作為萬用字元，進而允許任何來源存取該資源。*絕對不建議在生產環境中使用 `Allow-Origin: *`，因為其允許每個外部 (即攻擊者) 網站提出那些若是沒有 CORS 則瀏覽器嚴格禁止的要求。*

#### [!UICONTROL 允許的來源 (Regexp)]

* `"alloworiginregexp" <regexp>`
* 指定可以存取資源之 URI 的 `regexp` 規則運算式清單。*如果建置規則運算式時不夠謹慎，可能會導致意外的符合情況，使得攻擊者能夠使用同樣符合原則的自訂網域。*&#x200B;通常會建議使用 `alloworigin`，為每個特定的來源主機名稱分別訂立原則，即使這表示會重複設定其他原則屬性。不同的來源通常有不同的生命週期和需求，因此明確區隔的原則對其有好處。

#### [!UICONTROL 允許的路徑]

* `"allowedpaths" <regexp>`
* 指定原則適用之來源路徑的 `regexp` 規則運算式清單。

#### [!UICONTROL 公開的標頭]

* `"exposedheaders" <header>`
* 指出允許瀏覽器存取的回應標頭的標頭參數清單。針對 CORS 要求 (非預檢)，若其不為空，這些值會被複製到 `Access-Control-Expose-Headers` 回應標頭。然後清單中的值 (標頭名稱) 便可供瀏覽器存取；若沒有這些值，瀏覽器便無法讀取這些標頭。

#### [!UICONTROL 最長時間]

* `"maxage" <seconds>`
* 指出預檢要求的結果可供快取的時間的 `seconds` 參數。

#### [!UICONTROL 支援的標頭]

* `"supportedheaders" <header>`
* 指出在進行實際要求時可以使用哪些 HTTP 要求標頭的 `header` 參數清單。

#### [!UICONTROL 允許的方法]

* `"supportedmethods"`
* 指出在進行實際要求時可以使用哪些 HTTP 方法的方法參數清單。

#### [!UICONTROL 支援認證]

* `"supportscredentials" <boolean>`
* 指出對於要求之回應是否可以向瀏覽器公開的 `boolean`。用做預檢要求之回應的一部分時，這會指出是否可以使用認證進行實際要求。

### 設定範例

網站 1 為基本的、可匿名存取的唯讀情境，透過 [!DNL GET] 要求使用其中的內容：

```json
{
  "supportscredentials":false,
  "exposedheaders":[
    ""
  ],
  "supportedmethods":[
    "GET",
    "HEAD"
  ],
  "alloworigin":[
    "http://127.0.0.1:3000",
    "https://site1.com"
    
  ],
  "maxage:Integer": 1800,
  "alloworiginregexp":[
    "http://localhost:.*"
    "https://.*\.site1\.com"
  ],
  "allowedpaths":[
    "/content/_cq_graphql/site1/endpoint.json",
    "/graphql/execute.json.*",
    "/content/site1/.*"
  ],
  "supportedheaders":[
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers"
  ]
}
```

網站 2 較為複雜，需要經授權的變更性 (POST、PUT、DELETE) 要求：

```json
{
  "supportscredentials":true,
  "exposedheaders":[
    ""
  ],
  "supportedmethods":[
    "GET",
    "HEAD"
    "POST",
    "DELETE",
    "PUT"
  ],
  "alloworigin":[
    "http://127.0.0.1:3000",
    "https://site2.com"
    
  ],
  "maxage:Integer": 1800,
  "alloworiginregexp":[
    "http://localhost:.*"
    "https://.*\.site2\.com"
  ],
  "allowedpaths":[
    "/content/site2/.*",
    "/libs/granite/csrf/token.json",
  ],
  "supportedheaders":[
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
    "Authorization",
    "CSRF-Token"
  ]
}
```

## Dispatcher 快取關注事項和設定 {#dispatcher-caching-concerns-and-configuration}

從 Dispatcher 4.1.1+ 開始，回應標頭可以被快取。如此一來，只要要求是匿名的，就可以將 [!DNL CORS] 標頭與 [!DNL CORS] 要求的資源一起進行快取。

通常，在 Dispatcher 快取內容的考量，也同樣適用於在 Dispatcher 快取 CORS 回應標頭的情形。以下表格內容定義何時可以快取 [!DNL CORS] 標頭 (因此也包含 [!DNL CORS] 要求)。

| 可快取 | 環境 | 驗證狀態 | 說明 |
|-----------|-------------|-----------------------|-------------|
| 否 | AEM Publish | 經驗證 | AEM Author 上的 Dispatcher 快取僅限於靜態、非製作的資產。因此要在 AEM Author 上快取大多數資源 (包括 HTTP 回應標頭) 皆是困難且不切實際的做法。 |
| 否 | AEM Publish | 經驗證 | 避免快取經驗證要求的 CORS 標頭。這符合避免快取經驗證要求的常見指引，因為很難確定要求使用者的驗證/授權狀態會對所傳遞的資源造成什麼影響。 |
| 是 | AEM Publish | 匿名 | 在 Dispatcher 可以快取的匿名要求，其回應標頭也可以快取，進而確保未來的 CORS 要求可以存取所快取的內容。AEM Publish 上的任何 CORS 設定變更都&#x200B;**必定**&#x200B;會導致受影響的快取資源失效。最佳實務規定在程式碼或設定部署時需清除 Dispatcher 快取，因為很難判定哪些快取內容可能會受到影響。 |

### 允許 CORS 要求標頭

若要允許必要的 [HTTP 要求標頭傳遞至 AEM 進行處理](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=zh-Hant#specifying-the-http-headers-to-pass-through-clientheaders)，必須在 Dispatcher 的 `/clientheaders` 設定中允許這些標頭。

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### 快取 CORS 回應標頭

若要允許在快取內容上快取及提供 CORS 標頭，請將以下 [/cache /headers 設定](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=zh-Hant#caching-http-response-headers)新增至 AEM Publish `dispatcher.any` 檔案中。

```
/publishfarm {
    ...
    /cache {
        ...
        # CORS HTTP response headers
        # https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_response_headers
        /headers {
            ...
            "Access-Control-Allow-Origin"
            "Access-Control-Expose-Headers"
            "Access-Control-Max-Age"
            "Access-Control-Allow-Credentials"
            "Access-Control-Allow-Methods"
            "Access-Control-Allow-Headers"
        }
    ...
    }
...
}
```

請記得在 `dispatcher.any` 檔案進行變更後，**重新啟動網頁伺服器應用程式**。

可能需要完全清除快取，以確保在 `/cache/headers` 設定更新後，在下一個要求中能適當地快取標頭。

## CORS 疑難排解

記錄可於 `com.adobe.granite.cors` 找到：

* 啟用 `DEBUG` 以查看 [!DNL CORS] 要求被拒絕的詳細原因
* 啟用 `TRACE` 以查看 CORS 處理常式處理之所有要求的詳細資訊

### 秘訣：

* 使用 curl 手動重新建立 XHR 要求，但要確保複製所有標頭和詳細資訊，因為每個項目都會產生影響；有些瀏覽器主控台允許複製 curl 命令
* 檢查並確認要求是否被 CORS 處理常式拒絕，而不是被驗證、CSRF 權杖篩選器、Dispatcher 篩選器，或者其他安全性層級所拒絕
   * 若 CORS 處理常式回應 200，但回應中缺少 `Access-Control-Allow-Origin` 標頭，請檢閱 `com.adobe.granite.cors` 中，在 [!DNL DEBUG] 之下的拒絕記錄
* 若 [!DNL CORS] 要求的 Dispatcher 快取已啟用
   * 確保 `/cache/headers` 設定已套用至 `dispatcher.any`，並且網頁伺服器已成功重新啟動
   * 確保在任何 OSGi 或 Dispatcher 設定變更後正確清除快取。
* 如有需要，在要求時檢查是否存在驗證認證。

## 支援素材

* [適用於跨來源資源共用原則的 AEM OSGi 設定工廠](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [跨來源資源共用 (W3C)](https://www.w3.org/TR/cors/)
* [HTTP 存取控制 (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
