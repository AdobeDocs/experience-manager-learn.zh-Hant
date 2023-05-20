---
title: 瞭解跨源資源共用(CORS)AEM
description: Adobe Experience Manager的跨源資源共用(CORS)方便了非AEMWeb屬性，使客戶端調用（無論經過驗證還是未經驗證）AEM獲取內容或直接與之交AEM互。
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
topics: security, development, content-delivery
activity: understand
audience: architect, developer
doc-type: article
topic: Security
role: Developer
level: Intermediate
exl-id: 6009d9cf-8aeb-4092-9e8c-e2e6eec46435
source-git-commit: 2bd1b66dc28a6e591afda746e9d276cae7a29948
workflow-type: tm+mt
source-wordcount: '913'
ht-degree: 1%

---

# 瞭解跨源資源共用([!DNL CORS])

Adobe Experience Manager的跨源資源共用(I)[!DNL CORS])方便了AEM非Web屬性，使客戶端調用(驗證和未驗AEM證)獲取內容或直接與交互AEM。

## Adobe花崗岩跨源資源共用策略OSGi配置

CORS配置在中作為OSGi配置工廠進行AEM管理，每個策略都表示為工廠的一個實例。

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Adobe花崗岩跨源資源共用策略OSGi配置](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### 策略選擇

通過比較

* `Allowed Origin` 和 `Origin` 請求標題
* 和 `Allowed Paths` 和請求路徑。

使用與這些值匹配的第一個策略。 如果找不到，則 [!DNL CORS] 請求被拒絕。

如果根本未配置策略， [!DNL CORS] 由於處理程式被禁用，因此會被有效拒絕，因此也不會回答請求 — 只要伺服器中沒有其他模組響應 [!DNL CORS]。

### 策略屬性

#### [!UICONTROL 允許的源]

* `"alloworigin" <origin> | *`
* 清單 `origin` 參數指定可訪問資源的URI。 對於沒有憑據的請求，伺服器可以指定 &#42; 作為通配符，從而允許任何源訪問資源。 *絕對不建議使用 `Allow-Origin: *` 因為它允許每個外國（即攻擊者）網站發出沒有CORS的請求，瀏覽器嚴格禁止這些請求。*

#### [!UICONTROL 允許的源(Regexp)]

* `"alloworiginregexp" <regexp>`
* 清單 `regexp` 指定可訪問資源的URI的規則運算式。 *如果不精心構建規則運算式，則可能導致意外匹配，使得攻擊者能夠使用與策略相匹配的自定義域名。* 通常建議對每個特定的源主機名使用單獨的策略 `alloworigin`，即使這意味著重複配置其他策略屬性。 不同的起源往往具有不同的生命週期和要求，因而從明確的分離中受益。

#### [!UICONTROL 允許的路徑]

* `"allowedpaths" <regexp>`
* 清單 `regexp` 規則運算式指定策略所應用的資源路徑。

#### [!UICONTROL 暴露的標頭]

* `"exposedheaders" <header>`
* 指示允許瀏覽器訪問的請求標頭的標頭參數清單。

#### [!UICONTROL 最大使用期限]

* `"maxage" <seconds>`
* A `seconds` 參數，指示可快取飛行前請求的結果的時間。

#### [!UICONTROL 支援的標頭]

* `"supportedheaders" <header>`
* 清單 `header` 參數，指示在發出實際請求時可以使用哪些HTTP標頭。

#### [!UICONTROL 允許的方法]

* `"supportedmethods"`
* 指示在發出實際請求時可以使用哪些HTTP方法的方法參數清單。

#### [!UICONTROL 支援憑據]

* `"supportscredentials" <boolean>`
* A `boolean` 指示是否可以向瀏覽器顯示對請求的響應。 當用作對飛行前請求的響應的一部分時，這表示是否可以使用憑據來發出實際請求。

### 配置示例

站點1是基本的、可匿名訪問的只讀方案，內容通過 [!DNL GET] 請求：

```json
{
  "supportscredentials":false,
  "exposedheaders":[
    ""
  ],
  "supportedmethods":[
    "GET",
    "HEAD",
    "OPTIONS"
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
    "Access-Control-Request-Headers",
  ]
}
```

站點2更複雜，需要授權和變異(POST、PUT、DELETE)請求：

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
    "OPTIONS",
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

## 調度程式快取問題和配置 {#dispatcher-caching-concerns-and-configuration}

可以快取從Dispatcher 4.1.1+響應標頭開始。 這使得可以快取 [!DNL CORS] 標題 [!DNL CORS] — 請求的資源，只要該請求是匿名的。

通常，在Dispatcher上快取內容的相同注意事項可以應用於在Dispatcher上快取CORS響應標頭。 下表定義 [!DNL CORS] 標題(因此 [!DNL CORS] 請求)可快取。

| 可快取 | 環境 | 驗證狀態 | 解釋 |
|-----------|-------------|-----------------------|-------------|
| 否 | AEM 發佈 | 已驗證 | AEM作者上的Dispatcher快取限於靜態、非創作的資產。 這使得在AEM作者上快取大多數資源（包括HTTP響應標頭）變得困難和不切實際。 |
| 否 | AEM 發佈 | 已驗證 | 避免在經過身份驗證的請求上快取CORS標頭。 這與未快取經過驗證的請求的通用指導一致，因為很難確定請求用戶的驗證/授權狀態將如何影響所傳送的資源。 |
| 是 | AEM 發佈 | 匿名 | 調度程式處可快取的匿名請求也可以快取其響應頭，從而確保將來的CORS請求可以訪問快取的內容。 AEM發佈上的任何CORS配置更改 **必須** 隨後受影響的快取資源失效。 最佳做法要求在代碼或配置部署中清除調度程式快取，因為很難確定可能會影響哪些快取內容。 |

要允許快取CORS標頭，請將以下配置添加到所有支援的AEM Publish Dispatcher.any檔案。

```
/myfarm { 
  ...
  /headers {
      "Origin"
      "Access-Control-Allow-Origin"
      "Access-Control-Expose-Headers"
      "Access-Control-Max-Age"
      "Access-Control-Allow-Credentials"
      "Access-Control-Allow-Methods"
      "Access-Control-Allow-Headers"
  }
  ...
}
```

記住 **重新啟動web伺服器應用程式** 對 `dispatcher.any` 的子菜單。

可能需要完全清除快取，以確保在下次請求後，報頭被適當快取 `/cache/headers` 配置更新。

## 排除CORS故障

日誌記錄可在 `com.adobe.granite.cors`:

* 啟用 `DEBUG` 查看有關 [!DNL CORS] 請求被拒絕
* 啟用 `TRACE` 查看有關通過CORS處理程式的所有請求的詳細資訊

### 提示：

* 使用curl手動重新建立XHR請求，但確保複製所有標題和詳細資訊，因為每個標題和詳細資訊都可以帶來不同；某些瀏覽器控制台允許複製curl命令
* 驗證請求是否被CORS處理程式拒絕，而不是被驗證、 CSRF令牌篩選器、調度程式篩選器或其他安全層拒絕
   * 如果CORS處理程式以200響應，但 `Access-Control-Allow-Origin` 回應中缺少標頭，請查看下面的拒絕日誌 [!DNL DEBUG] 在 `com.adobe.granite.cors`
* 如果調度程式快取 [!DNL CORS] 請求已啟用
   * 確保 `/cache/headers` 配置應用於 `dispatcher.any` 並且Web伺服器已成功重新啟動
   * 確保在任何OSGi或dispatcher.any配置更改後正確清除快取。
* 如果需要，檢查請求上是否存在身份驗證憑據。

## 支撐材料

* [OSGiAEM跨源資源共用策略配置工廠](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [跨源資源共用(W3C)](https://www.w3.org/TR/cors/)
* [HTTP訪問控制(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
