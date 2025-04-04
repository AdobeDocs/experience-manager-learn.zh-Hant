---
title: 如何停用CDN快取
description: 瞭解如何在 AEM 中停用 HTTP 回應快取為Cloud Service的 CDN。
version: Experience Manager as a Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-30T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
exl-id: 22b1869e-5bb5-437d-9cb5-2d27f704c052
duration: 100
source-git-commit: a98ca7ddc155190b63664239d604d11ad470fdf5
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 0%

---

# 如何停用CDN快取

瞭解如何在AEM as a Cloud Service的CDN中停用HTTP回應的快取。 回應快取是由`Cache-Control`、`Surrogate-Control`或`Expires` HTTP回應快取標頭所控制。

這些快取標頭通常在使用`mod_headers`的AEM Dispatcher vhost設定中設定，但也可以在AEM Publish本身中執行的自訂Java™程式碼中設定。

## 預設快取行為

AEM as a Cloud Service CDN中HTTP回應的快取是由以下來自來源`Cache-Control`、`Surrogate-Control`或`Expires`的HTTP回應標頭所控制。  不快取`Cache-Control`中包含`private`、`no-cache`或`no-store`的原始回應。

部署以AEM專案原型為基礎的AEM專案時，檢閱AEM Publish和Author的[預設快取行為](./enable-caching.md#default-caching-behavior)。


## 停用快取

關閉快取可能會對AEM as a Cloud Service執行個體的效能產生負面影響，因此在關閉預設快取行為時請務必謹慎。

不過，在某些情況下，您可能需要停用快取，例如：

- 開發新功能並希望立即看到更改。
- 内容是安全的（僅適用於經過身份驗證的使用者）或動態的（購物車、訂單詳細資訊），不應緩存。

若要停用快取，您可以使用兩種方式更新快取標頭。

1. **Dispatcher vhost設定：**&#x200B;僅適用於AEM Publish。
1. **自訂Java™程式碼：**&#x200B;可同時用於AEM Publish和Author。

讓我們檢閱這些選項中的每一個。

### Dispatcher vhost設定

此選項是停用快取的建議方法，但此選項僅適用於AEM Publish。 若要更新快取標頭，請使用Apache HTTP伺服器的vhost檔案中的`mod_headers`模組和`<LocationMatch>`指示詞。 一般語法如下：

```
<LocationMatch "$URL$ || $URL_REGEX$">
    # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
    Header unset Cache-Control
    Header unset Surroagate-Control
    Header unset Expires

    # Instructs the Browser and the CDN to not cache the response.
    Header always set Cache-Control "private"

    # Instructs only the CDN to not cache the response.
    Header always set Surrogate-Control "private"
</LocationMatch>
```

#### 範例

若要出於某些故障排除目的禁用 CSS 內容類型的&#x200B;**CDN 快取**，追隨以下步驟。

請注意，要繞過現有的 CSS 快取，需要更改 CSS 檔才能為 CSS 檔生成新的緩存鍵。

1. 在 AEM 專案中，從 `dispatcher/src/conf.d/available_vhosts` 目錄中找到所需的 vhsot 檔。
1. 更新vhost （例如`wknd.vhost`）檔案，如下所示：

   ```
   <LocationMatch "^/etc.clientlibs/.*\.(css)$">
       # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
       Header unset Cache-Control
       Header unset Expires
   
       # Instructs the Browser and the CDN to not cache the response.
       Header always set Cache-Control "private"
   </LocationMatch>
   ```

   `dispatcher/src/conf.d/enabled_vhosts`目錄中的vhost檔案是`dispatcher/src/conf.d/available_vhosts`目錄中檔案的&#x200B;**symlink**，因此請務必建立symlink （若不存在）。
1. 使用[Cloud Manager - Web層設定管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines)或[RDE命令](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration)，將vhost變更部署到所需的AEM as a Cloud Service環境。

### 自訂Java™程式碼

此選項同時適用於AEM Publish和Author。 若要更新快取標頭，請在自訂Java™程式碼（Sling servlet、Sling servlet篩選器）中使用`SlingHttpServletResponse`物件。 一般語法如下：

```java
response.setHeader("Cache-Control", "private");
```
