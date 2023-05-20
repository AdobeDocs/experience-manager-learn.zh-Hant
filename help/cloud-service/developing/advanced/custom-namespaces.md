---
title: 自定義命名空間
description: 瞭解如何定義自定義命名空間並將其部署到AEMas a Cloud Service。
version: Cloud Service
topic: Development, Content Management
feature: Metadata
role: Developer
level: Intermediate
kt: 11618
thumbnail: 3412319.jpg
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: e86ddc9d-ce44-407a-a20c-fb3297bb0eb2
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 6%

---

# 自定義命名空間

瞭解如何定義和部署自定義 [命名空間](https://developer.adobe.com/experience-manager/reference-materials/spec/jcr/1.0/4.5_Namespaces.html) 到AEMas a Cloud Service。

自定義命名空間是JCR屬性的可選部分，該屬性位於 `:`。 AEM使用多個命名空間，如：

+ `jcr` 用於JCR系統屬性
+ `cq` (AEM前稱Adobe CQ)
+ `dam` 特定AEM於DAM資產的屬性
+ `dc` Dublin Core屬性

...和其他很多。

命名空間可用於表示屬性的範圍和意圖。 建立自定義命名空間（通常是您的公司名稱）有助於明確標識特定於您的實施的節點或AEM屬性，並包含特定於您的業務的資料。

在中管理自定義命名空間 [Sling Repository初始化（重點）](https://sling.apache.org/documentation/bundles/repository-initialization.html) 指令碼，並將其部AEM署到as a Cloud Service的OSGi配置中 — 並添加到 [項AEM目](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=zh-Hant) `ui.config` 項目。

>[!VIDEO](https://video.tv.adobe.com/v/3412319?quality=12&learn=on)

## 資源

+ [Sling Repository Initialization（重點）文檔](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios)

## 代碼

以下代碼用於配置 `wknd` 命名空間。

### RepositoryInitializer OSGi配置

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~wknd-examples-namespaces.cfg.json`

```json
{

    "scripts": [
        "register namespace (wknd) https://site.wknd/1.0"
    ]
}
```

這允許使用 `wknd` 命名空間，在 `register namespace` 指AEM示。 有關更高級的指令碼定義，請查看 [Sling Repository Initialization（重點）文檔](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios)。
