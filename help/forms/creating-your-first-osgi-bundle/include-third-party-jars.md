---
title: 使用AEM Forms建立您的第一個OSGi套件組合
description: 使用Maven和Eclipse建立您的第一個OSGi套件組合
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
kt: kt-11245
source-git-commit: 8944a4feaefbc4cf0db52011a0d49b22341780c0
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 0%

---

# 在您的AEM專案中包含協力廠商套件組合

在本文中，我們將逐步說明將第三方OSGi套件組合包含在您的AEM專案中的步驟。為了本文的目的，我們將包含 [jsch-0.1.55.jar](https://repo1.maven.org/maven2/com/jcraft/jsch/0.1.55/jsch-0.1.55.jar) 在AEM專案中。  如果OSGi可在maven存放庫中使用，則專案的POM.xml檔案中會包含套件組合的相依性。

>[!NOTE]
> 假定第三方jar是OSGi套件

```java
<!-- https://mvnrepository.com/artifact/com.jcraft/jsch -->
<dependency>
    <groupId>com.jcraft</groupId>
    <artifactId>jsch</artifactId>
    <version>0.1.55</version>
</dependency>
```

如果您的OSGi套件組合位於您的檔案系統上，相依性將如下所示

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

我們將此套件組合新增至AEM專案 **AEMFormsProcessStep** 居住在 **c:\aemformsbundles** 資料夾

* 開啟 **filter.xml** 從C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\META-INF\vault folder of your project Make a note of the root attribute of the filter element。

* 建立下列資料夾結構C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\jcr_root\apps\AEMFormsProcessStep-vendor-packages\application\install
* 此 **apps/AEMFormsProcessStep-vendor-packages** 是filter.xml中的根屬性值
* 更新專案POM.xml的相依性區段
* 開啟命令提示符。 在我的案例中，導覽至您專案的資料夾(c:\aemformsbundles\AEMFormsProcessStep)。 執行以下命令

```java
mvn clean install -pAutoInstallSinglePackage
```

如果一切順利，則會將套件與協力廠商套件一併安裝至您的AEM執行個體。 您可以使用 [felix web console](http://localhost:4502/system/console/bundles). 協力廠商套件組合位於 `crx` 存放庫，如下所示
![協力廠商](assets/custom-bundle1.png)



