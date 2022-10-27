---
title: 使用AEM表單建立您的第一個OSGi套件組合
description: 使用maven和eclipse建立您的第一個OSGi套件組合
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


# 建立您的第一個OSGi套件組合

OSGi套件是Java™封存檔案，包含Java程式碼、資源，以及說明套件及其相依性的資訊清單。 套件是應用程式的部署單位。 本文適用於想使用AEM Forms 6.4或6.5建立OSGi服務或servlet的開發人員。若要建立您的第一個OSGi套件組合，請遵循下列步驟：


## 安裝JDK

安裝支援的JDK版本。 我已使用JDK1.8。請確定您已新增 **JAVA_HOME** ，並指向JDK安裝的根資料夾。
將%JAVA_HOME%/bin添加到路徑

![資料來源](assets/java-home.JPG)

>[!NOTE]
> 請勿使用JDK 15。 AEM不支援此功能。

### 測試您的JDK版本

開啟新的命令提示窗口，然後鍵入： `java -version`. 您應該會回傳 `JAVA_HOME` 變數

![資料來源](assets/java-version.JPG)

## 安裝Maven

Maven是主要用於Java專案的建置自動化工具。 請按照以下步驟在本地系統上安裝maven。

* 建立名為 `maven` 在C驅動器中
* 下載 [二進位zip封存](http://maven.apache.org/download.cgi)
* 將zip封存的內容解壓縮至 `c:\maven`
* 建立環境變數，稱為 `M2_HOME` 值為 `C:\maven\apache-maven-3.6.0`. 就我而言， **mvn** 版本為3.6.0。撰寫本文時，最新的maven版本為3.6.3
* 新增 `%M2_HOME%\bin` 到
* 儲存您的變更
* 開啟新的命令提示字元並輸入 `mvn -version`. 您應會看到 **mvn** 如下方螢幕擷取所示的版本

![資料來源](assets/mvn-version.JPG)

## Settings.xml

A Maven `settings.xml` 檔案會定義以各種方式設定Maven執行的值。 最常見的情況是，它用於定義本地儲存庫位置、備用遠程儲存庫伺服器以及專用儲存庫的驗證資訊。

導覽至 `C:\Users\<username>\.m2 folder`
擷取 [settings.zip](assets/settings.zip) 檔案並放入 `.m2` 檔案夾。

## 安裝Eclipse

安裝最新版本的 [eclipe](https://www.eclipse.org/downloads/)

## 建立您的第一個專案

原型是Maven專案範本工具包。 原型被定義為原始模式或模型，從中產生所有同類的事物。 我們正嘗試提供系統，提供一致的方式來產生Maven專案，這個名稱就適合了。 原型可協助作者為使用者建立Maven專案範本，並提供使用者參數化版本專案範本的方法。
若要建立您的第一個Maven專案，請依照下列步驟操作：

* 建立名為 `aemformsbundles` 在C驅動器中
* 開啟命令提示字元並導覽至 `c:\aemformsbundles`
* 在命令提示符下運行以下命令
* `mvn archetype:generate  -DarchetypeGroupId=com.adobe.granite.archetypes  -DarchetypeArtifactId=aem-project-archetype -DarchetypeVersion=19`

Maven專案會以互動方式產生，系統會要求您提供一些屬性的值，例如

| 屬性名稱 | 顯著性 | 值 |
|------------------------|---------------------------------------|---------------------|
| groupId | groupId可在所有專案中唯一識別您的專案 | com.learningaemforms.adobe |
| appsFolderName | 保存項目結構的資料夾的名稱 | 學習版aemforms |
| artifactId | artifactId是未版本的jar的名稱。 如果建立了它，則可以選擇任何您想要的名稱，使用小寫字母而不使用奇怪的符號。 | 學習版aemforms |
| 版本 | 如果您分發該版本，則可以選擇任何包含數字和點(1.0、1.1、1.0.1、...)的典型版本。 | 1.0 |

按Enter鍵，接受其他屬性的預設值。
如果一切順利，您應該會在命令視窗中看到組建成功訊息

## 從Maven專案建立Eclipse專案

將工作目錄變更為 `learningaemforms`.
執行中 `mvn eclipse:eclipse` 從命令行上，以上命令讀取您的pom檔案，並使用正確的元資料建立Eclipse項目，以便Eclipse了解項目類型、關係、類路徑等。

## 將專案匯入eclipse

Launch **Eclipse**

前往 **檔案 — >導入** 選取 **現有Maven專案** 如下所示

![資料來源](assets/import-mvn-project.JPG)

按「下一步」

選取 `c:\aemformsbundles\learningaemform`按一下 **瀏覽** 按鈕

![資料來源](assets/select-mvn-project.JPG)

>[!NOTE]
>您可以視需要選取匯入適當的模組。 只有在您打算在專案中建立Java程式碼時，才選取並匯入核心模組。

按一下 **完成** 啟動導入過程

專案已匯入Eclipse，而您會看到 `learningaemforms.xxxx` 資料夾

展開 `src/main/java` 在 `learningaemforms.core` 檔案夾。 這是您編寫大部分程式碼的資料夾。

![資料來源](assets/learning-core.JPG)

## 建置專案

編寫OSGi服務（或servlet）後，您需要建置專案，以產生可使用Felix網頁主控台部署的OSGi套件組合。 請參閱 [AEMFD用戶端SDK](https://repo.adobe.com/nexus/content/repositories/public/com/adobe/aemfd/aemfd-client-sdk/) 將適當的用戶端SDK納入您的Maven專案。 您必須將AEM FD用戶端SDK包含在 `pom.xml` 核心專案，如下所示。

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

若要建置專案，請依照下列步驟操作：

* 開啟 **命令提示窗口**
* 瀏覽到 `c:\aemformsbundles\learningaemforms\core`
* 執行命令 `mvn clean install`
如果一切順利，您應該會在下列位置看到套件組合 `C:\AEMFormsBundles\learningaemforms\core\target`. 此套件組合現已準備就緒，可使用Felix網頁主控台部署至AEM。
