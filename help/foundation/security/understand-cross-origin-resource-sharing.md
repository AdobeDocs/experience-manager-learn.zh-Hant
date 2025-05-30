---
title: 瞭解使用AEM的跨原始資源共用(CORS)
description: Adobe Experience Manager的跨原始資源共用(CORS)可協助非AEM網頁屬性對AEM發出使用者端呼叫（包括已驗證和未驗證的呼叫），以擷取內容或直接與AEM互動。
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
workflow-type: tm+mt
source-wordcount: '1011'
ht-degree: 1%

---

# 瞭解跨原始資源共用([!DNL CORS])

Adobe Experience Manager的跨原始資源共用([!DNL CORS])可協助非AEM Web屬性對AEM進行使用者端呼叫（不論已驗證或未驗證），以擷取內容或直接與AEM互動。

本檔案中概述的OSGI設定足以用於：

1. AEM發佈上的單一來源資源共用
2. AEM作者的CORS存取權

如果AEM Publish上需要多來源CORS存取權，請參閱[此檔案](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=zh-Hant#dispatcher-configuration)。

## Adobe Granite跨原始資源共用原則OSGi設定

CORS設定在AEM中作為OSGi設定處理站進行管理，而每個原則都會表示為處理站的一個執行個體。

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Adobe Granite跨原始資源共用原則OSGi設定](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### 原則選擇

原則是透過比較

* 具有`Origin`請求標頭的`Allowed Origin`
* 和`Allowed Paths` （含要求路徑）。

系統會使用符合這些值的第一個原則。 如果找不到任何專案，則會拒絕任何[!DNL CORS]要求。

如果完全未設定原則，則不會回應[!DNL CORS]要求，因為處理常式已停用，因此會有效拒絕（只要伺服器的其他模組沒有回應[!DNL CORS]）。

### 原則屬性

#### [!UICONTROL 允許的來源]

* `"alloworigin" <origin> | *`
* `origin`引數的清單，指定可存取資源的URI。 對於沒有認證的請求，伺服器可以指定&#42;作為萬用字元，從而允許任何來源存取資源。 *絕對不建議在生產環境中使用`Allow-Origin: *`，因為它允許所有外國（即攻擊者）網站提出瀏覽器嚴禁不含CORS的要求。*

#### [!UICONTROL 允許的來源(Regexp)]

* `"alloworiginregexp" <regexp>`
* 指定可存取資源之URI的`regexp`規則運算式清單。 *規則運算式若未小心建置，可能會造成非預期的相符專案，讓攻擊者使用也會符合原則的自訂網域名稱。*&#x200B;通常建議使用`alloworigin`為每個特定的原始主機名稱設定不同的原則，即使這表示重複設定其他原則屬性。 不同的起源往往具有不同的生命週期和要求，因此有利於清晰的分離效果。

#### [!UICONTROL 允許的路徑]

* `"allowedpaths" <regexp>`
* 指定原則套用之資源路徑的`regexp`規則運算式清單。

#### [!UICONTROL 公開的標頭]

* `"exposedheaders" <header>`
* 標頭引數清單，指示瀏覽器可存取的回應標頭。 若為CORS請求（非預檢），若非空白，這些值會複製到`Access-Control-Expose-Headers`回應標頭。 之後，瀏覽器即可存取清單中的值（標頭名稱）；若沒有清單，瀏覽器就無法讀取這些標頭。

#### [!UICONTROL 最大年齡]

* `"maxage" <seconds>`
* `seconds`引數，指出可快取預檢要求結果的時間長度。

#### [!UICONTROL 支援的標頭]

* `"supportedheaders" <header>`
* `header`引數的清單，指出可在發出實際請求時使用哪些HTTP請求標頭。

#### [!UICONTROL 允許的方法]

* `"supportedmethods"`
* 方法引數清單，指示提出實際要求時可以使用哪些HTTP方法。

#### [!UICONTROL 支援認證]

* `"supportscredentials" <boolean>`
* 指出對要求的回應是否可向瀏覽器公開的`boolean`。 當用作預檢要求回應的一部分時，這表示是否可以使用認證提出實際要求。

### 設定範例

網站1是基本、匿名存取、唯讀的案例，透過[!DNL GET]請求使用內容：

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

網站2較為複雜，且需要授權和變體(POST、PUT、DELETE)請求：

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

## Dispatcher快取疑慮與設定 {#dispatcher-caching-concerns-and-configuration}

從Dispatcher 4.1.1+回應標頭開始，即可進行快取。 只要要求為匿名，這樣就可以隨同[!DNL CORS]要求的資源快取[!DNL CORS]標頭。

一般而言，在Dispatcher快取內容的考量也適用於在Dispatcher快取CORS回應標頭。 下表定義何時可以快取[!DNL CORS]個標題（以及[!DNL CORS]個請求）。

| 可快取 | 環境 | 驗證狀態 | 解釋 |
|-----------|-------------|-----------------------|-------------|
| 否 | AEM 發佈 | 已驗證 | AEM Author上的Dispatcher快取僅限於靜態的非編寫資產。 這使得在AEM Author上快取大部分資源（包括HTTP回應標頭）變得困難和不實際。 |
| 否 | AEM 發佈 | 已驗證 | 避免在已驗證的請求上快取CORS標頭。 這符合不快取已驗證請求的常見指南，因為很難判斷請求使用者的驗證/授權狀態將如何影響傳送的資源。 |
| 是 | AEM 發佈 | 匿名 | Dispatcher中可快取的匿名請求也可以快取其回應標頭，以確保未來的CORS請求可以存取快取的內容。 在AEM發佈&#x200B;**上進行的任何CORS組態變更都必須**，之後才會讓受影響的快取資源失效。 最佳實務指示程式碼或設定部署會清除Dispatcher快取，因為很難判斷哪些快取內容可能會受到影響。 |

### 允許CORS要求標頭

若要允許必要的[HTTP要求標頭傳遞至AEM進行處理](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=zh-Hant#specifying-the-http-headers-to-pass-through-clientheaders)，Dispatcher的`/clientheaders`設定中必須允許這些標頭。

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### 快取CORS回應標頭

若要允許在快取的內容上快取及提供CORS標頭，請將下列[/cache /headers設定](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=zh-Hant#caching-http-response-headers)新增至AEM發佈`dispatcher.any`檔案。

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

請記得在變更`dispatcher.any`檔案之後&#x200B;**重新啟動Web伺服器應用程式**。

可能需要完全清除快取，以確保在`/cache/headers`設定更新後，在下一個請求時適當地快取標題。

## 疑難排解CORS

記錄可在`com.adobe.granite.cors`下使用：

* 啟用`DEBUG`以檢視有關[!DNL CORS]請求被拒絕原因的詳細資料
* 啟用`TRACE`以檢視所有透過CORS處理常式處理的請求的詳細資料

### 提示：

* 使用curl手動重新建立XHR請求，但請務必複製所有標題和詳細資訊，因為每個標題和詳細資訊都可能有所不同；有些瀏覽器主控台允許複製curl命令
* 驗證請求是否被CORS處理常式拒絕，而不是被驗證、CSRF權杖篩選器、Dispatcher篩選器或其他安全性層拒絕
   * 如果CORS處理常式以200回應，但回應中沒有`Access-Control-Allow-Origin`標頭，請檢閱`com.adobe.granite.cors`中[!DNL DEBUG]底下的拒絕記錄檔
* 如果已啟用[!DNL CORS]請求的Dispatcher快取
   * 請確定`/cache/headers`設定已套用至`dispatcher.any`，且網頁伺服器已成功重新啟動
   * 請確保在任何OSGi或dispatcher.any設定變更後已正確清除快取。
* 如果需要，請檢查請求中是否存在驗證認證。

## 支援材料

* [跨原始資源共用原則的AEM OSGi Configuration Factory](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [跨原始資源共用(W3C)](https://www.w3.org/TR/cors/)
* [HTTP存取控制(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
