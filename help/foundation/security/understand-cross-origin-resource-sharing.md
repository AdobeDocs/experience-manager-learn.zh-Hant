---
title: 瞭解與AEM的跨原始資源共用(CORS)
description: Adobe Experience Manager的跨原始資源共用(CORS)可協助非AEM網頁屬性對AEM進行用戶端呼叫（驗證和未驗證），以擷取內容或直接與AEM互動。
version: 6.3, 6,4, 6.5
sub-product: 基礎，內容服務，網站
feature: null
topics: security, development, content-delivery
activity: understand
audience: architect, developer
doc-type: article
translation-type: tm+mt
source-git-commit: bc14783840a47fb79ddf1876aca1ef44729d097e
workflow-type: tm+mt
source-wordcount: '917'
ht-degree: 1%

---


# 瞭解跨來源資源共用([!DNL CORS])

Adobe Experience Manager的跨原始資源共用([!DNL CORS])可協助非AEM網頁屬性對AEM進行用戶端呼叫（驗證和未驗證），以擷取內容或直接與AEM互動。

## Adobe Granite跨原始資源共用政策OSGi設定

CORS組態在AEM中管理為OSGi組態工廠，每個原則都代表為工廠的一個例項。

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Adobe Granite跨原始資源共用政策OSGi設定](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### 策略選擇

通過比較

* `Allowed Origin` 與請求 `Origin` 標題
* 和`Allowed Paths`。

將使用與這些值匹配的第一個策略。 如果找不到任何[!DNL CORS]請求，則會拒絕。

如果完全未配置策略，則[!DNL CORS]請求也將不會被應答，因為處理程式將被禁用，因而有效地被拒絕——只要伺服器的其他模組沒有響應[!DNL CORS]。

### 策略屬性

#### [!UICONTROL 允許的原始源]

* `"alloworigin" <origin> | *`
* `origin`參數的清單，這些參數指定了可訪問資源的URI。 對於沒有憑證的請求，伺服器可指定*為萬用字元，以允許任何來源存取資源。 *絕對不建議在生產 `Allow-Origin: *` 中使用，因為它可讓每個外國（即攻擊者）網站提出沒有CORS的要求，而瀏覽器嚴格禁止。*

#### [!UICONTROL 允許的原點(Regexp)]

* `"alloworiginregexp" <regexp>`
* `regexp`規則運算式的清單，指定可存取資源的URI。 *規則運算式若未仔細建立，可能會產生非預期的相符項目，讓攻擊者使用自訂網域名稱，並符合原則。* 通常建議對每個特定的源主機名都使用不同的策略，即 `alloworigin`使這意味著重複配置其他策略屬性。不同的起源往往有不同的生命週期和要求，因而從明確的分離中獲益。

#### [!UICONTROL 允許的路徑]

* `"allowedpaths" <regexp>`
* `regexp`規則運算式的清單，指定套用原則的資源路徑。

#### [!UICONTROL 公開的標題]

* `"exposedheaders" <header>`
* 指示允許瀏覽器存取之請求標題的標題參數清單。

#### [!UICONTROL 最大年齡]

* `"maxage" <seconds>`
* `seconds`參數，指出可快取預備要求結果的時間長度。

#### [!UICONTROL 支援的標題]

* `"supportedheaders" <header>`
* `header`參數清單，指出在提出實際請求時可使用哪個HTTP標題。

#### [!UICONTROL 允許的方法]

* `"supportedmethods"`
* 方法參數清單，指出在提出實際請求時可使用哪些HTTP方法。

#### [!UICONTROL 支援認證]

* `"supportscredentials" <boolean>`
* `boolean`，指示對請求的回應是否可公開給瀏覽器。 當用作對預檢請求之回應的一部分時，這表示是否可使用認證來提出實際請求。

### 配置示例

網站1是可匿名存取的基本唯讀藍本，內容透過[!DNL GET]請求使用：

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

## Dispatcher快取問題和配置{#dispatcher-caching-concerns-and-configuration}

從Dispatcher 4.1.1+回應標頭開始，可以快取。 這樣，只要請求是匿名的，就可以將[!DNL CORS]標頭與[!DNL CORS]請求的資源一起快取。

一般而言，在Dispatcher中快取內容的相同考量，可套用至dispatcher中快取CORS回應標頭。 下表定義了可快取[!DNL CORS]標題（以及[!DNL CORS]請求）的時間。

| 可快取 | 環境 | 驗證狀態 | 說明 |
|-----------|-------------|-----------------------|-------------|
| 否 | AEM 發佈 | 已驗證 | AEM Author上的Dispatcher快取限於靜態、非編寫的資產。 這讓您在AEM Author上快取大部分資源（包括HTTP回應標頭）變得困難且不切實際。 |
| 否 | AEM 發佈 | 已驗證 | 避免在已驗證的請求上快取CORS標題。 這與未快取已驗證請求的一般指導一致，因為很難確定請求用戶的驗證／授權狀態將如何影響所傳送的資源。 |
| 是 | AEM 發佈 | 匿名 | Dispatcher的匿名請求可快取，也可快取其回應標頭，以確保未來的CORS請求可存取快取的內容。 AEM Publish **上的任何CORS組態變更必須**，之後才會有受影響的快取資源失效。 最佳做法要求在代碼或配置部署中清除調度程式快取，因為很難確定哪些快取內容可能受到影響。 |

若要允許快取CORS標頭，請將下列組態新增至所有支援的AEM Publish dispatcher.any檔案。

```
/cache { 
  ...
  /clientheaders {
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

在對`dispatcher.any`檔案進行更改後，請記住&#x200B;**重新啟動Web伺服器應用程式**。

在`/clientheaders`組態更新後，可能需要完全清除快取，以確保標頭會在下次要求時正確快取。

## 疑難排解CORS

在`com.adobe.granite.cors`下可使用日誌：

* 啟用`DEBUG`以查看[!DNL CORS]請求被拒絕的詳細資訊
* 啟用`TRACE`以檢視透過CORS處理常式之所有請求的詳細資訊

### 提示：

* 使用捲曲手動重新建立XHR請求，但請務必複製所有標題和詳細資訊，因為每個標題和詳細資訊都可產生影響；某些瀏覽器控制台允許複製curl命令
* 驗證請求是否遭到CORS處理常式拒絕，而非驗證、CSRF Token篩選、分派器篩選或其他安全層拒絕
   * 如果CORS處理常式以200回應，但回應上沒有`Access-Control-Allow-Origin`標題，請檢閱`com.adobe.granite.cors`中[!DNL DEBUG]下的拒絕記錄
* 如果[!DNL CORS]請求的分發程式快取已啟用
   * 確保`/clientheaders`配置已應用於`dispatcher.any`並且Web伺服器已成功重新啟動
   * 確保在任何OSGi或dispatcher.any配置更改後，快取已正確清除。
* 如有需要，請檢查請求上是否有驗證憑證。

## 支援材料

* [AEM OSGi Configuration Factory for Cross-Origin Resource Sharing Policy](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [跨原始資源共用(W3C)](https://www.w3.org/TR/cors/)
* [HTTP存取控制(Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
