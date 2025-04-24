---
title: 設定本機AEM開發環境
description: 瞭解如何設定Experience Manager的本機開發環境。 熟悉本機安裝、Apache Maven、整合式開發環境，以及偵錯和疑難排解。 使用Eclipse IDE、CRXDE-Lite、Visual Studio Code和IntelliJ。
version: Experience Manager 6.5
feature: Developer Tools
topic: Development
role: Developer
level: Beginner
exl-id: 58851624-71c9-4745-aaaf-305acf6ccb14
last-substantial-update: 2022-07-20T00:00:00Z
doc-type: Tutorial
thumbnail: aem-local-dev-env.jpg
duration: 4537
source-git-commit: 3ad201aad77e71b42d46d69fdda50bcc77316151
workflow-type: tm+mt
source-wordcount: '2408'
ht-degree: 0%

---

# 設定本機AEM開發環境

為AEM Adobe Experience Manager設定本機開發的指南。 涵蓋本機安裝、Apache Maven、整合式開發環境和偵錯/疑難排解等重要主題。 討論使用&#x200B;**Eclipse IDE、CRXDE Lite、Visual Studio Code和IntelliJ**&#x200B;進行開發。

## 概觀

設定本機開發環境是針對Adobe Experience Manager或AEM進行開發時的第一個步驟。 請花點時間設定高品質的開發環境，以提升生產力並更快撰寫更出色的程式碼。 我們可以將AEM本機開發環境分成四個區域：

* 本機AEM執行個體
* [!DNL Apache Maven]專案
* 整合式開發環境(IDE)
* 疑難排解

## 安裝本機AEM執行個體

當我們提及本機AEM執行個體時，我們談論的是在開發人員個人電腦上執行的Adobe Experience Manager復本。 ***所有*** AEM開發應該從撰寫和執行本機AEM執行個體的程式碼開始。

如果您是AEM的新手，可以安裝兩種基本執行模式： ***作者***&#x200B;和&#x200B;***發佈***。 ***作者*** [執行模式](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/configuring/configure-runmodes.html?lang=en)是數位行銷人員用來建立和管理內容的環境。 大部分開發期間，您都是將程式碼部署到Author例項。 這可讓您建立頁面以及新增和設定元件。 AEM Sites是WYSIWYG製作CMS，因此，大部分CSS和JavaScript都可透過製作例項進行測試。

它也是針對本機&#x200B;***發佈***&#x200B;執行個體的&#x200B;*關鍵*&#x200B;測試程式碼。 ***Publish***&#x200B;執行個體是您網站的訪客與之互動的AEM環境。 雖然&#x200B;***Publish***&#x200B;執行個體與&#x200B;***Author***&#x200B;執行個體是相同的技術棧疊，但在設定和許可權方面有一些重大差異。 程式碼必須先在本機&#x200B;***Publish***&#x200B;執行個體上測試，才能升級至較高層級的環境。

### 步驟

1. 確認已安裝Java™。
   * 偏好使用AEM 6.5+的[Java™ JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
   * 適用於AEM 6.5之前的AEM版本的[Java™ JDK 8](https://www.oracle.com/java/technologies/downloads/)
1. 取得[AEM QuickStart Jar和 [!DNL license.properties]](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html)的復本。
1. 在電腦上建立檔案夾結構，如下所示：

```plain
~/aem-sdk
    /author
    /publish
```

1. 將[!DNL QuickStart] JAR重新命名為&#x200B;***aem-author-p4502.jar***，並將其放在`/author`目錄下。 在`/author`目錄下新增&#x200B;***[!DNL license.properties]***&#x200B;檔案。

1. 複製[!DNL QuickStart] JAR，將其重新命名為&#x200B;***aem-publish-p4503.jar***，並放置在`/publish`目錄下。 在`/publish`目錄下新增&#x200B;***[!DNL license.properties]***&#x200B;檔案的復本。

```plain
~/aem-sdk
    /author
        + aem-author-p4502.jar
        + license.properties
    /publish
        + aem-publish-p4503.jar
        + license.properties
```

1. 連按兩下&#x200B;***aem-author-p4502.jar***&#x200B;檔案以安裝&#x200B;**Author**&#x200B;執行個體。 這會啟動編寫執行個體，在本機電腦的連線埠&#x200B;**4502**&#x200B;上執行。

連按兩下&#x200B;***aem-publish-p4503.jar***&#x200B;檔案以安裝&#x200B;**Publish**&#x200B;執行個體。 這會啟動在本機電腦的連線埠&#x200B;**4503**&#x200B;上執行的發佈執行個體。

>[!NOTE]
>
>視您的開發電腦硬體而定，可能很難同時執行&#x200B;**作者與發佈**&#x200B;執行個體。 很少需要在本機設定上同時執行這兩項。

### 使用命令列

連按兩下JAR檔案的替代方法是，從命令列啟動AEM或建立指令碼（`.bat`或`.sh`），視您的本機作業系統風格而定。 以下是範例指令的範例：

```shell
$ java -Xmx2048M -Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=30303 -jar aem-author-p4502.jar -gui -r"author,localdev"
```

此處，`-X`是JVM選項，`-D`是其他架構屬性，如需詳細資訊，請參閱[部署和維護AEM執行個體](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html)和[快速入門檔案中提供的其他選項](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/custom-standalone-install.html#further-options-available-from-the-quickstart-file)。

## 安裝Apache Maven

***[!DNL Apache Maven]***&#x200B;是用來管理Java型專案的建置和部署程式的工具。 AEM是Java型平台，[!DNL Maven]是管理AEM專案程式碼的標準方式。 當我們說&#x200B;***AEM Maven專案***&#x200B;或只是您的&#x200B;***AEM專案***&#x200B;時，我們指的是一個包含您網站所有&#x200B;*自訂*&#x200B;程式碼的Maven專案。

所有AEM專案應該以&#x200B;**[!DNL AEM Project Archetype]**&#x200B;的最新版本建置： [https://github.com/adobe/aem-project-archetype](https://github.com/adobe/aem-project-archetype)。 [!DNL AEM Project Archetype]提供AEM專案的啟動程式，其中包含一些範常式式碼和內容。 [!DNL AEM Project Archetype]也包含設定為在專案中使用的&#x200B;**[!DNL AEM WCM Core Components]**。

>[!CAUTION]
>
>開始新專案時，最佳實務是使用原型的最新版本。 請記住，原型有多種版本，並非所有版本都與舊版AEM相容。

### 步驟

1. 下載[Apache Maven](https://maven.apache.org/download.cgi)
2. 安裝[Apache Maven](https://maven.apache.org/install.html)，並確定已將安裝新增至命令列`PATH`。
   * [!DNL macOS]使用者可以使用[Homebrew](https://brew.sh/)安裝Maven
3. 透過開啟新的命令列終端機並執行下列動作，驗證是否已安裝&#x200B;**[!DNL Maven]**：

```shell
$ mvn --version
Apache Maven 3.3.9
Maven home: /Library/apache-maven-3.3.9
Java version: 1.8.0_111, vendor: Oracle Corporation
Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home/jre
Default locale: en_US, platform encoding: UTF-8
```

>[!NOTE]
>
> 在過去，需要新增`adobe-public` Maven設定檔才能指向`nexus.adobe.com`以下載AEM成品。 現在所有的AEM成品都可以透過Maven Central使用，而不需要`adobe-public`設定檔。

## 設定整合式開發環境

整合式開發環境或IDE是一種結合文字編輯器、語法支援和建置工具的應用程式。 根據您正在執行的開發型別，一個IDE可能比另一個IDE更適合。 不論IDE為何，請務必定期&#x200B;***將***&#x200B;程式碼推播至本機AEM執行個體以進行測試。 請務必不時從本機AEM執行個體將&#x200B;***提取***&#x200B;設定至您的AEM專案，以保留至原始檔控制管理系統，例如Git。

以下是一些與AEM開發搭配使用，並搭配顯示與本機AEM執行個體整合之對應影片的熱門IDE。

>[!NOTE]
>
> WKND專案已更新為預設可在AEM as a Cloud Service上運作。 已更新為[回溯相容於6.5/6.4](https://github.com/adobe/aem-guides-wknd#building-for-aem-6xx)。 如果使用AEM 6.5或6.4，請將`classic`設定檔附加至任何Maven命令。

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

使用IDE時，請務必在Maven設定檔標籤中檢視`classic`。

![Maven設定檔標籤](assets/set-up-a-local-aem-development-environment/intelliJMavenProfiles.png)

*IntelliJ Maven設定檔*

### [!DNL Eclipse] IDE

**[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)**&#x200B;是Java™開發中最常用的IDE之一，大部分是因為它是開放原始碼且&#x200B;***可用***！ Adobe為[!DNL Eclipse]提供外掛程式&#x200B;**[[!DNL AEM Developer Tools]](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)**，以便使用良好的GUI更輕鬆地開發，以將程式碼與本機AEM執行個體同步。 主要由於[!DNL AEM Developer Tools]的GUI支援，所以建議不熟悉AEM的開發人員使用[!DNL Eclipse] IDE。

#### 安裝及設定

1. 下載並安裝[!DNL Java™ EE Developers]的[!DNL Eclipse] IDE： [https://www.eclipse.org](https://www.eclipse.org/)
1. 依照指示安裝[!DNL AEM Developer Tools]外掛程式： [https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 — 匯入Maven專案
* 01:24 — 使用Maven建置和部署原始程式碼
* 04:33 — 使用AEM開發人員工具推送程式碼變更
* 10:55 — 使用AEM開發人員工具提取程式碼變更
* 13:12 — 使用Eclipse的整合式偵錯工具

### IntelliJ IDEA

**[IntelliJ IDEA](https://www.jetbrains.com/idea/)**&#x200B;是適用於專業Java™開發的強大IDE。 [!DNL IntelliJ IDEA]有兩種口味：***免費*** [!DNL Community]版本和商業（付費） [!DNL Ultimate]版本。 可用的[!DNL Community]版本[!DNL IntellIJ IDEA]足以進行更多AEM開發，但[!DNL Ultimate] [擴充其功能集](https://www.jetbrains.com/idea/download)。

#### [!DNL Installation and Setup]

1. 下載並安裝[!DNL IntelliJ IDEA]： [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. 安裝[!DNL Repo] （命令列工具）： [https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089?quality=12&learn=on)

* 00:00 — 匯入Maven專案
* 05:47 — 使用Maven建置和部署原始程式碼
* 08:17 — 使用存放庫推送變更
* 14:39 — 使用存放庫提取變更
* 17:25 — 使用IntelliJ IDEA的整合式偵錯工具

### [!DNL Visual Studio Code]

**[Visual Studio Code](https://code.visualstudio.com/)**&#x200B;已迅速成為&#x200B;***前端開發人員***&#x200B;最喜愛的工具，具有增強的JavaScript支援、[!DNL Intellisense]和瀏覽器偵錯支援。 **[!DNL Visual Studio Code]**&#x200B;是開放原始碼、免費、有許多強大的擴充功能。 [!DNL Visual Studio Code]可設定為在Adobe工具&#x200B;**[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)的協助下與AEM整合。**&#x200B;也有數個社群支援的擴充功能可安裝成與AEM整合。

對於主要撰寫CSS/LESS和JavaScript程式碼以建立AEM使用者端程式庫的前端開發人員而言，[!DNL Visual Studio Code]是很好的選擇。 此工具可能不是新AEM開發人員的最佳選擇，因為節點定義（對話方塊、元件）需要以原始XML編輯。 有數個Java™擴充功能可用於[!DNL Visual Studio Code]，但如果主要執行Java™開發[!DNL Eclipse IDE]或[!DNL IntelliJ]，可能較偏好使用。

#### 重要連結

* [**下載**](https://code.visualstudio.com/Download) **Visual Studio程式碼**
* **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)** — 適用於JCR內容的類似FTP的工具
* **[AEM Sync](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)** - Visual Studio Code的社群支援&#42;擴充功能
* **[WKND專案](https://github.com/adobe/aem-guides-wknd)** — 此影片中顯示的範例AEM專案。

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 — 匯入Maven專案
* 00:53 — 使用Maven建置和部署原始程式碼
* 04:03 — 使用存放庫命令列工具推送程式碼變更
* 08:29 — 使用存放庫命令列工具變更提取程式碼
* 10:32 — 疑難排解，重建使用者端程式庫

### [!DNL CRXDE Lite]

[CRXDE Lite](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/developing-with-crxde-lite.html)是AEM存放庫的瀏覽器式檢視。 [!DNL CRXDE Lite]內嵌於AEM中，可讓開發人員執行標準開發工作，例如編輯檔案、定義元件、對話方塊和範本。 [!DNL CRXDE Lite]是&#x200B;***不是***&#x200B;打算做為完整開發環境，但做為偵錯工具卻很有效。 [!DNL CRXDE Lite]在延伸或只是瞭解程式碼基底以外的產品程式碼時很有用。 [!DNL CRXDE Lite]提供存放庫的強大檢視，以及有效測試和管理許可權的方法。

[!DNL CRXDE Lite]應該與其他IDE搭配使用以測試和偵錯程式碼，但絕不可作為主要開發工具。 其語法支援有限，沒有自動完成功能，而且與原始檔控制管理系統的整合也有限。

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## 疑難排解

***說明！***&#x200B;我的程式碼無法運作！ 就像所有開發一樣，有時候（可能很多）您的程式碼無法如預期運作。 AEM是一個功能強大的平台，但強大的……帶來極大的複雜性。 以下是疑難排解及追蹤問題時的幾個高階起點（遠非可能錯誤的完整清單）：

### 驗證程式碼部署

遇到問題時，正確的第一步是驗證程式碼是否已成功部署並安裝至AEM。

1. **檢查[!UICONTROL 封裝管理員]**，確認是否已上傳及安裝程式碼封裝： [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)。 檢查時間戳記，確認最近是否安裝了套件。
1. 如果使用[!DNL Repo]或[!DNL AEM Developer Tools]之類的工具進行增量檔案更新，**請檢查[!DNL CRXDE Lite]**&#x200B;檔案是否已推送至本機AEM執行個體，且檔案內容已更新： [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **如果看到與OSGi套件中的Java™程式碼相關的問題，請檢查套件是否已上傳**。 開啟[!UICONTROL Adobe Experience Manager Web Console]： [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)並搜尋您的套件。 確定組合具有&#x200B;**[!UICONTROL 作用中]**&#x200B;狀態。 請參閱下文，以取得疑難排解&#x200B;**[!UICONTROL 已安裝]**&#x200B;狀態的套件組合的相關詳細資訊。

#### 檢查記錄

AEM是一個聊天平台，並將有用的資訊記錄在&#x200B;**error.log**&#x200B;中。 **error.log**&#x200B;可找到AEM的安裝位置： &lt; `aem-installation-folder>/crx-quickstart/logs/error.log`。

追蹤問題的實用技術是在Java™程式碼中新增記錄陳述式：

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

根據預設，**error.log**&#x200B;設定為記錄&#x200B;*[!DNL INFO]*&#x200B;陳述式。 如果您想要變更記錄層級，可以移至[!UICONTROL 記錄支援]： [http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog)。 您也可能發現&#x200B;**error.log**&#x200B;太常聊。 您可以使用[!UICONTROL Log Support]，為指定的Java™封裝設定記錄陳述式。 這是專案的最佳實務，以便輕鬆地將自訂程式碼問題與OOTB AEM平台問題分開。

![在AEM中記錄設定](./assets/set-up-a-local-aem-development-environment/logging.png)

#### 套件組合處於已安裝狀態 {#bundle-active}

所有組合（不包括片段）都應處於&#x200B;**[!UICONTROL 作用中]**&#x200B;狀態。 如果您看到程式碼套件組合處於[!UICONTROL 已安裝]狀態，表示有問題需要解決。 大多數時候這會是相依性問題：

AEM中的![套件組合錯誤](assets/set-up-a-local-aem-development-environment/bundle-error.png)

在上述熒幕擷圖中，[!DNL WKND Core bundle]是[!UICONTROL 已安裝]狀態。 這是因為套件組合預期的`com.adobe.cq.wcm.core.components.models`版本與AEM執行個體上可用的版本不同。

可用的實用工具是[!UICONTROL 相依性尋找器]： [http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder)。 新增Java™套件名稱以檢查AEM執行個體上可用的版本：

![核心元件](assets/set-up-a-local-aem-development-environment/core-components.png)

繼續上述範例，我們可看到AEM執行個體上安裝的版本是&#x200B;**12.2** vs **12.6**，也就是套件所預期的版本。 從那裡，您可以回溯工作，檢視AEM上的[!DNL Maven]相依性是否符合AEM專案中的[!DNL Maven]相依性。 在中，上述範例[!DNL Core Components] **v2.2.0**&#x200B;已安裝在AEM執行個體上，但程式碼套件組合是以&#x200B;**v2.2.2**&#x200B;上的相依性建置，因此這是相依性問題的原因。

#### 驗證Sling模型註冊 {#osgi-component-sling-models}

AEM元件必須由[!DNL Sling Model]提供支援，以封裝任何商業邏輯並確保HTL轉譯指令碼保持整潔。 如果發生找不到Sling模型的問題，則從主控台[http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)檢視[!DNL Sling Models]會很有幫助。 這會告訴您是否已註冊Sling模型，以及它繫結的資源型別（元件路徑）。

![Sling模型狀態](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

顯示繫結至元件資源型別`wknd/components/content/byline`的[!DNL Sling Model]、`BylineImpl`的登入。

#### CSS或JavaScript問題

針對大多數CSS和JavaScript問題，使用瀏覽器的開發工具是進行疑難排解的最有效方式。 若要在針對AEM作者例項開發時縮小問題範圍，檢視「已發佈」頁面會很有幫助。

![CSS或JS問題](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

開啟[!UICONTROL 頁面屬性]功能表，然後按一下[!UICONTROL 以發佈的形式檢視]。 這會在沒有AEM編輯器的情況下開啟頁面，且查詢引數設定為&#x200B;**wcmmode=disabled**。 這實際上停用了AEM編寫UI，並讓疑難排解/偵錯前端問題變得更容易。

開發前端程式碼時另一個常見問題為過時或正在載入過期的CSS/JS。 首先，請確定瀏覽器歷史記錄已清除，並視需要啟動無痕瀏覽器或全新工作階段。

#### 偵錯使用者端程式庫

使用不同的類別和內嵌方法來包含多個使用者端程式庫，進行疑難排解可能會相當麻煩。 AEM提供數種工具可協助解決此問題。 最重要的工具之一是[!UICONTROL 重建使用者端資料庫]，它會強制AEM重新編譯任何LESS檔案並產生CSS。

* [傾印程式庫](http://localhost:4502/libs/granite/ui/content/dumplibs.html) — 列出在AEM執行個體中註冊的所有使用者端程式庫。 &lt;host>/libs/granite/ui/content/dumplibs.html
* [測試輸出](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) — 允許使用者根據類別檢視clientlib includes的預期HTML輸出。 &lt;host>/libs/granite/ui/content/dumplibs.test.html
* [資料庫相依性驗證](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) — 反白標示找不到的任何相依性或內嵌類別。 &lt;host>/libs/granite/ui/content/dumplibs.validate.html
* [重新建置使用者端資料庫](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) — 允許使用者強制AEM重新建置所有使用者端資料庫，或是讓使用者端資料庫的快取失效。 使用LESS開發時，此工具相當有效，因為這會強制AEM重新編譯產生的CSS。 一般而言，讓快取失效，然後執行頁面重新整理比重建所有程式庫更有效率。 &lt;host>/libs/granite/ui/content/dumplibs.rebuild.html

![偵錯Clientlibs](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>如果您必須經常使用[!UICONTROL Rebuild Client Libraries]工具讓快取失效，一次重建所有使用者端資料庫或許是值得的。 這可能需要約15分鐘，但通常可消除未來任何快取問題。
