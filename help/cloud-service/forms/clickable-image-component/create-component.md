---
title: 建立可點按的影像元件
description: 在AEM Forms as a Cloud Service中建立可點按的影像元件
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15968
exl-id: b635f171-775d-480e-bf7a-c92ab4af0aee
source-git-commit: b4df652fcda0af5d01077b97aa7fa17cfe2abf4b
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 1%

---

# 建立元件

本文假設您在開發AEM Forms CS方面有一些經驗。同時假設您已建立AEM Forms原型專案。

在IntelliJ或您選擇的任何其他IDE中開啟您的AEM Forms專案。 在下建立名為svg的新節點

```
apps\corecomponent\components\adaptiveForm
```

>[!NOTE]
>
> ``corecomponent``是在建立Maven專案時提供的appId。 此appId在您的環境中可能會不同。


## 建立.content.xml檔案

在svg節點下建立名為.content.xml的檔案。 將下列內容新增至新建立的檔案。 您可以根據需求變更jcr：description、jcr：title和componentGroup。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
    jcr:description="USA MAP"
    jcr:primaryType="cq:Component"
    jcr:title="USA MAP"
    sling:resourceSuperType="wcm/foundation/components/responsivegrid"
    componentGroup="CustomCoreComponent - Adaptive Form"/>
```

## 建立svg.html

建立名為svg.html的檔案。 此檔案會呈現USA map的SVG。將[svg.html](assets/svg.html)的內容複製到新建立的檔案中。 您複製的是美國地圖的SVG。 儲存檔案。

## 部署專案

將專案部署到您的本機雲端就緒例項以測試元件。

若要部署專案，您必須在命令提示字元視窗中瀏覽至專案的根資料夾，然後執行下列命令。

```
mvn clean install -PautoInstallSinglePackage
```

這會將專案部署到您的本機AEM Forms執行個體，並且元件將包含在您的Adaptive Form中

![usa-map](./assets/usa-map.png)
