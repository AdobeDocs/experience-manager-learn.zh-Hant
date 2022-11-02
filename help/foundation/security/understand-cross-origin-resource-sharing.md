---
title: 了解使用AEM的跨原始資源共用(CORS)
description: Adobe Experience Manager的跨原始資源共用(CORS)方便了非AEM Web屬性向AEM進行用戶端呼叫（驗證和未驗證），以擷取內容或直接與AEM互動。
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
source-git-commit: 2f02a4e202390434de831ce1547001b2cef01562
workflow-type: tm+mt
source-wordcount: '910'
ht-degree: 1%

---

# 了解跨原始資源共用([!DNL CORS])

Adobe Experience Manager的跨原始資源共用([!DNL CORS])可促進非AEM Web屬性向AEM進行用戶端呼叫（驗證和未驗證），以擷取內容或直接與AEM互動。

## AdobeGranite跨原始資源共用原則OSGi設定

在AEM中，CORS設定會以OSGi設定工廠的形式管理，而每個政策會以工廠的一個例項來表示。

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![AdobeGranite跨原始資源共用原則OSGi設定](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### 策略選擇

通過比較

* `Allowed Origin` 和 `Origin` 請求標題
* 和 `Allowed Paths` 和要求路徑。

將使用與這些值匹配的第一個策略。 若找不到任何項目， [!DNL CORS] 請求被拒絕。

如果完全未配置策略， [!DNL CORS] 由於處理程式被禁用，因此會有效地拒絕請求，因此，只要伺服器的其他模組沒有響應 [!DNL CORS].

### 策略屬性

#### [!UICONTROL 允許的原始項]

* `"alloworigin" <origin> | *`
* 清單 `origin` 指定可訪問資源的URI的參數。 對於沒有憑據的請求，伺服器可以指定 &#42; 為萬用字元，因此允許任何來源存取資源。 *絕對不建議使用 `Allow-Origin: *` 在生產環境中，因為它可讓每個外國（即攻擊者）網站提出不含CORS的要求，是瀏覽器嚴格禁止的。*

#### [!UICONTROL 允許的原始項(Regexp)]

* `"alloworiginregexp" <regexp>`
* 清單 `regexp` 規則運算式，指定可能訪問資源的URI。 *如果未仔細構建規則表達式，則可能導致意外匹配，使得攻擊者能夠使用與策略匹配的自定義域名。* 一般建議針對每個特定來源主機名稱分別制定原則，使用 `alloworigin`，即使這意味著重複配置其他策略屬性。 不同的起源往往具有不同的生命週期和要求，因此從明確的分離中受益。

#### [!UICONTROL 允許的路徑]

* `"allowedpaths" <regexp>`
* 清單 `regexp` 規則表達式，指定策略應用的資源路徑。

#### [!UICONTROL 公開的標題]

* `"exposedheaders" <header>`
* 指示瀏覽器可存取之請求標題的標題參數清單。

#### [!UICONTROL 最大年齡]

* `"maxage" <seconds>`
* A `seconds` 指出可快取飛行前請求結果的參數。

#### [!UICONTROL 支援的標題]

* `"supportedheaders" <header>`
* 清單 `header` 參數，指出在提出實際請求時可以使用哪些HTTP標題。

#### [!UICONTROL 允許的方法]

* `"supportedmethods"`
* 方法參數清單，指出在提出實際請求時可使用哪些HTTP方法。

#### [!UICONTROL 支援憑據]

* `"supportscredentials" <boolean>`
* A `boolean` 指出是否可向瀏覽器公開要求的回應。 當用作飛行前請求的回應時，這表示是否可使用憑證來提出實際請求。

### 組態範例

網站1是匿名存取的基本唯讀案例，其中內容透過 [!DNL GET] 要求：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://site1.com]"
    alloworiginregexp="[]"
    allowedpaths="[/content/site1/.*]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers]"
    supportedmethods="[GET]"
    supportscredentials="{Boolean}false"
/>
```

站點2更複雜，需要授權和不安全的請求：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://site2.com]"
    alloworiginregexp="[]"
    allowedpaths="[/content/site2/.*,/libs/granite/csrf/token.json]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers,Authorization,CSRF-Token]"
    supportedmethods="[GET,HEAD,POST,DELETE,OPTIONS,PUT]"
    supportscredentials="{Boolean}true"
/>
```

## Dispatcher快取考量事項和設定 {#dispatcher-caching-concerns-and-configuration}

從Dispatcher 4.1.1+回應標頭開始，可以快取。 這樣便能快取 [!DNL CORS] 標題 [!DNL CORS]-requested資源，只要該請求是匿名的。

一般而言，在Dispatcher快取內容時，相同的考量可套用至Dispatcher快取CORS回應標題。 下表定義 [!DNL CORS] 標題(因此 [!DNL CORS] 請求)。

| 可快取 | 環境 | 驗證狀態 | 解釋 |
|-----------|-------------|-----------------------|-------------|
| 否 | AEM 發佈 | 已驗證 | AEM Author上的Dispatcher快取限於靜態、非製作的資產。 這樣就很難且不切實際地在AEM作者上快取大部分資源，包括HTTP回應標題。 |
| 否 | AEM 發佈 | 已驗證 | 避免在已驗證的請求上快取CORS標題。 這符合未快取已驗證請求的一般指引，因為很難判斷要求使用者的驗證/授權狀態將如何影響傳送的資源。 |
| 是 | AEM 發佈 | 匿名 | 在Dispatcher可快取的匿名請求也可以快取其回應標頭，以確保未來的CORS請求可存取快取的內容。 AEM發佈上的任何CORS設定變更 **必須** 之後，受影響之快取資源的失效。 最佳實務是要清除調度程式快取的程式碼或設定部署，因為很難判斷會影響哪些快取內容。 |

若要允許快取CORS標頭，請將下列設定新增至所有支援的AEM Publish dispatcher.any檔案。

```
/cache { 
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

記住 **重新啟動web伺服器應用程式** 變更 `dispatcher.any` 檔案。

可能需要完全清除快取，以確保在 `/cache/headers` 設定更新。

## 疑難排解CORS

記錄功能可在 `com.adobe.granite.cors`:

* 啟用 `DEBUG` 若要查看 [!DNL CORS] 請求被拒絕
* 啟用 `TRACE` 若要查看所有要求的詳細資訊，請透過CORS處理常式

### 提示：

* 使用curl手動重新建立XHR請求，但請務必複製所有標題和詳細資訊，因為每個標題和詳細資訊都可能有所差異；某些瀏覽器主控台可複製curl命令
* 驗證請求是否被CORS處理常式拒絕，而不是由驗證、CSRF代號篩選器、Dispatcher篩選器或其他安全層拒絕
   * 如果CORS處理常式有200回應，但 `Access-Control-Allow-Origin` 回應上沒有標題，請檢閱下方的拒絕記錄 [!DNL DEBUG] in `com.adobe.granite.cors`
* 如果Dispatcher快取 [!DNL CORS] 請求已啟用
   * 確保 `/cache/headers` 設定已套用至 `dispatcher.any` 並且已成功重新啟動Web伺服器
   * 在任何OSGi或dispatcher.any設定變更後，請確定已正確清除快取。
* 如果需要，請檢查請求上是否存在身份驗證憑據。

## 支援材料

* [AEM OSGi跨原始資源共用原則的設定工廠](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [跨原始資源共用(W3C)](https://www.w3.org/TR/cors/)
* [HTTP存取控制(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
