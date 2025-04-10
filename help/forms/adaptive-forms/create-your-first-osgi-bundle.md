---
title: 使用AEM表單建立您的第一個OSGi套件
description: 使用maven和eclipse建置您的第一個OSGi套件
feature: Adaptive Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2021-06-09T00:00:00Z
duration: 177
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 0%

---


# 建立您的第一個OSGi套件

OSGi套件組合是Java™封存檔案，其中包含Java程式碼、資源，以及說明套件組合及其相依性的資訊清單。 組合是應用程式的部署單位。 本文適用於想使用AEM Forms 6.4或6.5建立OSGi服務或servlet的開發人員。若要建置您的第一個OSGi套件，請遵循下列步驟：


## 安裝JDK

安裝支援的JDK版本。 我已使用JDK1.8。確定已在您的環境變數中新增&#x200B;**JAVA_HOME**，並指向JDK安裝的根資料夾。
將%JAVA_HOME%/bin新增至路徑

![資料來源](assets/java-home.JPG)

>[!NOTE]
> 請勿使用JDK 15。 AEM不支援。

### 測試您的JDK版本

開啟新的命令提示字元視窗並輸入： `java -version`。 您應該取回`JAVA_HOME`變數所識別的JDK版本

![資料來源](assets/java-version.JPG)

## 安裝Maven

Maven是主要用於Java專案的組建自動化工具。 請依照下列步驟在本機系統上安裝maven。

* 在您的C磁碟機中建立名為`maven`的資料夾
* 下載[二進位zip封存](http://maven.apache.org/download.cgi)
* 將zip封存的內容解壓縮至`c:\maven`
* 使用值`C:\maven\apache-maven-3.6.0`建立名為`M2_HOME`的環境變數。 以我為例，**mvn**&#x200B;版本是3.6.0。在撰寫本文時，最新的maven版本是3.6.3
* 將`%M2_HOME%\bin`新增至您的路徑
* 儲存您的變更
* 開啟新的命令提示字元並輸入`mvn -version`。 您應該會看到&#x200B;**mvn**&#x200B;版本已列出，如下列熒幕擷圖所示

![資料來源](assets/mvn-version.JPG)

## Settings.xml

Maven `settings.xml`檔案會定義以各種方式設定Maven執行的值。 最常用來定義本機存放庫位置、替代遠端存放庫伺服器，以及私有存放庫的驗證資訊。

瀏覽至`C:\Users\<username>\.m2 folder`
解壓縮[settings.zip](assets/settings.zip)檔案的內容，並將其置於`.m2`資料夾中。

## 安裝Eclipse

安裝最新版的[eclipse](https://www.eclipse.org/downloads/)

## 建立您的第一個專案

Archetype是一種Maven專案範本工具組。 原型被定義為原始陣列或模型，其他同類物件的製作都來自此原始陣列或模型。 此名稱適合我們努力提供的系統，此系統可提供產生Maven專案的一致方法。 Archetype可協助作者為使用者建立Maven專案範本，並為使用者提供產生這些專案範本引數化版本的方法。
若要建立您的第一個maven專案，請遵循下列步驟：

* 在您的C磁碟機中建立名為`aemformsbundles`的新資料夾
* 開啟命令提示字元並瀏覽至`c:\aemformsbundles`
* 在命令提示字元中執行以下命令
* `mvn archetype:generate  -DarchetypeGroupId=com.adobe.granite.archetypes  -DarchetypeArtifactId=aem-project-archetype -DarchetypeVersion=19`

Maven專案會以互動方式產生，並且系統會要求您提供多個屬性的值，例如

| 屬性名稱 | 重要性 | 值 |
|------------------------|---------------------------------------|---------------------|
| groupId | groupId可唯一識別所有專案中的專案 | com.learningaemforms.adobe |
| appsFolderName | 儲存專案結構的資料夾名稱 | 學習表單 |
| artifactId | artifactId是不含版本的jar的名稱。 如果您已建立它，則可使用小寫字母且不含奇怪符號來選擇任何您想要的名稱。 | 學習表單 |
| 版本 | 如果您將其散佈，則可以選擇具有數字和點(1.0、1.1、1.0.1、...)的任何典型版本。 | 1.0 |

按下Enter鍵即可接受其他屬性的預設值。
如果一切順利，您應該會在命令視窗中看到組建成功訊息

## 從您的Maven專案建立Eclipse專案

將工作目錄變更為`learningaemforms`。
正在從命令列執行`mvn eclipse:eclipse`
上述命令會讀取您的pom檔案，並使用正確的中繼資料建立Eclipse專案，讓Eclipse瞭解專案型別、關係、類別路徑等。

## 將專案匯入eclipse

啟動&#x200B;**Eclipse**

移至&#x200B;**檔案 — >匯入**&#x200B;並選取&#x200B;**現有Maven專案**，如下所示

![資料來源](assets/import-mvn-project.JPG)

按「下一步」

按一下&#x200B;**瀏覽**&#x200B;按鈕以選取`c:\aemformsbundles\learningaemform`

![資料來源](assets/select-mvn-project.JPG)

>[!NOTE]
>您可以視需要選擇匯入適當的模組。 如果您只打算在專案中建立Java程式碼，請僅選取並匯入核心模組。

按一下&#x200B;**完成**&#x200B;以開始匯入程式

專案已匯入至Eclipse，而您看到許多`learningaemforms.xxxx`資料夾

展開`learningaemforms.core`資料夾下的`src/main/java`。 這是您撰寫大部分程式碼的資料夾。

![資料來源](assets/learning-core.JPG)

## 建置您的專案

撰寫OSGi服務或servlet後，您需要建置專案以產生可使用Felix Web主控台部署的OSGi套件。 請參考[AEMFD使用者端SDK](https://repo.adobe.com/nexus/content/repositories/public/com/adobe/aemfd/aemfd-client-sdk/)，在您的Maven專案中包含適當的使用者端SDK。 您必須在核心專案的`pom.xml`的相依性區段中包含AEM FD使用者端SDK，如下所示。

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

若要建置專案，請遵循下列步驟：

* 開啟&#x200B;**命令提示字元視窗**
* 瀏覽至`c:\aemformsbundles\learningaemforms\core`
* 執行命令`mvn clean install`
如果一切順利，您應該會在下列位置`C:\AEMFormsBundles\learningaemforms\core\target`看到該組合。 此套件組合現在已準備好使用Felix Web主控台部署至AEM。
