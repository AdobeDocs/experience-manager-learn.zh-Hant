---
title: 包括第三方罐
description: 學習在項目中使用第三方jar文AEM件
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
kt: 11245
last-substantial-update: 2022-10-15T00:00:00Z
thumbnail: third-party.jpg
exl-id: e8841c63-3159-4f13-89a1-d8592af514e3
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 0%

---

# 在項目中包括第三方捆綁AEM包

在本文中，我們將介紹將第三方OSGi捆綁包AEM包括在您的項目中涉及的步驟。為了本文的目的，我們將包括 [jsch-0.1.55.jar](https://repo1.maven.org/maven2/com/jcraft/jsch/0.1.55/jsch-0.1.55.jar) 我們的AEM計畫。  如果OSGi在主儲存庫中可用，則在項目的POM.xml檔案中包括捆綁的依賴關係。

>[!NOTE]
> 假定第三方jar是OSGi捆綁包

```java
<!-- https://mvnrepository.com/artifact/com.jcraft/jsch -->
<dependency>
    <groupId>com.jcraft</groupId>
    <artifactId>jsch</artifactId>
    <version>0.1.55</version>
</dependency>
```

如果OSGi捆綁包位於檔案系統上，請建立名為 **本地jar** 在項目的基目錄(C:\aemformsbundles\AEMFormsProcessStep\localjar)下，依賴項將類似於此

```java
<dependency>
    <groupId>jsch</groupId>
    <artifactId>jsch</artifactId>
    <version>1.0</version>
    <scope>system</scope>
    <systemPath>${project.basedir}/localjar/jsch-0.1.55-bundle.jar</systemPath>
</dependency>
```

## 建立資料夾結構

我們正在將此捆綁包添加到我們的AEM項目 **AEMFormsProcessStep** 住在 **c:\aemformsbundles** 資料夾

* 開啟 **filter.xml** 從C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\META-INF\vault folder of your project Make a note of the root attribute of the filter element

* 建立以下資料夾結構C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\jcr_root\apps\AEMFormsProcessStep-vendor-packages\application\install
* 的 **apps/AEMFormsProcessStep供應商軟體包** 是filter.xml中的根屬性值
* 更新項目POM.xml的依賴項部分
* 開啟命令提示符。 在我的情況下，導航到項目的資料夾(c:\aemformsbundles\AEMFormsProcessStep)。 執行以下命令

```java
mvn clean install -PautoInstallSinglePackage
```

如果一切順利，則軟體包隨第三方捆綁一起安裝到您的實AEM例中。 可以使用 [felix Web控制台](http://localhost:4502/system/console/bundles)。 第三方捆綁包位於的/apps資料夾中 `crx` 儲存庫，如下所示
![第三方](assets/custom-bundle1.png)
