---
title: 設定本地開發AEM環境
description: Adobe Experience Manager地方開發指南AEM。 介紹本地安裝、Apache Maven、整合開發環境和調試/故障排除等重要主題。 討論了Eclipse IDE、CRXDE-Lite、Visual Studio代碼和IntelliJ開發。
version: 6.4, 6.5
feature: Developer Tools
topics: development
activity: develop
audience: developer
topic: Development
role: Developer
level: Beginner
exl-id: 58851624-71c9-4745-aaaf-305acf6ccb14
source-git-commit: 25a1a40f42d37443db9edc0e09b1691b1c19e848
workflow-type: tm+mt
source-wordcount: '2582'
ht-degree: 1%

---

# 設定本地開發AEM環境

Adobe Experience Manager地方開發指南AEM。 介紹本地安裝、Apache Maven、整合開發環境和調試/故障排除等重要主題。 開發 **[!DNL Eclipse IDE]。 [!DNL CRXDE Lite]。 [!DNL Visual Studio Code] 和[!DNL IntelliJ]** 中。

## 概覽

建立地方發展環境是Adobe Experience Manager或發展的第一步AEM. 請花時間建立一個高質量的開發環境，以提高您的生產力並更快地編寫更好的代碼。 把當地的AEM發展環境分為四個方面：

* 本地實AEM例
* [!DNL Apache Maven] 項目
* 整合開發環境(IDE)
* 疑難排解

## 安裝本地實AEM例

當我們指的是本AEM地實例時，我們指的是開發人員個人電腦上運行的Adobe Experience Manager副本。 ***全部*** 開AEM發應首先編寫並運行針對本地實例的AEM代碼。

如果您是新用戶AEM，則可以安裝兩種基本運行模式： ***作者*** 和 ***發佈***。 的 ***作者*** [運行模式](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/configure-runmodes.html)  是數字營銷人員用來建立和管理內容的環境。 開發時 **最** 將代碼部署到Author實例的時間。 這允許您建立新頁面以及添加和配置元件。 AEM Sites是WYSIWYG的創作CMS，因此，大多數CSS和JavaScript都可以針對創作實例進行測試。

也 *關鍵* test本地代碼 ***發佈*** 實例。 的 ***發佈*** 實例是訪AEM問您網站的人員將與其交互的環境。 當 ***發佈*** 實例與 ***作者*** 例如，在配置和權限方面存在一些主要區別。 代碼應 *總是* 經過本地測試 ***發佈*** 在升級到更高級別環境之前執行實例。

### 步驟

1. 確保 [爪哇](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html) 已安裝。
   * 首選 [Java JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3內容%2Fmetadata%2Fdc%3SoftwareType&amp;1_group.propertyvalues.operation=等於&amp;1_group.propertyvalues.0_values=軟體類型%3Atoling&amp;orderby=%40jcr%3Acont%2Fjcr%3AlastModified&amp;orderby.sort.st&amp;rded&amp;lay=list&amp;p.offset=0&amp;p.limit=14) 6AEM.5+
   * [Java JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html#JDK8) 適AEM用於AEM6.5版之前
2. 獲取副本 [快AEM速啟動Jar和 [!DNL license.properties]](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)。
3. 在電腦上建立資料夾結構，如下所示：

   ```plain
   ~/aem-sdk
       /author
       /publish
   ```

4. 更名 [!DNL QuickStart] JAR到 ***aem-author-p4502.jar*** 放在下面 `/author` 的子菜單。 添加 ***[!DNL license.properties]*** 檔案下方 `/author` 的子菜單。
5. 複製 [!DNL QuickStart] JAR，將其更名為 ***aem-publish-p4503.jar*** 放在下面 `/publish` 的子菜單。 添加副本 ***[!DNL license.properties]*** 檔案下方 `/publish` 的子菜單。

   ```plain
   ~/aem-sdk
       /author
           + aem-author-p4502.jar
           + license.properties
       /publish
           + aem-publish-p4503.jar
           + license.properties
   ```

6. 按兩下 ***aem-author-p4502.jar*** 檔案以安裝 **作者** 實例。 這將啟動在埠上運行的作者實例 **4502** 在本地電腦上。

   按兩下 ***aem-publish-p4503.jar*** 檔案以安裝 **發佈** 實例。 這將啟動在埠上運行的發佈實例 **4503** 在本地電腦上。

   >[!NOTE]
   >
   >根據開發機器的硬體，可能很難同時使用 **作者和發佈** 同時運行的實例。 您很少需要在本地安裝程式上同時運行這兩個程式。

   有關詳細資訊，請參閱 [部署和維護實AEM例](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html)。

## 安裝Apache Maven

***[!DNL Apache Maven]*** 是用於管理基於Java的項目的生成和部署過程的工具。 是AEM一個基於Java的平台 [!DNL Maven] 是管理項目代碼的標準方AEM法。 當我們說 ***馬文AEM項目*** 或者 ***項AEM目***&#x200B;我們指的是馬文項目，包括 *自定義* 站點的代碼。

所有AEM項目都應基於 **[!DNL AEM Project Archetype]**: [https://github.com/adobe/aem-project-archetype](https://github.com/adobe/aem-project-archetype)。 的 [!DNL AEM Project Archetype] 將建立包含某些示例AEM代碼和內容的項目引導。 的 [!DNL AEM Project Archetype] 還包括 **[!DNL AEM WCM Core Components]** 配置為用於您的項目。

>[!CAUTION]
>
>啟動新項目時，最好使用最新版本的原型。 請記住，原型有多個版本，並且並非所有版本都與早期版本兼AEM容。

### 步驟

1. 下載 [阿帕奇·馬文](https://maven.apache.org/download.cgi)
2. 安裝 [阿帕奇·馬文](https://maven.apache.org/install.html) 並確保已將安裝添加到命令行 `PATH`。
   * [!DNL macOS] 用戶可以使用 [荷姆布魯](https://brew.sh/)
3. 驗證 **[!DNL Maven]** 通過開啟新命令行終端並執行以下操作來安裝：

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
   > 過去， `adobe-public` 需要Maven配置檔案來指出 `nexus.adobe.com` 下載工AEM件。 現在，AEM通過Maven Central和 `adobe-public` 不需要配置檔案。

## 建立綜合開發環境

整合開發環境或IDE是一個將文本編輯器、語法支援和生成工具結合在一起的應用程式。 根據您正在進行的開發類型，一個IDE可能比另一個IDE更好。 無論IDE如何，定期能夠 ***推*** 代碼到本AEM地實例，以便test它。 偶爾也很重要 ***拉*** 將本地實AEM例配置AEM到項目中，以保持到原始碼管理系統（如Git）。

下面是一些較為流行的IDE，它們與開發一起使AEM用，並帶有相應的視頻，顯示與本地實例的AEM整合。

>[!NOTE]
>
> WKND項目已更新為預設值以處理AEMas a Cloud Service。 已更新為 [向後相容6.5/6.4](https://github.com/adobe/aem-guides-wknd#building-for-aem-6xx)。 如果使AEM用6.5或6.4，則追加 `classic` 配置檔案。

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

使用IDE時，請確保檢查 `classic` 的子菜單。

![「Maven配置檔案」頁籤](assets/set-up-a-local-aem-development-environment/intelliJMavenProfiles.png)

*IntelliJ Maven配置檔案*

### [!DNL Eclipse] IDE

的 **[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)** 是Java開發中比較流行的IDE之一，這在很大程度上是因為它是開源的， ***免費***! Adobe提供插件， **[[!DNL AEM Developer Tools]](https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html)**, [!DNL Eclipse] 使用好的GUI更輕鬆地開發，以便將代碼與本地實例AEM同步。 的 [!DNL Eclipse] 建議新開發人員使用IDEAEM，這在很大程度上是因為 [!DNL AEM Developer Tools]。

#### 安裝和設定

1. 下載並安裝 [!DNL Eclipse] IDE用於 [!DNL Java EE Developers]: [https://www.eclipse.org](https://www.eclipse.org/)
1. 按照說明安裝 [!DNL AEM Developer Tools] 插件： [https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 — 導入Maven項目
* 01:24 — 使用Maven構建和部署原始碼
* 04:33 — 使用開發人員工具推送代AEM碼更改
* 10:55 — 使用開發人員工具進行拉AEM式代碼更改
* 13:12 — 使用Eclipse的整合調試工具

### IntelliJ IDEA

的 **[IntelliJ IDEA](https://www.jetbrains.com/idea/)** 是用於專業Java開發的強大IDE。 [!DNL IntelliJ IDEA] 有兩種口味， ***免費*** [!DNL Community] 版本及廣告（付費） [!DNL Ultimate] 。 免費 [!DNL Community] 版本 [!DNL IntellIJ IDEA] 足以AEM發展， [!DNL Ultimate] [擴展其功能集](https://www.jetbrains.com/idea/download)。

#### [!DNL Installation and Setup]

1. 下載並安裝 [!DNL IntelliJ IDEA]: [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. 安裝 [!DNL Repo] （命令行工具）: [https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

* 00:00 — 導入Maven項目
* 05:47 — 使用Maven構建和部署原始碼
* 08:17 — 使用回購推動更改
* 14:39 — 利用回購協定進行更改
* 17:25 — 使用IntelliJ IDEA的整合調試工具

### [!DNL Visual Studio Code]

**[Visual Studio代碼](https://code.visualstudio.com/)** 很快成為了 ***前端開發人員*** 支援增強的JavaScript, [!DNL Intellisense]以及瀏覽器調試支援。 **[!DNL Visual Studio Code]** 是開源的，免費的，有很多強大的擴展。 [!DNL Visual Studio Code] 可以配合一AEM個Adobe工具， **[回購](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)。** 還有幾個社區支援的擴展可以安裝以與整合AEM。

[!DNL Visual Studio Code] 對於主要編寫CSS/LESS和JavaScript代碼以建立客戶端庫的前端開發人員來說，這是一個絕AEM佳選擇。 此工具可能不是新開發人員的最佳選擇AEM，因為節點定義（對話框、元件）都需要以原始XML進行編輯。 有幾個Java擴展可供 [!DNL Visual Studio Code]但是，如果主要進行Java開發 [!DNL Eclipse IDE] 或 [!DNL IntelliJ] 可能是首選。

#### 重要連結

* [**下載**](https://code.visualstudio.com/Download) **Visual Studio代碼**
* **[回購](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)**  — 類似FTP的JCR內容工具
* **[美國](https://aemfed.io/)**  — 加快前AEM端工作流
* **[同AEM步](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)**  — 社區支援* Visual Studio代碼擴展

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 — 導入Maven項目
* 00:53 — 使用Maven構建和部署原始碼
* 04:03 — 使用Repo命令行工具推送代碼更改
* 08:29 — 使用Repo命令行工具拉取代碼更改
* 10:40 — 使用修正工具推送代碼更改
* 14:24 — 故障排除，重建客戶端庫

### [!DNL CRXDE Lite]

[CRXDE Lite](https://helpx.adobe.com/experience-manager/6-4/sites/developing/using/developing-with-crxde-lite.html) 是基於瀏覽器的儲存庫AEM視圖。 [!DNL CRXDE Lite] 嵌入並允AEM許開發人員執行標準開發任務，如編輯檔案、定義元件、對話框和模板。 [!DNL CRXDE Lite] 是 ***不*** 本來是一個完整的開發環境，但作為調試工具非常有效。 [!DNL CRXDE Lite] 在擴展或僅瞭解代碼庫之外的產品代碼時非常有用。 [!DNL CRXDE Lite] 提供了儲存庫的強大視圖，以及有效test和管理權限的方法。

[!DNL CRXDE Lite] 應始終與其他IDE一起使用，以test和調試代碼，但永遠不應將其用作主開發工具。 它的語法支援有限，沒有自動完成功能，與原始碼管理系統的整合也有限。

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## 疑難排解

***說明!*** 我的密碼無效！ 與所有發展一樣，有時（可能很多），您的代碼會不如預期那樣運行。 是AEM一個強大的平台，但是……非常複雜。 以下是故障排除和跟蹤問題的幾個高級起點（但遠遠不是可能出錯的詳盡清單）:

### 驗證代碼部署

遇到問題時，一個好的第一步是驗證代碼是否已成功部署並安裝到AEM。

1. **檢查 [!UICONTROL 包管理器]** 要確保已上載並安裝代碼包，請執行以下操作： [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)。 檢查時間戳以驗證最近已安裝軟體包。
1. 如果使用類似工具執行增量檔案更新 [!DNL Repo] 或 [!DNL AEM Developer Tools]。 **檢查[!DNL CRXDE Lite]** 已將檔案推送到本地實例AEM並更新檔案內容： [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **檢查是否上載了包** 看到與OSGi捆綁包中的Java代碼相關的問題。 開啟 [!UICONTROL Adobe Experience ManagerWeb控制台]: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) 搜索你的捆綁包。 確保捆綁包具有 **[!UICONTROL 活動]** 狀態。 有關對中的捆綁包進行故障排除的詳細資訊，請參閱下面 **[!UICONTROL 已安裝]** 狀態。

#### 檢查日誌

AEM是一個聊天平台，並在 **錯誤.log**。 的 **錯誤.log** 可找到已安AEM裝的位置：&lt; `aem-installation-folder>/crx-quickstart/logs/error.log`。

跟蹤問題的一種有用方法是在Java代碼中添加日誌語句：

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

預設情況下， **錯誤.log** 已配置為日誌 *[!DNL INFO]* 的下界。 如果要更改日誌級別，可以通過轉到 [!UICONTROL 日誌支援]: [http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog)。 您可能還會發現 **錯誤.log** 太無聊了。 您可以使用 [!UICONTROL 日誌支援] 為指定的Java包配置日誌語句。 這是項目的最佳做法，以便輕鬆將自定義代碼問題與OOTB平台問題AEM分開。

![登錄配AEM置](./assets/set-up-a-local-aem-development-environment/logging.png)

#### 捆綁包處於「已安裝」狀態 {#bundle-active}

所有束（不包括片段）應位於 **[!UICONTROL 活動]** 狀態。 如果在 [!UICONTROL 已安裝] 那就有一個問題需要解決。 多數情況下，這是依賴關係問題：

![捆綁錯誤AEM](assets/set-up-a-local-aem-development-environment/bundle-error.png)

在上面的螢幕截圖中， [!DNL WKND Core bundle] 是 [!UICONTROL 已安裝] 狀態。 這是因為捆綁需要的是 `com.adobe.cq.wcm.core.components.models` 例AEM行。

一個有用的工具是 [!UICONTROL 依賴關係查找器]: [http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder)。 添加Java包名稱以檢查實例上可用的版AEM本：

![核心元件](assets/set-up-a-local-aem-development-environment/core-components.png)

繼續上面的示例，我們可以看到實例上安裝的AEM版本是 **12.2** 與 **12.6** 這個捆綁包是預期的。 從那裡，你可以倒著看 [!DNL Maven] 依賴項AEM匹配 [!DNL Maven] 項目中的AEM依賴項。 在上例中 [!DNL Core Components] **v2.2.0** 安裝在實AEM例上，但代碼包是在依賴於 **v2.2.2**，因此是依賴關係問題的原因。

#### 驗證Sling模型註冊 {#osgi-component-sling-models}

AEM元件應始終由 [!DNL Sling Model] 封裝任何業務邏輯並確保HTL呈現指令碼保持乾淨。 如果遇到找不到Sling模型的問題，檢查 [!DNL Sling Models] 從控制台： [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)。 這將告訴您是否已註冊了Sling Model ，以及它與哪種資源類型（元件路徑）關聯。

![吊具模型狀態](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

顯示註冊 [!DNL Sling Model]。 `BylineImpl` 與元件資源類型關聯的 `wknd/components/content/byline`。

#### CSS或JavaScript問題

對於大多數CSS和JavaScript問題，使用瀏覽器的開發工具是最有效的故障排除方法。 要在針對作者實例進行開發時縮AEM小問題範圍，查看「已發佈」頁面會有所幫助。

![CSS或JS問題](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

開啟 [!UICONTROL 頁面屬性] 的 [!UICONTROL 查看為已發佈]。 此操作將在沒有編輯器AEM且查詢參數設定為的情況下開啟頁面 **wcmmode=已禁用**。 這將有效地禁AEM用創作UI，並使故障排除/調試前端問題變得容易得多。

開發前端代碼時遇到的另一個常見問題是CSS/JS已過時或正在載入。 作為第一步，請確保已清除瀏覽器歷史記錄，並在必要時啟動隱藏瀏覽器或新會話。

#### 調試客戶端庫

使用不同的類別和嵌入方法來包括多個客戶端庫，可能會很麻煩進行故障排除。 公AEM開了幾種工具來幫助處理。 最重要的工具之一是 [!UICONTROL 重建客戶端庫] 這將強AEM制重新編譯任何LESS檔案並生成CSS。

* [轉儲清單](http://localhost:4502/libs/granite/ui/content/dumplibs.html)  — 列出實例中註冊的所有客戶端AEM庫。 &lt;host>/libs/granite/ui/content/dumplibs.html
* [Test輸出](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html)  — 允許用戶查看客戶端lib的預期HTML輸出（基於類別）。 &lt;host>/libs/granite/ui/content/dumplibs.test.html
* [庫相關性驗證](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  — 突出顯示找不到的任何依賴項或嵌入類別。 &lt;host>/libs/granite/ui/content/dumplibs.validate.html
* [重建客戶端庫](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)  — 允許用戶強制重AEM建所有客戶端庫或使客戶端庫的快取無效。 此工具在使用LESS開發時特別有效，因為這AEM會強制重新編譯生成的CSS。 通常，與重建所有庫相比，使快取失效並執行頁面刷新更有效。 &lt;host>/libs/granite/ui/content/dumplibs.rebuild.html

![調試客戶端](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>如果您經常必須使用 [!UICONTROL 重建客戶端庫] 工具對所有客戶端庫進行一次重建可能是值得的。 這可能需要大約15分鐘，但通常可以消除將來的任何快取問題。
