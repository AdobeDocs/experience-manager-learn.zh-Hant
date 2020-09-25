---
title: 設定本機AEM開發環境
description: 為AEM的Adobe Experience Manager設定本機開發指南。 涵蓋本機安裝、Apache Maven、整合式開發環境和除錯／疑難排解的重要主題。 討論了使用Eclipse IDE、CRXDE-Lite、Visual Studio程式碼和IntelliJ進行開發。
version: 6.4, 6.5
feature: maven-archetype
topics: development
activity: develop
audience: developer
translation-type: tm+mt
source-git-commit: c85a59a8bd180d5affe2a5bf5939dabfb2776d73
workflow-type: tm+mt
source-wordcount: '2594'
ht-degree: 0%

---


# 設定本機AEM開發環境

為AEM的Adobe Experience Manager設定本機開發指南。 涵蓋本機安裝、Apache Maven、整合式開發環境和除錯／疑難排解的重要主題。 討論了 **[!DNL Eclipse IDE]與、[!DNL CRXDE Lite][!DNL Visual Studio Code]和[!DNL IntelliJ]** 開發。

## 概覽

為Adobe Experience Manager或AEM進行開發時，請先設定本機開發環境。 請花點時間來設定高品質的開發環境，以更快的速度提高生產力並編寫更好的程式碼。 我們可以將AEM當地開發環境分為4個領域：

* 本機AEM例項
* [!DNL Apache Maven] project
* 整合開發環境(IDE)
* 疑難排解

## 安裝本機AEM例項

當我們參考本機AEM實例時，我們討論的是在開發人員個人電腦上執行的Adobe Experience Manager。 ***所有*** AEM開發應從針對本機AEM例項編寫和執行程式碼開始。

如果您是AEM的新手，可安裝兩種基本執行模式： ***作者*** 和 ***發佈***。 Author ****** runmode [](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/configure-runmodes.html) 是數位行銷人員用來建立和管理內容的環境。 在開發 **程式碼時** ，您大部份的時間都會將程式碼部署至「作者」例項。 這可讓您建立新頁面，以及新增和設定元件。 AEM Sites是WYSIWYG編寫CMS，因此，大部分的CSS和JavaScript都可針對編寫例項進行測試。

它也是對本 *機* 「發佈」例項的重 ****** 要測試程式碼。 Publish ***例項*** 是您網站的訪客將與之互動的AEM環境。 雖然 ***Publish*** 執行個體與 ****** Author執行個體是相同的技術堆疊，但組態和權限有一些主要差異。 程式碼 *應一律* ，在升級至較高等級的環境之前，應先針對本機 ***Publish*** 執行個體進行測試。

### 步驟

1. 確 [保已安裝](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html) Java。
   * 適 [用於AEM 6.5+的Java JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderstestModied&amp;orderby.st.sort.st.st.st.st.st.st&amp;st.sted&amp;orde.sted&amp;ord&amp;orde.s.sted&amp;ord.se.s.st.st.s.se.st.se.se.s=desc&amp;d.se.se.st.s&amp;dsc&amp;des&amp;des&amp;desc&amp;s&amp;desc&amp;s&amp;d.sy=list&amp;p.offset=0&amp;p.limit=14)
   * [AEM 6.5之前](https://www.oracle.com/technetwork/java/javase/downloads/index.html#JDK8) 的AEM版本適用的Java JDK 8
2. 取得 [AEM QuickStart Jar和 [!DNL license.properties]](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)。
3. 在您的電腦上建立資料夾結構，如下所示：

   ```plain
   ~/aem-sdk
       /author
       /publish
   ```

4. 將 [!DNL QuickStart] JAR重新命 ***名為aem-author-p4502.jar*** ，並將其置於目錄下 `/author` 方。 在目錄 ***[!DNL license.properties]*** 下添加文 `/author` 件。
5. 製作JAR的復 [!DNL QuickStart] 本，將它重新命名為 ***aem-publish-p4503.jar*** ，並將它置於目錄下 `/publish` 方。 在目錄下添加 ***[!DNL license.properties]*** 檔案副 `/publish` 本。

   ```plain
   ~/aem-sdk
       /author
           + aem-author-p4502.jar
           + license.properties
       /publish
           + aem-publish-p4503.jar
           + license.properties
   ```

6. 連按兩下 ***aem-author-p4502.jar*** 檔案，以安裝 **Author** 例項。 這將啟動作者實例，在本地計 **算機的埠** 4502上運行。

   連按兩下 ***aem-publish-p4503.jar*** 檔案，以安裝 **Publish** 例項。 這將啟動在本機電腦的埠 **4503** 上執行的Publish執行個體。

   >[!NOTE]
   >
   >視您的開發機器硬體而定，可能很難同時執行 **Author和Publish** 執行個體。 您很少需要在本機設定上同時執行兩者。

   如需詳細資訊，請 [參閱「部署和維護AEM例項」](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html)。

## 安裝Apache Maven

***[!DNL Apache Maven]*** 是管理Java專案建立和部署程式的工具。 AEM是以Java為基礎的平台， [!DNL Maven] 是管理AEM專案程式碼的標準方式。 當我們說 ***AEM Maven Project*** ，或僅是您的 ***AEM Project***，我們指的是包含您網站所有自訂程式碼的Maven ** 專案。

所有AEM專案都應以最新版本為基礎 **[!DNL AEM Project Archetype]**: [https://github.com/Adobe-Marketing-Cloud/aem-project-archetype](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype). 將 [!DNL AEM Project Archetype] 會建立AEM專案的引導程式，其中包含一些范常式式碼和內容。 此外 [!DNL AEM Project Archetype] 還包 **[!DNL AEM WCM Core Components]** 含設定為用於您的專案。

>[!CAUTION]
>
>啟動新專案時，使用最新版原型是最佳實務。 請記住，原型有多種版本，並且並非所有版本都與舊版AEM相容。

### 步驟

1. 下載 [Apache Maven](https://maven.apache.org/download.cgi)
2. 安 [裝Apache Maven](https://maven.apache.org/install.html) ，並確保已將安裝添加到命令行中 `PATH`。
   * [!DNL macOS] 使用者可使用 [Homebrew安裝Maven](https://brew.sh/)
3. 通過打 **[!DNL Maven]** 開新的命令行終端並執行以下操作來驗證是否已安裝：

   ```shell
   $ mvn --version
   Apache Maven 3.3.9
   Maven home: /Library/apache-maven-3.3.9
   Java version: 1.8.0_111, vendor: Oracle Corporation
   Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home/jre
   Default locale: en_US, platform encoding: UTF-8
   ```

4. 將描述 **[!DNL adobe-public]** 檔新增至 [!DNL Maven] settings.xml檔案，以便自動新增 [](https://maven.apache.org/settings.html)**[!DNL repo.adobe.com]** 至主要的建立程式。

5. 如果檔案不 `settings.xml` 存 `~/.m2/settings.xml` 在，請建立名為的檔案。

6. 根據此 **[!DNL adobe-public]** 處的指示，將描述 `settings.xml` 檔新 [增至檔案中](https://repo.adobe.com/)。

   以下列 `settings.xml` 出範例。 *請注意，用戶目錄`settings.xml`下的命名慣例和位置`.m2`很重要。*

   ```xml
   <settings xmlns="https://maven.apache.org/SETTINGS/1.0.0"
     xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="https://maven.apache.org/SETTINGS/1.0.0
                         https://maven.apache.org/xsd/settings-1.0.0.xsd">
   <profiles>
    <!-- ====================================================== -->
    <!-- A D O B E   P U B L I C   P R O F I L E                -->
    <!-- ====================================================== -->
        <profile>
            <id>adobe-public</id>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
            <properties>
                <releaseRepository-Id>adobe-public-releases</releaseRepository-Id>
                <releaseRepository-Name>Adobe Public Releases</releaseRepository-Name>
                <releaseRepository-URL>https://repo.adobe.com/nexus/content/groups/public</releaseRepository-URL>
            </properties>
            <repositories>
                <repository>
                    <id>adobe-public-releases</id>
                    <name>Adobe Public Repository</name>
                    <url>https://repo.adobe.com/nexus/content/groups/public</url>
                    <releases>
                        <enabled>true</enabled>
                        <updatePolicy>never</updatePolicy>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </repository>
            </repositories>
            <pluginRepositories>
                <pluginRepository>
                    <id>adobe-public-releases</id>
                    <name>Adobe Public Repository</name>
                    <url>https://repo.adobe.com/nexus/content/groups/public</url>
                    <releases>
                        <enabled>true</enabled>
                        <updatePolicy>never</updatePolicy>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </pluginRepository>
            </pluginRepositories>
        </profile>
   </profiles>
    <activeProfiles>
        <activeProfile>adobe-public</activeProfile>
    </activeProfiles>
   </settings>
   ```

7. 執行下列命 **令，確認adobe-public** 描述檔是否作用中：

   ```shell
   $ mvn help:effective-settings
   ...
   <activeProfiles>
       <activeProfile>adobe-public</activeProfile>
   </activeProfiles>
   <pluginGroups>
       <pluginGroup>org.apache.maven.plugins</pluginGroup>
       <pluginGroup>org.codehaus.mojo</pluginGroup>
   </pluginGroups>
   </settings>
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  0.856 s
   ```

   如果您未看到， **[!DNL adobe-public]** 表示您的檔案中未正確參考Adobe回 `~/.m2/settings.xml` 購。 請重新造訪之前的步驟，並驗證settings.xml檔案是否參照Adobe repo。

## 建立整合的開發環境

整合的開發環境或IDE是結合文字編輯器、語法支援和建置工具的應用程式。 根據您正在進行的開發類型，一個IDE比另一個IDE更好。 不論IDE為何，必須能夠定期將程式碼推 ***播*** ，至本機AEM例項以進行測試。 您也必須偶爾從本 ***機*** AEM例項提取設定至AEM專案，以便持續使用Git等來源控制管理系統。

以下是AEM開發中使用的一些較常用的IDE，以及顯示與本機AEM例項整合的對應影片。

### [!DNL Eclipse] IDE

IDE **[[!DNL Eclipse] 是](https://www.eclipse.org/ide/)** Java開發中較常用的IDE之一，主要是因為它是開放原始碼和免費 ***的***! Adobe提供外掛程式 **[[!DNL AEM Developer Tools]](https://eclipse.adobe.com/aem/dev-tools/)**, [!DNL Eclipse] 可讓使用好用的GUI進行更輕鬆的開發，以便將程式碼與本機AEM例項同步。 由於 [!DNL Eclipse] GUI支援，所以建議IDE讓AEM新手的開發人員使用 [!DNL AEM Developer Tools]。

#### 安裝與設定

1. 下載並安裝 [!DNL Eclipse] IDE，以 [!DNL Java EE Developers]便： [https://www.eclipse.org](https://www.eclipse.org/)
1. 請依照指示安裝外掛程 [!DNL AEM Developer Tools] 式： [https://eclipse.adobe.com/aem/dev-tools/](https://eclipse.adobe.com/aem/dev-tools/)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 —— 導入Maven項目
* 01:24 —— 使用Maven建立和部署原始碼
* 04:33 —— 使用AEM Developer Tool推播程式碼變更
* 10:55 —— 使用AEM Developer Tool提取程式碼變更
* 13:12 —— 使用Eclipse的整合除錯工具

### IntelliJ IDEA

IntelliJ IDEA **[](https://www.jetbrains.com/idea/)** 是功能強大的IDE，適用於專業Java開發。 [!DNL IntelliJ IDEA] 提供兩種口味：免 ***費版*** 本 [!DNL Community] 、商業版（付費） [!DNL Ultimate] 。 免費版 [!DNL Community] 本足 [!DNL IntellIJ IDEA] 夠讓AEM開發更多，但是擴充 [!DNL Ultimate] 了 [其功能集](https://www.jetbrains.com/idea/download)。

#### [!DNL Installation and Setup]

1. 下載並安裝 [!DNL IntelliJ IDEA]: [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. 安 [!DNL Repo] 裝（命令行工具）: [https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

* 00:00 —— 導入Maven項目
* 05:47 —— 使用Maven建立和部署原始碼
* 08:17 —— 使用回購功能推送變更
* 14:39 —— 利用回購協定進行更改
* 17:25 —— 使用IntelliJ IDEA的整合除錯工具

### [!DNL Visual Studio Code]

**[Visual Studio Code](https://code.visualstudio.com/)** (Visual Studio程式碼 ***)已迅速成為前端開發人員最愛的*** 工具，具備增強的JavaScript支援 [!DNL Intellisense]和瀏覽器除錯支援。 **[!DNL Visual Studio Code]** 是開放原始碼、免費的，並具備許多強大的擴充功能。 [!DNL Visual Studio Code] 可以透過Adobe工具repo的協助來設定與AEM整 **[合](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)。** 此外，還可安裝數個社群支援的擴充功能，以與AEM整合。

[!DNL Visual Studio Code] 是前端開發人員的絕佳選擇，他們主要會編寫CSS/LESS和JavaScript程式碼來建立AEM用戶端程式庫。 對於新的AEM開發人員而言，此工具可能不是最佳選擇，因為節點定義（對話框、元件）都需要以原始XML編輯。 有數種Java擴充功能可 [!DNL Visual Studio Code]供使用，但主要是執行Java開發或 [!DNL Eclipse IDE] 可 [!DNL IntelliJ] 能偏好使用。

#### 重要連結

* [**下載**](https://code.visualstudio.com/Download)**Visual Studio代碼**
* **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)** - JCR內容的FTP樣式工具
* **[aemfed](https://aemfed.io/)** —— 加速您的AEM前端工作流程
* **[AEM Sync](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)** - Visual Studio程式碼的社群支援*擴充功能

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 —— 導入Maven項目
* 00:53 —— 使用Maven建立和部署原始碼
* 04:03 —— 使用Repo命令列工具進行推播程式碼變更
* 08:29 —— 使用Repo命令行工具提取代碼更改
* 10:40 —— 使用修正工具推播程式碼變更
* 14:24 —— 疑難排解，重建客戶端庫

### [!DNL CRXDE Lite]

[CRXDE Lite](https://helpx.adobe.com/experience-manager/6-4/sites/developing/using/developing-with-crxde-lite.html) 是AEM存放庫的瀏覽器檢視。 [!DNL CRXDE Lite] 內嵌在AEM中，可讓開發人員執行標準開發工作，例如編輯檔案、定義元件、對話方塊和範本。 [!DNL CRXDE Lite] 並非 ***要成為*** 完整的開發環境，但是除錯工具非常有效。 [!DNL CRXDE Lite] 在擴充或瞭解產品程式碼庫以外的程式碼時，會很有用。 [!DNL CRXDE Lite] 提供了儲存庫的強大視圖，以及有效測試和管理權限的方法。

[!DNL CRXDE Lite] 一律應與其他IDE搭配使用，以測試和除錯程式碼，但絕不是主要開發工具。 它的語法支援有限，沒有自動完成功能，並且與來源控制管理系統整合有限。

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## 疑難排解

***說明!*** 我的程式碼沒用！ 和所有開發一樣，有時候（可能很多）您的程式碼無法如預期般運作。 AEM是強大的平台，但具備強大的功能……複雜性極高。 以下是疑難排解和追蹤問題的幾個高階起點（但遠非可能出錯的完整清單）:

### 驗證代碼部署

當遇到問題時，一個好的第一步是確認程式碼已部署並成功安裝至AEM。

1. **檢查[!UICONTROL Package Manager]** ，以確保已上載並安裝代碼包： [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). 檢查時間戳記，確認軟體包最近已安裝。
1. 如果使用例如或等工具進行增 [!DNL Repo] 量檔案更 [!DNL AEM Developer Tools]新， **請[!DNL CRXDE Lite]** 檢查檔案是否已推送至本機AEM例項，以及檔案內容已更新： [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **如果在OSGi套件中看到與** Java程式碼相關的問題，請檢查套件是否已上傳。 開啟 [!UICONTROL Adobe Experience Manager Web Console]: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) ，並搜尋您的套件。 請確定此捆綁包的狀態為「 **[!UICONTROL 活動]** 」。 請參閱下方，以取得與疑難排解已安裝狀態之套件 **[!UICONTROL 相關]** 的詳細資訊。

#### 檢查日誌

AEM是一個聊天平台，在 **error.log中記錄許多有用的資訊**。 您 **可在已安裝AEM的地方找到error.log** :&lt; `aem-installation-folder>/crx-quickstart/logs/error.log`ý?

追蹤向下問題的實用技巧是在Java程式碼中新增記錄陳述式：

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
...

public class MyClass {
    private final Logger log = LoggerFactory.getLogger(getClass());

    ...

    String myVariable = "My Variable";

    log.debug("Debug statement of myVariable {}", myVariable);

    log.info("Info statement of myVariable {}", myVariable);
}
```

預設情況下， **error.log** 配置為log語 *[!DNL INFO]* 句。 如果要更改日誌級別，請轉至「日誌支援」 [!UICONTROL 進行更改]: [http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog). 您也會發現 **error.log** 太動聽。 您可以使用「 [!UICONTROL 日誌支援] 」為指定的Java包配置日誌語句。 這是專案的最佳實務，以便輕鬆將自訂程式碼問題與OOTB AEM平台問題區隔開。

![AEM中的記錄設定](./assets/set-up-a-local-aem-development-environment/logging.png)

#### Bundle處於「已安裝」狀態 {#bundle-active}

所有組合（排除片段）都應處於「作 **[!UICONTROL 用中]** 」狀態。 如果您在「已安裝  」狀態中看到程式碼套件，則有問題需要解決。 多數情況下，這是依賴性問題：

![AEM中的Bundle錯誤](assets/set-up-a-local-aem-development-environment/bundle-error.png)

在上述螢幕擷取中， [!DNL WKND Core bundle] 是「已 [!UICONTROL 安裝] 」狀態。 這是因為Bundle需要的版本與AEM例項 `com.adobe.cq.wcm.core.components.models` 上的版本不同。

可使用的實用工具是「相依性 [!UICONTROL 搜尋器」]: [http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder). 新增Java套件名稱，以檢查AEM例項上有哪些版本可用：

![核心元件](assets/set-up-a-local-aem-development-environment/core-components.png)

繼續上述範例，我們可以看到，AEM例項上安裝的版本是 **12.2** , **而搭售的版本是12.6** 。 從這裡，您可以倒著工作，看看AEM的相 [!DNL Maven] 依性是否符合AEM專 [!DNL Maven] 案中的相依性。 在上述範例中， [!DNL Core Components] v2.2.0 **已安裝在AEM例項上，但程式碼套件是建立時需依賴** v2.2.2 ****，因此是相關性問題的原因。

#### 驗證Sling Models註冊 {#osgi-component-sling-models}

AEM元件應一律以封裝任何商 [!DNL Sling Model] 業邏輯作為備份，並確保HTL轉譯指令碼保持簡潔。 如果遇到找不到Sling Model的問題，從主控台檢查 [!DNL Sling Models] 可能會有幫助： [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels). 這會告訴您Sling Model是否已註冊，以及它系結至的資源類型（元件路徑）。

![Sling Model Status](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

顯示與元件資 [!DNL Sling Model]源類 `BylineImpl` 型相關聯的註冊 `wknd/components/content/byline`。

#### CSS或JavaScript問題

對於大部分的CSS和JavaScript問題，使用瀏覽器的開發工具是最有效的疑難排解方式。 若要針對AEM作者例項進行開發時縮小問題，請檢視「已發佈」頁面。

![CSS或JS問題](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

開啟「頁 [!UICONTROL 面屬性] 」功能表，然後按 [!UICONTROL 一下「檢視為發佈」]。 這會開啟未使用AEM編輯器且查詢參數設為 **wcmmode=disabled的頁面**。 這可有效停用AEM編寫UI，並讓疑難排解／除錯前端問題變得更輕鬆。

開發前端程式碼時，另一個常見的問題是舊版或過時的CSS/JS正在載入。 首先，確保已清除瀏覽器歷史記錄，並在必要時啟動Incognito瀏覽器或新會話。

#### 調試客戶端庫

使用不同的類別和內嵌方式來包含多個用戶端程式庫，疑難排解會很麻煩。 AEM提供數種工具來協助處理此問題。 最重要的工具之一是 [!UICONTROL Rebuild Client Libraries] ，這會強制AEM重新編譯任何LESS檔案並產生CSS。

* [Dump Libs](http://localhost:4502/libs/granite/ui/content/dumplibs.html) —— 列出在AEM例項中註冊的所有用戶端程式庫。 &lt;host>/libs/granite/ui/content/dumplibs.html
* [Test Output](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) —— 允許用戶根據類別查看clientlib的預期HTML輸出。 &lt;host>/libs/granite/ui/content/dumplibs.test.html
* [庫相依性驗證](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) -突出顯示找不到的任何相依性或嵌入類別。 &lt;host>/libs/granite/ui/content/dumplibs.validate.html
* [Rebuild Client Libraries](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) —— 允許使用者強制AEM重建所有用戶端程式庫，或使用戶端程式庫的快取失效。 此工具在使用LESS進行開發時特別有效，因為這會迫使AEM重新編譯產生的CSS。 一般而言，使快取無效，然後執行頁面重新整理與重建所有程式庫比較有效。 &lt;host>/libs/granite/ui/content/dumplibs.rebuild.html

![除錯Clientlibs](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>如果您不斷使用「重建客戶端庫」工具使快取失效  ，則對所有客戶端庫進行一次重建可能是值得的。 這可能需要大約15分鐘，但通常可消除未來的快取問題。
