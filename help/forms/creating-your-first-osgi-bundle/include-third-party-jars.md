---
title: 包含第三方jar
description: 瞭解如何在您的AEM專案中使用協力廠商jar檔案
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
jira: KT-11245
last-substantial-update: 2022-10-15T00:00:00Z
thumbnail: third-party.jpg
exl-id: e8841c63-3159-4f13-89a1-d8592af514e3
duration: 53
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 0%

---

# 在您的AEM專案中包含協力廠商套件組合

在本文中，我們將逐步說明在您的AEM專案中包含第三方OSGi套件組合的相關步驟。基於本文目的，我們將在AEM專案中包含[jsch-0.1.55.jar](https://repo1.maven.org/maven2/com/jcraft/jsch/0.1.55/jsch-0.1.55.jar)。  如果OSGi可在maven存放庫中取得，請在專案的POM.xml檔案中包含套件的相依性。

>[!NOTE]
> 我們假設第三方jar是OSGi套件

```java
<!-- https://mvnrepository.com/artifact/com.jcraft/jsch -->
<dependency>
    <groupId>com.jcraft</groupId>
    <artifactId>jsch</artifactId>
    <version>0.1.55</version>
</dependency>
```

如果您的OSGi套件組合位於您的檔案系統上，請在專案基底目錄(C:\aemformsbundles\AEMFormsProcessStep\localjar)下建立名為&#x200B;**localjar**&#x200B;的資料夾，相依性看起來會像這樣

```java
<dependency>
    <groupId>jsch</groupId>
    <artifactId>jsch</artifactId>
    <version>1.0</version>
    <scope>system</scope>
    <systemPath>${project.basedir}/localjar/jsch-0.1.55-bundle.jar</systemPath>
</dependency>
```

## 建立檔案夾結構

我們正在將此組合新增至位於&#x200B;**c：\aemformsbundles**&#x200B;資料夾中的AEM專案&#x200B;**AEMFormsProcessStep**

* 從專案的C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\META-INF\vault資料夾開啟&#x200B;**filter.xml**
記下篩選元素的根屬性。

* 建立下列檔案夾結構C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\jcr_root\apps\AEMFormsProcessStep-vendor-packages\application\install
* **apps/AEMFormsProcessStep-vendor-packages**&#x200B;是filter.xml中的根屬性值
* 更新專案POM.xml的相依性區段
* 開啟命令提示。 以我的案例瀏覽至專案的資料夾(c：\aemformsbundles\AEMFormsProcessStep)。 執行以下命令

```java
mvn clean install -PautoInstallSinglePackage
```

如果一切順利，套件會與協力廠商套件一起安裝在您的AEM執行個體中。 您可以使用[felix Web主控台](http://localhost:4502/system/console/bundles)檢查套件組合。 協力廠商套件組合可在`crx`存放庫的/apps資料夾中使用，如下所示
![協力廠商](assets/custom-bundle1.png)
