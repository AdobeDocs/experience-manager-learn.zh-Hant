---
title: 使用AEM表單建立您的第一個OSGi套件組合
description: 使用maven和eclipse建立您的第一個OSGi套件組合
feature: 適用性表單
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
topic: 開發
role: Developer
level: Beginner
source-git-commit: 3a9778c97d57e55e3da740b492472456768fb32c
workflow-type: tm+mt
source-wordcount: '833'
ht-degree: 2%

---


# 建立您的第一個OSGi套件組合

OSGi套件是Java™封存檔案，包含Java程式碼、資源，以及說明套件及其相依性的資訊清單。 套件是應用程式的部署單位。 本文適用於想使用AEM Forms 6.4或6.5建立OSGi服務或servlet的開發人員。若要建立您的第一個OSGi套件組合，請遵循下列步驟：


## 安裝JDK

安裝支援的JDK版本。 我已使用JDK1.8。請確保您已在環境變數中添加&#x200B;**JAVA_HOME**，並指向JDK安裝的根資料夾。
將%JAVA_HOME%/bin添加到路徑

![資料來源](assets/java-home.JPG)

>[!NOTE]
> 請勿使用JDK 15。 AEM不支援此功能。

### 測試您的JDK版本

開啟新的命令提示窗口，然後鍵入：`java -version`。 您應該會回傳`JAVA_HOME`變數所識別的JDK版本

![資料來源](assets/java-version.JPG)

## 安裝Maven

Maven是主要用於Java專案的建置自動化工具。 請按照以下步驟在本地系統上安裝maven。

* 在C驅動器中建立名為`maven`的資料夾
* 下載[二進位zip封存](http://maven.apache.org/download.cgi)
* 將zip封存的內容解壓縮至`c:\maven`
* 建立名為`M2_HOME`的環境變數，其值為`C:\maven\apache-maven-3.6.0`。 在我的案例中，**mvn**&#x200B;版本為3.6.0。撰寫本文時，最新的maven版本為3.6.3
* 將`%M2_HOME%\bin`新增至路徑
* 儲存您的變更
* 開啟新的命令提示符，然後鍵入`mvn -version`。 您應該會看到&#x200B;**mvn**&#x200B;版本列示，如下方螢幕擷取所示

![資料來源](assets/mvn-version.JPG)

## Settings.xml

Maven `settings.xml`檔案定義以各種方式配置Maven執行的值。 最常見的情況是，它用於定義本地儲存庫位置、備用遠程儲存庫伺服器以及專用儲存庫的驗證資訊。

導覽至`C:\Users\<username>\.m2 folder`
解壓縮[settings.zip](assets/settings.zip)檔案的內容，並將其置於`.m2`資料夾中。

## 安裝Eclipse

安裝最新版本的[eclipse](https://www.eclipse.org/downloads/)

## 建立您的第一個專案

原型是Maven專案範本工具包。 原型被定義為原始模式或模型，從中產生所有同類的事物。 我們正嘗試提供系統，提供一致的方式來產生Maven專案，這個名稱就適合了。 原型將協助作者為使用者建立Maven專案範本，並提供使用者產生這些專案範本的參數化版本的方法。
若要建立您的第一個Maven專案，請依照下列步驟操作：

* 在C驅動器中建立名為`aemformsbundles`的新資料夾
* 開啟命令提示符並導航到`c:\aemformsbundles`
* 在命令提示符下運行以下命令
* `mvn archetype:generate  -DarchetypeGroupId=com.adobe.granite.archetypes  -DarchetypeArtifactId=aem-project-archetype -DarchetypeVersion=19`

maven專案將會以互動方式產生，並要求您提供一些屬性的值，例如

| 屬性名稱 | 顯著性 | 值 |
|------------------------|---------------------------------------|---------------------|
| groupId | groupId可在所有專案中唯一識別您的專案 | com.learningaemforms.adobe |
| appsFolderName | 包含項目結構的資料夾的名稱 | 學習版aemforms |
| artifactId | artifactId是未版本的jar的名稱。 如果建立了它，則可以選擇任何您想要的名稱，使用小寫字母而不使用奇怪的符號。 | 學習版aemforms |
| 版本 | 如果您分發該版本，則可以選擇任何包含數字和點(1.0、1.1、1.0.1、...)的典型版本。 | 1.0 |

按Enter鍵，接受其他屬性的預設值。
如果一切順利，您應該會在命令視窗中看到組建成功訊息

## 從Maven專案建立Eclipse專案

將工作目錄更改為`learningaemforms`。
從命令行執行`mvn eclipse:eclipse`
以上命令讀取您的pom檔案，並使用正確的元資料建立Eclipse項目，以便Eclipse了解項目類型、關係、類路徑等。

## 將專案匯入eclipse

啟動&#x200B;**Eclipse**

前往&#x200B;**File -> Import**&#x200B;並選取&#x200B;**Existing Maven Projects**，如下所示

![資料來源](assets/import-mvn-project.JPG)

按「下一步」

按一下&#x200B;**Browse**&#x200B;按鈕，選擇`c:\aemformsbundles\learningaemform`s

![資料來源](assets/select-mvn-project.JPG)

>[!NOTE]
>您可以視需要選取匯入適當的模組。 只有在您打算在專案中建立Java程式碼時，才選取並匯入核心模組。

按一下&#x200B;**完成**&#x200B;以啟動導入過程

專案已匯入Eclipse中，您會看到許多`learningaemforms.xxxx`資料夾

展開`learningaemforms.core`資料夾下的`src/main/java`。 這是您將在其中撰寫大部分程式碼的資料夾。

![資料來源](assets/learning-core.JPG)

## 建置專案

編寫OSGi服務（或servlet）後，您需要建置專案，以產生可使用Felix網頁主控台部署的OSGi套件組合。 請參閱[AEMFD用戶端SDK](https://repo.adobe.com/nexus/content/repositories/public/com/adobe/aemfd/aemfd-client-sdk/) ，將適當的用戶端SDK納入您的Maven專案。 您必須將AEM FD用戶端SDK包含在核心專案`pom.xml`的相依性區段中，如下所示。

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

若要建置專案，請依照下列步驟操作：

* 開啟&#x200B;**命令提示窗口**
* 導航到 `c:\aemformsbundles\learningaemforms\core`
* 執行命令`mvn clean install`
如果一切順利，您應會在下列位置`C:\AEMFormsBundles\learningaemforms\core\target`看到套件組合。 此套件組合現已準備就緒，可使用Felix網頁主控台部署至AEM。
