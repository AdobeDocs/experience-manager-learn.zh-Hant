---
title: AEM發佈服務快取
description: AEMas a Cloud Service發佈服務快取的一般總覽。
version: Cloud Service
feature: Dispatcher, Developer Tools
topic: Performance
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
exl-id: 1a1accbe-7706-4f9b-bf63-755090d03c4c
duration: 325
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1134'
ht-degree: 1%

---

# AEM 發佈

AEM Publish服務有兩個主要快取層，即AEMas a Cloud ServiceCDN和AEM Dispatcher。 您可以選擇將客戶管理的CDN放在AEMas a Cloud ServiceCDN前。 AEMas a Cloud ServiceCDN提供內容邊緣交付，確保為全球的使用者提供低延遲的體驗。 AEM Dispatcher會直接在AEM Publish之前提供快取，並用來減少AEM Publish本身不必要的負載。

![AEM發佈快取概觀圖表](./assets/publish/publish-all.png){align="center"}

## CDN

AEMas a Cloud Service的CDN快取是由HTTP回應快取標題所控制，其目的在於快取內容以最佳化新鮮度與效能之間的平衡。 CDN位於一般使用者和AEM Dispatcher之間，用於快取儘可能接近一般使用者的內容，以確保高效能體驗。

![AEM發佈CDN](./assets/publish/publish-cdn.png){align="center"}

設定CDN快取內容的方式僅限於在HTTP回應上設定快取標題。 這些快取標頭通常是在AEM Dispatcher vhost設定中使用 `mod_headers`，但也可以在AEM Publish本身執行的自訂Java™程式碼中設定。

### 何時會快取HTTP要求/回應？

AEMas a Cloud ServiceCDN只會快取HTTP回應，且必須符合下列所有條件：

+ HTTP要求狀態為 `2xx` 或 `3xx`
+ HTTP要求方法為 `GET` 或 `HEAD`
+ 至少存在下列其中一個HTTP回應標題： `Cache-Control`， `Surrogate-Control`，或  `Expires`
+ HTTP回應可以是任何內容型別，包括HTML、JSON、CSS、JS和二進位檔案。

依預設，不會快取HTTP回應 [AEM傳送器](#aem-dispatcher) 自動移除任何HTTP回應快取標題，以避免CDN的快取。 只要仔細覆寫此行為，可使用 `mod_headers` 使用 `Header always set ...` 指示詞。

### 快取的內容？

AEMas a Cloud ServiceCDN快取下列專案：

+ HTTP回應內文
+ HTTP回應標題

通常，單一URL的HTTP請求/回應會快取為單一物件。 不過，CDN可以在以下情況下處理單一URL的多個物件的快取： `Vary` 標頭是在HTTP回應上設定。 避免指定 `Vary` 在值沒有嚴格控制值集的標頭上，因為這可能會導致許多快取遺漏，降低快取命中率。 若要支援在AEM Dispatcher快取各種請求， [檢閱變體快取檔案](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/advanced/variant-caching.html).

### 快取存留期{#cdn-cache-life}

AEM Publish CDN是以TTL （存留時間）為基礎，這表示快取存留期是由 `Cache-Control`， `Surrogate-Control`，或 `Expires` HTTP回應標頭。 如果專案未設定HTTP回應快取標頭，而 [適用性條件](#when-are-http-requestsresponses-cached) 若符合，Adobe設定的預設快取存留期為10分鐘（600秒）。

以下是快取標題如何影響CDN快取壽命：

+ [`Cache-Control`](https://developer.fastly.com/reference/http/http-headers/Cache-Control/) HTTP回應標題會指示網頁瀏覽器和CDN快取回應的時間長度。 該值以秒為單位。 例如， `Cache-Control: max-age=3600` 告訴網頁瀏覽器快取一小時的回應。 CDN會忽略此值，如果 `Surrogate-Control` HTTP回應標頭也會出現。
+ [`Surrogate-Control`](https://developer.fastly.com/reference/http/http-headers/Surrogate-Control/) HTTP回應標題會指示AEM CDN快取回應的時間長度。 該值以秒為單位。 例如， `Surrogate-Control: max-age=3600` 告訴CDN將回應快取一小時。
+ [`Expires`](https://developer.fastly.com/reference/http/http-headers/Expires/) HTTP回應標頭會指示AEM CDN （和網頁瀏覽器）快取回應有效的時間。 值為日期。 例如， `Expires: Sat, 16 Sept 2023 09:00:00 EST` 告訴網頁瀏覽器快取回應，直到指定的日期和時間為止。

使用 `Cache-Control` 在瀏覽器和CDN的快取期限相同時控制快取期限。 使用 `Surrogate-Control` 當網頁瀏覽器應該在與CDN不同的期間內快取回應。

#### 預設快取存留期

如果HTTP回應符合AEM Dispatcher快取的資格 [每個上述限定詞](#when-are-http-requestsresponses-cached)，則以下為預設值，除非有自訂設定。

| 內容類型 | 預設CDN快取期限 |
|:------------ |:---------- |
| [HTML/JSON/XML](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#html-text) | 5 分鐘 |
| [資產（影像、影片、檔案等）](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#images) | 10 分鐘 |
| [持久查詢(JSON)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?publish-instances) | 2 小時 |
| [使用者端資料庫(JS/CSS)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries) | 30 天 |
| [其他](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#other-content) | 未快取 |

### 如何自訂快取規則

[設定CDN快取內容的方式](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#disp) 僅限於在HTTP回應上設定快取標題。 這些快取標頭通常設定在AEM Dispatcher中 `vhost` 設定，使用 `mod_headers`，但也可以在AEM Publish本身執行的自訂Java™程式碼中設定。

## AEM Dispatcher

![AEM發佈AEM Dispatcher](./assets/publish/publish-dispatcher.png){align="center"}

### 何時會快取HTTP要求/回應？

當滿足以下所有條件時，會快取對應HTTP請求的HTTP回應：

+ HTTP要求方法為 `GET` 或 `HEAD`
   + `HEAD` HTTP要求只會快取HTTP回應標題。 它們沒有回應內文。
+ HTTP回應狀態為 `200`
+ HTTP回應不適用於二進位檔案。
+ HTTP要求URL路徑以副檔名結尾，例如： `.html`， `.json`， `.css`， `.js`等
+ HTTP要求不包含授權，且未由AEM驗證。
   + 不過，快取已驗證的請求 [可全域啟用](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#caching-when-authentication-is-used) 或選擇性地透過 [許可權敏感型快取](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/permissions-cache.html).
+ HTTP要求不包含查詢引數。
   + 但是，設定 [忽略的查詢引數](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#ignoring-url-parameters) 允許從快取中快取/提供包含已忽略查詢引數的HTTP請求。
+ HTTP要求的路徑 [符合允許Dispatcher規則，但不符合拒絕規則](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#specifying-the-documents-to-cache).
+ HTTP回應沒有下列AEM Publish設定的HTTP回應標頭：

   + `no-cache`
   + `no-store`
   + `must-revalidate`

### 快取的內容？

AEM Dispatcher快取以下專案：

+ HTTP回應內文
+ Dispatcher的 [快取標頭設定](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#caching-http-response-headers). 檢視附帶的預設設定 [AEM專案原型](https://github.com/adobe/aem-project-archetype/blob/develop/src/main/archetype/dispatcher.cloud/src/conf.dispatcher.d/available_farms/default.farm#L106-L113).
   + `Cache-Control`
   + `Content-Disposition`
   + `Content-Type`
   + `Expires`
   + `Last-Modified`
   + `X-Content-Type-Options`

### 快取存留期

AEM Dispatcher會使用下列方法快取HTTP回應：

+ 直到透過如發佈或取消發佈內容等機制觸發失效為止。
+ TTL （存留時間），明確 [已在Dispatcher設定中設定](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#configuring-time-based-cache-invalidation-enablettl). 請參閱中的預設設定 [AEM專案原型](https://github.com/adobe/aem-project-archetype/blob/develop/src/main/archetype/dispatcher.cloud/src/conf.dispatcher.d/available_farms/default.farm#L122-L127) 藉由檢閱 `enableTTL` 設定。

#### 預設快取存留期

如果HTTP回應符合AEM Dispatcher快取的資格 [每個上述限定詞](#when-are-http-requestsresponses-cached-1)，則以下為預設值，除非有自訂設定。

| 內容類型 | 預設CDN快取期限 |
|:------------ |:---------- |
| [HTML/JSON/XML](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#html-text) | 直到失效 |
| [資產（影像、影片、檔案等）](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#images) | 永不 |
| [持久查詢(JSON)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?publish-instances) | 1分鐘 |
| [使用者端資料庫(JS/CSS)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries) | 30 天 |
| [其他](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#other-content) | 直到失效 |

### 如何自訂快取規則

AEM Dispatcher的快取可透過以下方式設定： [Dispatcher設定](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache) 包括：

+ 快取的內容
+ 發佈/取消發佈時快取的哪些部分會失效
+ 評估快取時會忽略哪些HTTP要求查詢引數
+ 快取哪些HTTP回應標題
+ 啟用或停用TTL快取
+ ...等等

使用 `mod_headers` 若要設定快取標頭 `vhost` 設定不會影響Dispatcher快取（以TTL為基礎），因為這些會在AEM Dispatcher處理回應後新增到HTTP回應。 若要透過HTTP回應標頭影響Dispatcher快取，需要在AEM Publish中執行並設定適當HTTP回應標頭的自訂Java™程式碼。
