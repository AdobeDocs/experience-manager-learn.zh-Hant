---
title: 如何停用CDN快取
description: 瞭解如何在AEMas a Cloud Service的CDN中停用HTTP回應的快取。
version: Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-30T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
exl-id: 22b1869e-5bb5-437d-9cb5-2d27f704c052
source-git-commit: 783f84c821ee9f94c2867c143973bf8596ca6437
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 0%

---

# 如何停用CDN快取

瞭解如何在AEMas a Cloud Service的CDN中停用HTTP回應的快取。 回應的快取是由所控制 `Cache-Control`， `Surrogate-Control`，或 `Expires` HTTP回應快取標題。

這些快取標頭通常是在AEM Dispatcher vhost設定中使用 `mod_headers`，但也可以在AEM Publish本身執行的自訂Java™程式碼中設定。

## 預設快取行為

檢閱AEM Publish和Author的預設快取行為(若 [AEM專案原型](./enable-caching.md#default-caching-behavior) 基礎的AEM專案已部署。

## 停用快取

關閉快取可能會對AEMas a Cloud Service執行個體的效能產生負面影響，因此在關閉預設快取行為時請務必謹慎。

不過，在某些情況下，您可能會想要停用快取，例如：

- 正在開發新功能，並且想要立即看到變更。
- 內容安全（僅供驗證的使用者使用）或動態（購物車、訂單詳細資料）且不應快取。

若要停用快取，您可以使用兩種方式更新快取標頭。

1. **Dispatcher vhost設定：** 僅適用於AEM Publish。
1. **自訂Java™程式碼：** 可供AEM Publish和Author使用。

讓我們檢閱這些選項中的每一個。

### Dispatcher vhost設定

此選項是停用快取的建議方法，但僅適用於AEM Publish。 若要更新快取標題，請使用 `mod_headers` 模組和 `<LocationMatch>` 指示詞。 一般語法如下：

```
<LocationMatch "$URL$ || $URL_REGEX$">
    # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
    Header unset Cache-Control
    Header unset Expires

    # Instructs the CDN to not cache the response.
    Header set Cache-Control "private"
</LocationMatch>
```

#### 範例

若要停用 **CSS內容型別** 如需疑難排解，請依照下列步驟進行。

請注意，若要略過現有的CSS快取，必須變更CSS檔案，才能為CSS檔案產生新的快取索引鍵。

1. 在您的AEM專案中，從找到所需的影片檔案 `dispatcher/src/conf.d/available_vhosts` 目錄。
1. 更新vhost (例如 `wknd.vhost`)檔案，如下所示：

   ```
   <LocationMatch "^/etc.clientlibs/.*\.(css)$">
       # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
       Header unset Cache-Control
       Header unset Expires
   
       # Instructs the CDN to not cache the response.
       Header set Cache-Control "private"
   </LocationMatch>
   ```

   中的vhost檔案 `dispatcher/src/conf.d/enabled_vhosts` 目錄為 **符號連結** 至中的檔案 `dispatcher/src/conf.d/available_vhosts` 目錄，因此如果沒有，請務必建立符號連結。
1. 使用將vhost變更部署到所需的AEMas a Cloud Service環境 [Cloud Manager — 網頁層設定管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) 或 [RDE命令](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration).

### 自訂Java™程式碼

此選項同時適用於AEM Publish和Author。 若要更新快取標題，請使用 `SlingHttpServletResponse` 自訂Java™程式碼中的物件（Sling servlet、Sling servlet篩選器）。 一般語法如下：

```java
response.setHeader("Cache-Control", "private");
```
