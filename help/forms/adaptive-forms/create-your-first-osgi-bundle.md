---
title: 使用表單建立第一個OSGi捆綁包AEM
description: 使用maven和eclipse構建您的第一個OSGi捆綁包
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2021-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '820'
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
* 下載 [二進位zip存檔](http://maven.apache.org/download.cgi)
* 將ZIP存檔的內容解壓到 `c:\maven`
* 建立名為 `M2_HOME` 值為 `C:\maven\apache-maven-3.6.0`。 就我而言， **mvn** 版本為3.6.0。在撰寫本文時，最新版本為3.6.3
* 添加 `%M2_HOME%\bin` 你的路
* 保存更改
* 開啟新的命令提示符並鍵入 `mvn -version`。 您應該看到 **mvn** 下面螢幕截圖中所示的版本

![資料源](assets/mvn-version.JPG)

## Settings.xml

馬文 `settings.xml` file定義用各種方式配置Maven執行的值。 通常，它用於定義本地儲存庫位置、備用遠程儲存庫伺服器和專用儲存庫的身份驗證資訊。

導航到 `C:\Users\<username>\.m2 folder`
提取 [設定.zip](assets/settings.zip) 把檔案放在 `.m2` 的子菜單。

## 安裝Eclipse

安裝最新版本 [日](https://www.eclipse.org/downloads/)

## 建立第一個項目

原型是Maven項目的模板工具包。 原型被定義為原始圖案或模型，從中可以製造所有同類的事物。 我們試圖提供一個系統，提供生成Maven項目的一致方法，這個名稱與此相符。 原型幫助作者為用戶建立Maven項目模板，並為用戶提供生成這些項目模板參數化版本的方法。
要建立第一個主項目，請執行以下步驟：

* 建立名為 `aemformsbundles` 在C驅動器中
* 開啟命令提示符並導航到 `c:\aemformsbundles`
* 在命令提示符下運行以下命令
* `mvn archetype:generate  -DarchetypeGroupId=com.adobe.granite.archetypes  -DarchetypeArtifactId=aem-project-archetype -DarchetypeVersion=19`

Maven項目是以交互方式生成的，系統會要求您為許多屬性(如

| 屬性名稱 | 意義 | 值 |
|------------------------|---------------------------------------|---------------------|
| 組ID | groupId可唯一標識所有項目中的項目 | com.learningaemforms.adobe |
| appsFolderName | 保存項目結構的資料夾的名稱 | 學習Aemon |
| 項目ID | artifactId是不帶版本的jar的名稱。 如果建立了它，則可以選擇任何您想要的名稱，包含小寫字母和無奇異符號。 | 學習Aemon |
| 版本 | 如果分發它，則可以選擇任何帶數字和點(1.0、1.1、1.0.1、...)的典型版本。 | 1.0 |

按Enter鍵，接受其它屬性的預設值。
如果一切順利，您應在命令窗口中看到生成成功消息

## 從主項目建立eclipse項目

將工作目錄更改為 `learningaemforms`。
正在執行 `mvn eclipse:eclipse` 命令行中，以上命令讀取pom檔案並建立具有正確元資料的Eclipse項目，以便Eclipse瞭解項目類型、關係、類路徑等。

## 將項目導入eclipse

啟動 **日蝕**

轉到 **檔案 — >導入** 選擇 **現有Maven項目** 如圖所示

![資料源](assets/import-mvn-project.JPG)

按一下「下一步」

選擇 `c:\aemformsbundles\learningaemform`按一下 **瀏覽** 按鈕

![資料源](assets/select-mvn-project.JPG)

>[!NOTE]
>您可以根據需要選擇導入相應的模組。 如果您只打算在項目中建立Java代碼，請僅選擇並導入核心模組。

按一下 **完成** 啟動導入流程

項目已導入到Eclipse中，您會看到 `learningaemforms.xxxx` 資料夾

展開 `src/main/java` 下 `learningaemforms.core` 的子菜單。 這是您正在其中寫入大部分代碼的資料夾。

![資料源](assets/learning-core.JPG)

## 生成項目

一旦編寫了OSGi服務或Servlet，就需要構建項目以生成可使用Felix Web控制台部署的OSGi捆綁包。 請參考 [AEMFD客戶端SDK](https://repo.adobe.com/nexus/content/repositories/public/com/adobe/aemfd/aemfd-client-sdk/) 在Maven項目中包含相應的客戶端SDK。 必須將AEMFD客戶端SDK包含在 `pom.xml` 如下所示。

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

要構建項目，請執行以下步驟：

* 開啟 **命令提示符窗口**
* 瀏覽到 `c:\aemformsbundles\learningaemforms\core`
* 執行命令 `mvn clean install`
如果一切順利，您應在以下位置看到捆綁 `C:\AEMFormsBundles\learningaemforms\core\target`。 此捆綁包現已準備好使用Felix AEM Web控制台部署到。
