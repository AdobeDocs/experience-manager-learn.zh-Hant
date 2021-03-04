---
title: 建立當地開發AEM環境
description: 為Adobe Experience Manager設立地方發展指南AEM。 涵蓋本機安裝、Apache Maven、整合式開發環境和除錯／疑難排解的重要主題。 討論了使用Eclipse IDE、CRXDE-Lite、Visual Studio程式碼和IntelliJ進行開發。
version: 6.4, 6.5
feature: 開發人員工具
topics: development
activity: develop
audience: developer
topic: 開發
role: 開發人員
level: 初學者
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '2657'
ht-degree: 1%

---


# 建立當地開發AEM環境

為Adobe Experience Manager設立地方發展指南AEM。 涵蓋本機安裝、Apache Maven、整合式開發環境和除錯／疑難排解的重要主題。 討論了與&#x200B;**[!DNL Eclipse IDE]、[!DNL CRXDE Lite]、[!DNL Visual Studio Code]和[!DNL IntelliJ]**&#x200B;一起開發。

## 概覽

建立地方開發環境是Adobe Experience Manager, 請花點時間來設定高品質的開發環境，以更快的速度提高生產力並編寫更好的程式碼。 我們可以將AEM地方開發環境分為4個方面：

* 本機例AEM項
* [!DNL Apache Maven] project
* 整合開發環境(IDE)
* 疑難排解

## 安裝本機AEM例項

當我們參考本機AEM實例時，會討論在開發人員個人電腦上執行的Adobe Experience Manager復本。 ***AllAEM*** 開發應從針對本機執行個體編寫和執行程式AEM碼開始。

如果您是新手，AEM則可安裝兩種基本的執行模式：***作者***&#x200B;和&#x200B;***發佈***。 ***Author*** [runmode](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/configure-runmodes.html)是數位行銷人員用來建立和管理內容的環境。 在開發&#x200B;**most**&#x200B;時，您將要將程式碼部署至「作者」例項。 這可讓您建立新頁面，以及新增和設定元件。 AEM Sites是WYSIWYG編寫CMS，因此大部分的CSS和JavaScript都可針對編寫例項進行測試。

它也是針對本機&#x200B;***Publish***&#x200B;例項的&#x200B;*critical*&#x200B;測試程式碼。 ***Publish***&#x200B;例項是您網站AEM的訪客將與之互動的環境。 雖然&#x200B;***Publish***&#x200B;實例與&#x200B;***Author***&#x200B;實例的技術堆棧相同，但配置和權限有一些主要區別。 程式碼應在升級至較高層級環境之前，先針對本機&#x200B;***Publish***&#x200B;例項測試&#x200B;*always*。

### 步驟

1. 確保已安裝[Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)。
   * 6.5+版本偏好[Java JDK 11&lt;a1/AEM>](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderstestModied&amp;orderby.st.sort.st.st.st.st.st.st&amp;st.sted&amp;orde.sted&amp;ord&amp;orde.s.sted&amp;ord.se.s.st.st.s.se.st.se.se.s=desc&amp;d.se.se.st.s&amp;dsc&amp;des&amp;des&amp;desc&amp;s&amp;desc&amp;s&amp;d.sy=list&amp;p.offset=0&amp;p.limit=14)
   * [Java JDK 8 ](https://www.oracle.com/technetwork/java/javase/downloads/index.html#JDK8) AEM for AEM 6.5之前版本
2. 獲取[QuickStart JarAEM和a [!DNL license.properties]](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)的副本。
3. 在您的電腦上建立資料夾結構，如下所示：

   ```plain
   ~/aem-sdk
       /author
       /publish
   ```

4. 將[!DNL QuickStart] JAR重新命名為&#x200B;***aem-author-p4502.jar***，並將它放置在`/author`目錄下。 在`/author`目錄下添加&#x200B;***[!DNL license.properties]***&#x200B;檔案。
5. 製作[!DNL QuickStart] JAR的副本，將其重新命名為&#x200B;***aem-publish-p4503.jar***，並將其置於`/publish`目錄下。 在`/publish`目錄下添加&#x200B;***[!DNL license.properties]***&#x200B;檔案的副本。

   ```plain
   ~/aem-sdk
       /author
           + aem-author-p4502.jar
           + license.properties
       /publish
           + aem-publish-p4503.jar
           + license.properties
   ```

6. 連按兩下&#x200B;***aem-author-p4502.jar***&#x200B;檔案，以安裝&#x200B;**Author**&#x200B;例項。 這將啟動作者實例，該實例在本地電腦上的埠&#x200B;**4502**&#x200B;上運行。

   連按兩下&#x200B;***aem-publish-p4503.jar***&#x200B;檔案，以安裝&#x200B;**Publish**&#x200B;例項。 這將啟動在本地電腦上的埠&#x200B;**4503**&#x200B;上運行的Publish實例。

   >[!NOTE]
   >
   >視您的開發機器硬體而定，可能很難同時執行&#x200B;**Author和Publish**&#x200B;執行個體。 您很少需要在本機設定上同時執行兩者。

   有關詳細資訊，請參閱[部署和維護AEM實例](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html)。

## 安裝Apache Maven

***[!DNL Apache Maven]*** 是管理Java專案建立和部署程式的工具。AEM是以Java為基礎的平台，而[!DNL Maven]是管理專案程式碼的標準方AEM式。 當我們說出&#x200B;***AEM Maven Project***&#x200B;或僅表示您的&#x200B;***AEM Project***&#x200B;時，我們指的是Maven專案，其中包含您網站的所有&#x200B;*自訂*&#x200B;程式碼。

所AEM有專案都應以&#x200B;**[!DNL AEM Project Archetype]**&#x200B;的最新版本為基礎：[https://github.com/Adobe-Marketing-Cloud/aem-project-archetype](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype)。 [!DNL AEM Project Archetype]會建立專案的引導，其中包含一AEM些范常式式碼和內容。 [!DNL AEM Project Archetype]還包括&#x200B;**[!DNL AEM WCM Core Components]**，配置為用於項目。

>[!CAUTION]
>
>啟動新專案時，使用最新版原型是最佳實務。 請記住，原型有多種版本，並且並非所有版本都與舊版相容AEM。

### 步驟

1. 下載[Apache Maven](https://maven.apache.org/download.cgi)
2. 安裝[Apache Maven](https://maven.apache.org/install.html)並確保已將安裝添加到命令行`PATH`中。
   * [!DNL macOS] 使用者可使用 [Homebrew安裝Maven](https://brew.sh/)
3. 通過開啟新命令行終端並執行以下操作來驗證是否安裝了&#x200B;**[!DNL Maven]**:

   ```shell
   $ mvn --version
   Apache Maven 3.3.9
   Maven home: /Library/apache-maven-3.3.9
   Java version: 1.8.0_111, vendor: Oracle Corporation
   Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home/jre
   Default locale: en_US, platform encoding: UTF-8
   ```

4. 將&#x200B;**[!DNL adobe-public]**&#x200B;描述檔新增至您的[!DNL Maven] [settings.xml](https://maven.apache.org/settings.html)檔案，以便自動將&#x200B;**[!DNL repo.adobe.com]**&#x200B;新增至主要的建立程式。

5. 在`~/.m2/settings.xml`處建立名為`settings.xml`的檔案（如果該檔案尚不存在）。

6. 根據[此處的說明，將&#x200B;**[!DNL adobe-public]**&#x200B;描述檔新增至`settings.xml`檔案。](https://repo.adobe.com/)

   下面列出了示例`settings.xml`。 *請注意，用戶目 `settings.xml` 錄下的命名慣例和 `.m2` 位置很重要。*

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

7. 執行下列命令，確認&#x200B;**adobe-public**&#x200B;描述檔是否作用中：

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

   如果您未看到&#x200B;**[!DNL adobe-public]**，表示`~/.m2/settings.xml`檔案中未正確引用Adobe回購。 請重新訪問前面的步驟，並驗證settings.xml檔案是否引用Adobe回購。

## 建立整合的開發環境

整合的開發環境或IDE是結合文字編輯器、語法支援和建置工具的應用程式。 根據您正在進行的開發類型，一個IDE比另一個IDE更好。 無論使用何種IDE，都必須能夠定期將&#x200B;***push***&#x200B;程式碼傳送到本機AEM執行個體以進行測試。 此外，有時從本機例項將&#x200B;***pull***&#x200B;配置放入AEM您的AEM專案中，以便持續存留至Git等來源控制管理系統也很重要。

以下是一些較常用的IDE，這些IDE與開發搭配使AEM用，並顯示與本機例項的整AEM合。

>[!NOTE]
>
> WKND項目已更新為預設值，以作AEM為Cloud Service使用。 它已更新為與6.5/6.4](https://github.com/adobe/aem-guides-wknd#building-for-aem-6xx)向後相容。 [如果使用AEM6.5或6.4，請將`classic`描述檔附加至任何Maven命令。

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

使用IDE時，請確保在「Maven配置式」頁籤中選中`classic`。

![Maven配置檔案頁籤](assets/set-up-a-local-aem-development-environment/intelliJMavenProfiles.png)

*IntelliJ Maven設定檔*

### [!DNL Eclipse] IDE

**[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)**&#x200B;是Java開發中較常用的IDE之一，主要是因為它是開放原始碼和&#x200B;***free***! Adobe提供&#x200B;**[[!DNL AEM Developer Tools]](https://eclipse.adobe.com/aem/dev-tools/)**&#x200B;外掛程式，讓使用良好的GUI進行更輕鬆的開發，以便將程式碼與本機執行個體同AEM步。 [!DNL Eclipse]由於[!DNL AEM Developer Tools]對GUI的支援，因此建議對AEM大部分新進的開發人員使用[!DNL Eclipse] IDE。

#### 安裝與設定

1. 下載並安裝[!DNL Eclipse] IDE for [!DNL Java EE Developers] :[https://www.eclipse.org](https://www.eclipse.org/)
1. 請依照指示安裝[!DNL AEM Developer Tools]增效模組：[https://eclipse.adobe.com/aem/dev-tools/](https://eclipse.adobe.com/aem/dev-tools/)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 —— 導入Maven項目
* 01:24 —— 使用Maven建立和部署原始碼
* 04:33 —— 使用開發人員工具推送程式AEM碼變更
* 10:55 —— 使用開發人員工具提取程式AEM碼變更
* 13:12 —— 使用Eclipse的整合除錯工具

### IntelliJ IDEA

**[IntelliJ IDEA](https://www.jetbrains.com/idea/)**&#x200B;是專業Java開發的強大IDE。 [!DNL IntelliJ IDEA] 有兩種口味，一種是 ****** [!DNL Community] 免費版，另一種是商業版（付費） [!DNL Ultimate] 版。[!DNL IntellIJ IDEA]的免費[!DNL Community]版本足以進行更AEM多開發，但[!DNL Ultimate] [擴展了其功能集](https://www.jetbrains.com/idea/download)。

#### [!DNL Installation and Setup]

1. 下載並安裝[!DNL IntelliJ IDEA]:[https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. 安裝[!DNL Repo]（命令行工具）:[https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

* 00:00 —— 導入Maven項目
* 05:47 —— 使用Maven建立和部署原始碼
* 08:17 —— 使用回購功能推送變更
* 14:39 —— 利用回購協定進行更改
* 17:25 —— 使用IntelliJ IDEA的整合除錯工具

### [!DNL Visual Studio Code]

**[Visual Studio ](https://code.visualstudio.com/)** Code已迅速成為前端開發人員最愛的工 ***具，具備增強的JavaScript*** 支援、瀏覽器 [!DNL Intellisense]除錯支援。**[!DNL Visual Studio Code]** 是開放原始碼、免費的，並具備許多強大的擴充功能。[!DNL Visual Studio Code] 可與Adobe工AEM具repo整合 **[](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)。** 此外，還可安裝數個社群支援的擴充功能，以與整合AEM。

[!DNL Visual Studio Code] 是前端開發人員的絕佳選擇，他們主要會編寫CSS/LESS和JavaScript程式碼來建立用戶AEM端程式庫。對於新開發人員來說，此工具可能不是最佳AEM選擇，因為節點定義（對話方塊、元件）都需要在原始XML中編輯。 [!DNL Visual Studio Code]有幾個可用的Java擴充功能，但如果主要執行Java開發[!DNL Eclipse IDE]或[!DNL IntelliJ]，則可能更適合。

#### 重要連結

* [**下**](https://code.visualstudio.com/Download) **載Visual Studio代碼**
* **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)** - JCR內容的FTP樣式工具
* **[aemfed](https://aemfed.io/)**  —— 加速前AEM端工作流程
* **[SyncAEM](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)**  - Visual Studio代碼支援的*擴展

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 —— 導入Maven項目
* 00:53 —— 使用Maven建立和部署原始碼
* 04:03 —— 使用Repo命令列工具進行推播程式碼變更
* 08:29 —— 使用Repo命令行工具提取代碼更改
* 10:40 —— 使用修正工具推播程式碼變更
* 14:24 —— 疑難排解，重建客戶端庫

### [!DNL CRXDE Lite]

[CRXDE列](https://helpx.adobe.com/experience-manager/6-4/sites/developing/using/developing-with-crxde-lite.html) 表儲存庫的基於瀏覽器的AEM視圖。[!DNL CRXDE Lite] 可讓開AEM發人員執行標準開發工作，例如編輯檔案、定義元件、對話方塊和範本。[!DNL CRXDE Lite] 不 ****** 是完整的開發環境，但是除錯工具非常有效。[!DNL CRXDE Lite] 在擴充或瞭解產品程式碼庫以外的程式碼時，會很有用。[!DNL CRXDE Lite] 提供了儲存庫的強大視圖，以及有效測試和管理權限的方法。

[!DNL CRXDE Lite] 一律應與其他IDE搭配使用，以測試和除錯程式碼，但絕不是主要開發工具。它的語法支援有限，沒有自動完成功能，並且與來源控制管理系統整合有限。

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## 疑難排解

***說明!*** 我的程式碼沒用！和所有開發一樣，有時候（可能很多）您的程式碼無法如預期般運作。 是AEM一個強大的平台，但是……複雜性極高。 以下是疑難排解和追蹤問題的幾個高階起點（但遠非可能出錯的完整清單）:

### 驗證代碼部署

遇到問題時，最好的第一步是確認代碼已成功部署並安裝至AEM。

1. **檢查 [!UICONTROL Package]** Manager以確保已上載並安裝代碼包： [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).檢查時間戳記，確認軟體包最近已安裝。
1. 如果使用[!DNL Repo]或[!DNL AEM Developer Tools]等工具進行增量檔案更新， **檢查[!DNL CRXDE Lite]**&#x200B;檔案是否已推送到本地實例AEM，並且檔案內容已更新：[http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **如果發現與OSGi套件中** 的Java程式碼相關的問題，請檢查套件是否已升級。開啟[!UICONTROL Adobe Experience ManagerWeb控制台]:[http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)並搜尋您的套件。 確保包的狀態為&#x200B;**[!UICONTROL 活動]**。 有關在&#x200B;**[!UICONTROL Installed]**&#x200B;狀態下診斷包的詳細資訊，請參見以下。

#### 檢查日誌

AEM是一個動態平台，並在&#x200B;**error.log**&#x200B;中記錄許多有用的資訊。 **error.log**&#x200B;可在已安裝的位AEM置找到：&lt; `aem-installation-folder>/crx-quickstart/logs/error.log`。

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

預設情況下， **error.log**&#x200B;配置為log *[!DNL INFO]*&#x200B;語句。 如果要更改日誌級別，請轉至[!UICONTROL 日誌支援]:[http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog)。 您可能會發現&#x200B;**error.log**&#x200B;太動聽了。 您可以使用[!UICONTROL 日誌支援]為指定的Java包配置日誌語句。 這是專案的最佳實務，以便輕鬆將自訂程式碼問題與OOTB平台問題AEM區隔。

![記錄配置AEM](./assets/set-up-a-local-aem-development-environment/logging.png)

#### Bundle處於「已安裝」狀態{#bundle-active}

所有捆綁包（除片段外）都應處於&#x200B;**[!UICONTROL 活動]**&#x200B;狀態。 如果您在[!UICONTROL Installed]狀態中看到程式碼套件，則有問題需要解決。 多數情況下，這是依賴性問題：

![Bundle錯AEM誤](assets/set-up-a-local-aem-development-environment/bundle-error.png)

在上述螢幕擷取中，[!DNL WKND Core bundle]是[!UICONTROL Installed]狀態。 這是因為Bundle需要的`com.adobe.cq.wcm.core.components.models`版本與實例上的版本不AEM同。

[!UICONTROL 相依性搜尋器]是可使用的實用工具：[http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder)。 新增Java套件名稱，以檢查實例上有哪些版本AEM可用：

![核心元件](assets/set-up-a-local-aem-development-environment/core-components.png)

繼續上述範例，我們可以看到，在例AEM項上安裝的版本是&#x200B;**12.2**&#x200B;與包期望的&#x200B;**12.6**。 從中，您可以向後工作，並查看項目中[!DNL Maven]相AEM依性是否與[!DNL Maven]相AEM符。 在上述範例中，[!DNL Core Components] **v2.2.0**&#x200B;安裝在例項上AEM，但程式碼套裝是以對&#x200B;**v2.2.2**&#x200B;的相依性來建立，因此是相關性問題的原因。

#### 驗證Sling Models Registration {#osgi-component-sling-models}

元AEM件應始終由[!DNL Sling Model]備份，以封裝任何商業邏輯，並確保HTL轉換指令碼保持乾淨。 如果遇到找不到Sling Model的問題，從主控台檢查[!DNL Sling Models]可能會很有幫助：[http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)。 這會告訴您Sling Model是否已註冊，以及它系結至的資源類型（元件路徑）。

![Sling Model Status](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

顯示與`wknd/components/content/byline`元件資源類型相關聯的[!DNL Sling Model]、`BylineImpl`的註冊。

#### CSS或JavaScript問題

對於大部分的CSS和JavaScript問題，使用瀏覽器的開發工具是最有效的疑難排解方式。 若要針對作者例項進行開發時縮AEM小問題，請檢視「已發佈」頁面。

![CSS或JS問題](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

開啟[!UICONTROL 頁面屬性]功能表，然後按一下「檢視為已發佈」。 這將開啟不含編輯器的頁AEM面，並將查詢參數設定為&#x200B;**wcmmode=disabled**。 如此可有效停用編寫AEMUI，並讓疑難排解／除錯前端問題變得更輕鬆。

開發前端程式碼時，另一個常見的問題是舊版或過時的CSS/JS正在載入。 首先，確保已清除瀏覽器歷史記錄，並在必要時啟動Incognito瀏覽器或新會話。

#### 調試客戶端庫

使用不同的類別和內嵌方式來包含多個用戶端程式庫，疑難排解會很麻煩。 AEM提供數種工具來協助。 最重要的工具之一是[!UICONTROL 重建客戶端庫]，它將強制AEM重新編譯任何LESS檔案並生成CSS。

* [轉儲庫](http://localhost:4502/libs/granite/ui/content/dumplibs.html) -列出實例中註冊的所有客戶端庫AEM。&lt;host>/libs/granite/ui/content/dumplibs.html
* [Test Output](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html)  —— 允許使用者根據類別查看clientlib的預期HTML輸出。&lt;host>/libs/granite/ui/content/dumplibs.test.html
* [庫相關性驗證](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) -突出顯示所有找不到的相關性或嵌入類別。&lt;host>/libs/granite/ui/content/dumplibs.validate.html
* [重建客戶端庫](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) -允許用戶強制重建所AEM有客戶端庫或使客戶端庫的快取無效。使用LESS進行開發時，此工具特別有效，因為這可AEM以強制重新編譯產生的CSS。 一般而言，使快取無效，然後執行頁面重新整理與重建所有程式庫比較有效。 &lt;host>/libs/granite/ui/content/dumplibs.rebuild.html

![除錯Clientlibs](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>如果您不斷使用[!UICONTROL 重建客戶端庫]工具使快取失效，則對所有客戶端庫進行一次重建可能是值得的。 這可能需要大約15分鐘，但通常可消除未來的快取問題。
