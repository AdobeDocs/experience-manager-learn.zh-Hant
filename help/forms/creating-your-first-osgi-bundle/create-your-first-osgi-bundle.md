---
title: 與AEM Forms建立第一個OSGi捆綁包
description: 使用Maven和Eclipse構建您的第一個OSGi捆綁包
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
exl-id: 307cc3b2-87e5-4429-8f21-5266cf03b78f
source-git-commit: f4e86059d29acf402de5242f033a25f913febf36
workflow-type: tm+mt
source-wordcount: '674'
ht-degree: 1%

---

# 建立第一個OSGi捆綁包

OSGi捆綁包是Java™存檔檔案，包含Java代碼、資源，以及描述捆綁包及其依賴項的清單。 該捆綁包是應用程式的部署單位。 本文旨在為希望使用AEM Forms6.4或6.5建立OSGi服務或Servlet的開發人員提供幫助。要構建第一個OSGi捆綁包，請執行以下步驟：


## 安裝JDK

安裝支援的JDK版本。 我使用了JDK1.8。確保已添加 **JAVA_HOME** 中，並指向JDK安裝的根資料夾。
將%JAVA_HOME%/bin添加到路徑

![資料源](assets/java-home.JPG)

>[!NOTE]
> 請勿使用JDK 15。 不支援AEM。

### TestJDK版本

開啟新的命令提示符窗口並鍵入： `java -version`。 應返回由 `JAVA_HOME` 變數

![資料源](assets/java-version.JPG)

## 安裝Maven

Maven是一個主要用於Java項目的生成自動化工具。 請按照以下步驟在本地系統上安裝maven。

* 建立名為 `maven` 在C驅動器中
* 下載 [二進位zip存檔](https://maven.apache.org/download.cgi)
* 將ZIP存檔的內容解壓到 `c:\maven`
* 建立名為 `M2_HOME` 值為 `C:\maven\apache-maven-3.6.0`。 就我而言， **mvn** 版本為3.6.0。在撰寫本文時，最新版本為3.6.3
* 添加 `%M2_HOME%\bin` 你的路
* 保存更改
* 開啟新的命令提示符並鍵入 `mvn -version`。 您應該看到 **mvn** 下面螢幕截圖中所示的版本

![資料源](assets/mvn-version.JPG)


## 安裝Eclipse

安裝最新版本 [日](https://www.eclipse.org/downloads/)

## 建立第一個項目

原型是Maven項目的模板工具包。 原型被定義為原始圖案或模型，從中可以製造所有同類的事物。 我們試圖提供一個系統，提供生成Maven項目的一致方法，這個名稱與此相符。 原型將幫助作者為用戶建立Maven項目模板，並為用戶提供生成這些項目模板參數化版本的方法。
要建立第一個主項目，請執行以下步驟：

* 建立名為 `aemformsbundles` 在C驅動器中
* 開啟命令提示符並導航到 `c:\aemformsbundles`
* 在命令提示符下運行以下命令

```java
mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate -D archetypeGroupId=com.adobe.aem -D archetypeArtifactId=aem-project-archetype -D archetypeVersion=36 -D appTitle="My Site" -D appId="mysite" -D groupId="com.mysite" -D aemVersion=6.5.13
```

成功完成後，您應在命令窗口中看到生成成功消息

## 從主項目建立eclipse項目

* 將工作目錄更改為 `mysite`
* 執行 `mvn eclipse:eclipse` 命令行。 該命令讀取pom檔案，並使用正確的元資料建立Eclipse項目，以便Eclipse瞭解項目類型、關係、類路徑等。

## 將項目導入eclipse

啟動 **日蝕**

轉到 **檔案 — >導入** 選擇 **現有Maven項目** 如圖所示

![資料源](assets/import-mvn-project.JPG)

按一下「下一步」

選擇c:\aemformsbundles\mysite by clicking the **瀏覽** 按鈕

![資料源](assets/mysite-eclipse-project.png)

>[!NOTE]
>您可以根據需要選擇導入相應的模組。 如果您只打算在項目中建立Java代碼，請僅選擇並導入核心模組。

按一下 **完成** 啟動導入流程

項目已導入到Eclipse中，您將看到 `mysite.xxxx` 資料夾

展開 `src/main/java` 下 `mysite.core` 的子菜單。 這是您將在其中寫入大部分代碼的資料夾。

![資料源](assets/mysite-core-project.png)

## 包括AEMFD客戶端SDK

您需要將AEMFD客戶端sdk包括在項目中，以利用隨AEM Forms提供的各種服務。 請參考 [AEMFD客戶端SDK](https://mvnrepository.com/artifact/com.adobe.aemfd/aemfd-client-sdk) 在Maven項目中包含相應的客戶端SDK。 您必須將FD客戶AEM端SDK包括在 `pom.xml` 如下所示。

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

要構建項目，請執行以下步驟：

* 開啟 **命令提示符窗口**
* 導航到 `c:\aemformsbundles\mysite\core`
* 執行命令 `mvn clean install -PautoInstallBundle`
上述命令在運行於的伺服器中生成AEM並安裝捆綁包 `http://localhost:4502`。 此捆綁包也將在以下位置的檔案系統上提供
   `C:\AEMFormsBundles\mysite\core\target` 可以使用 [Felix Web控制台](http://localhost:4502/system/console/bundles)
