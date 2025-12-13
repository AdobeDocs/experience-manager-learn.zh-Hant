---
title: 如何啟用CDN快取
description: 瞭解如何在AEM as a Cloud Service的CDN中啟用HTTP回應的快取。
version: Experience Manager as a Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-17T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
exl-id: 544c3230-6eb6-4f06-a63c-f56d65c0ff4b
duration: 174
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '631'
ht-degree: 1%

---

# 如何啟用CDN快取

瞭解如何在AEM as a Cloud Service的CDN中啟用HTTP回應的快取。 回應快取是由`Cache-Control`、`Surrogate-Control`或`Expires` HTTP回應快取標頭所控制。

這些快取標頭通常在使用`mod_headers`的AEM Dispatcher vhost設定中設定，但也可以在AEM Publish本身中執行的自訂Java™程式碼中設定。

## 預設快取行為

當自訂設定不存在時，將使用預設值。 在以下熒幕擷圖中，您可以看到部署[AEM專案原型](https://github.com/adobe/aem-project-archetype)型`mynewsite`AEM專案時，AEM Publish和Author的預設快取行為。

![預設快取行為](../assets/how-to/aem-publish-default-cache-headers.png){width="800" zoomable="yes"}

檢閱[AEM Publish — 預設快取存留期](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/publish.html?lang=zh-Hant#cdn-cache-life)和[AEM Author — 預設快取存留期](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/author.html?lang=zh-Hant&#default-cache-life)以取得詳細資訊。

總而言之，AEM as a Cloud Service會在AEM Publish中快取大部分的內容型別(HTML、JSON、JS、CSS和Assets)，並在AEM Author中快取少數內容型別(JS、CSS)。

## 啟用快取

若要變更預設快取行為，您可以透過兩種方式更新快取標頭。

1. **Dispatcher vhost設定：**&#x200B;僅適用於AEM Publish。
1. **自訂Java™程式碼：**&#x200B;可同時用於AEM Publish和Author。

讓我們檢閱這些選項中的每一個。

### Dispatcher vhost設定

此選項是啟用快取的建議方法，但此選項僅適用於AEM Publish。 若要更新快取標頭，請使用Apache HTTP伺服器的vhost檔案中的`mod_headers`模組和`<LocationMatch>`指示詞。 一般語法如下：

```
<LocationMatch "$URL$ || $URL_REGEX$">
    # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
    Header unset Cache-Control
    Header unset Surrogate-Control
    Header unset Expires

    # Instructs the web browser and CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
    Header set Cache-Control "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX"
    
    # Instructs the CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
    Header set Surrogate-Control "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX"
    
    # Instructs the web browser and CDN to cache the response until the specified date and time.
    Header set Expires "Sun, 31 Dec 2023 23:59:59 GMT"
</LocationMatch>
```

以下摘要說明每個&#x200B;**標頭**&#x200B;的用途以及適用於標頭的&#x200B;**屬性**。

|                     | 網頁瀏覽器 | CDN | 說明 |
|---------------------|:-----------:|:---------:|:-----------:|
| Cache-Control | ✔ | ✔ | 此標題會控制網頁瀏覽器和CDN快取存留期。 |
| Surrogate-Control | ✘ | ✔ | 此標題會控制CDN快取壽命。 |
| 到期時間 | ✔ | ✔ | 此標題會控制網頁瀏覽器和CDN快取存留期。 |


- **max-age**：此屬性會控制回應內容的TTL或「存留時間」（以秒為單位）。
- **stale-while-revalidate**：當收到的要求位於指定的期間（以秒為單位）內時，此屬性會控制CDN層回應內容的&#x200B;_過時狀態_&#x200B;處理。 _過時狀態_&#x200B;是TTL過期之後與重新驗證回應之前的時間段。
- **stale-if-error**：當原始伺服器無法使用，且收到的請求在指定的期間（以秒為單位）內時，此屬性會控制CDN層回應內容的&#x200B;_過時狀態_&#x200B;處理。

如需詳細資訊，請檢閱[失效與重新驗證](https://developer.fastly.com/learning/concepts/edge-state/cache/stale/)詳細資料。

#### 範例

若要將&#x200B;**HTML內容型別**&#x200B;的網頁瀏覽器和CDN快取存留期增加到&#x200B;_10分鐘_，而不處理過時的狀態，請遵循下列步驟：

1. 在您的AEM專案中，從`dispatcher/src/conf.d/available_vhosts`目錄找出所需的vhsot檔案。
1. 更新vhost （例如`wknd.vhost`）檔案，如下所示：

   ```
   <LocationMatch "^/content/.*\.(html)$">
       # Removes the response header if present
       Header unset Cache-Control
   
       # Instructs the web browser and CDN to cache the response for max-age value (600) seconds.
       Header set Cache-Control "max-age=600"
   </LocationMatch>
   ```

   `dispatcher/src/conf.d/enabled_vhosts`目錄中的vhost檔案是&#x200B;**目錄中檔案的** symlink`dispatcher/src/conf.d/available_vhosts`，因此請務必建立symlink （若不存在）。
1. 使用[Cloud Manager - Web層設定管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=zh-Hant&#web-tier-config-pipelines)或[RDE命令](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=zh-Hant#deploy-apache-or-dispatcher-configuration)，將vhost變更部署到所需的AEM as a Cloud Service環境。

不過，若要讓網頁瀏覽器和CDN快取存留期的值不同，您可以使用上述範例中的`Surrogate-Control`標頭。 同樣地，若要在特定日期和時間讓快取到期，您可以使用`Expires`標頭。 此外，使用`stale-while-revalidate`和`stale-if-error`屬性，您可以控制回應內容的過時狀態處理。 AEM WKND專案有[參考過時狀態處理](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L150-L155) CDN快取設定。

同樣地，您也可以更新其他內容型別(JSON、JS、CSS和Assets)的快取標頭。

### 自訂Java™程式碼

此選項同時適用於AEM Publish和Author。 但是，不建議在AEM Author中啟用快取並保留預設快取行為。

若要更新快取標頭，請在自訂Java™程式碼（Sling servlet、Sling servlet篩選器）中使用`HttpServletResponse`物件。 一般語法如下：

```java
// Instructs the web browser and CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
response.setHeader("Cache-Control", "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX");

// Instructs the CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
response.setHeader("Surrogate-Control", "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX");

// Instructs the web browser and CDN to cache the response until the specified date and time.
response.setHeader("Expires", "Sun, 31 Dec 2023 23:59:59 GMT");
```
