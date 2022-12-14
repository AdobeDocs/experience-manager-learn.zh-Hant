---
title: 自訂命名空間
description: 了解如何定義自訂命名空間並部署至AEMas a Cloud Service。
version: Cloud Service
topic: Development, Content Management
feature: Metadata
role: Developer
level: Intermediate
kt: 11618
thumbnail: 3412319.jpg
last-substantial-update: 2022-12-14T00:00:00Z
source-git-commit: 79804ac1ec18f8ac4815bd5ee6f309ef85110cb9
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 3%

---

# 自訂命名空間

了解如何定義和部署自訂 [命名空間](https://developer.adobe.com/experience-manager/reference-materials/spec/jcr/1.0/4.5_Namespaces.html) 到AEMas a Cloud Service。

自訂命名空間是JCR屬性中，在 `:`. AEM使用數個命名空間，例如：

+ `jcr` JCR系統屬性
+ `cq` 適用於AEM(舊稱Adobe CQ)屬性
+ `dam` 適用於DAM Assets的AEM屬性
+ `dc` 都柏林核心屬性

還有其他很多。

命名空間可用來表示屬性的範圍和目的。 建立自訂命名空間（通常是您的公司名稱）有助於清楚識別AEM實作專屬的節點或屬性，並包含您企業專屬的資料。

自訂命名空間可在中管理 [Sling存放庫初始化（重新指向）](https://sling.apache.org/documentation/bundles/repository-initialization.html) 指令碼，並以OSGi設定的形式部署至AEMas a Cloud Service，然後新增至您的 [AEM專案](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=zh-Hant) `ui.config` 專案。

>[!VIDEO](https://video.tv.adobe.com/v/3412319/?quality=12&learn=on)

## 資源

+ [Sling存放庫初始化（重新指向）檔案](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios)

## 程式碼

下列程式碼用於設定 `wknd` 命名空間。

### RepositoryInitializer OSGi配置

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~wknd-examples-namespaces.cfg.json`

```json
{

    "scripts": [
        "register namespace (wknd) https://site.wknd/1.0"
    ]
}
```

這允許使用 `wknd` 命名空間，如 `register namespace` 說明，以用於AEM。 如需進階指令碼定義，請檢閱 [Sling存放庫初始化（重新指向）檔案](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios).
