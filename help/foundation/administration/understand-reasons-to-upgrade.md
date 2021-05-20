---
title: 了解升級的理由
description: 針對考慮升級至最新版Adobe Experience Manager的客戶，提供主要功能的高階劃分。
version: 6.5
sub-product: assets, cloud manager，商務，內容服務， dynamic media, forms, foundation，螢幕， sites
topics: best-practices, upgrade
audience: all
activity: understand
doc-type: article
topic: 升級
role: Leader, Architect, Developer, Administrator, Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '3541'
ht-degree: 3%

---


# 了解升級的理由

針對考慮升級至最新版Adobe Experience Manager的客戶，提供主要功能的高階劃分。

## 升級至AEM 6.5的主要功能

+ [Adobe Experience Manager 6.5發行說明](https://helpx.adobe.com/tw/experience-manager/6-5/release-notes.html)

### 基礎改良

Adobe Experience Manager 6.5持續透過以下途徑提升系統的穩定性、效能和可支援性：

+ **Java 11** 支援（同時維護Java 8支援）。

### 網站建立與管理

AEM Sites推出多項功能，旨在加速網站的建立和建置：

+ **SPA** Editor支援可讓SPA（單頁應用程式）在AEM中完整製作，支援豐富、適合行銷人員的製作體驗。+_ **JavaScript SDK的**、SPA專案開始套件和支援建置工具，讓前端開發人員可獨立於AEM開發與SPA Editor相容的單頁應用程式。
+ **核心** 元件提供多種新元件、元 **件** 程式庫，以及針對現有核心元件的多項增強功能。
+ 進一步的&#x200B;**翻譯**&#x200B;增強功能可簡化AEM Sites的翻譯。

### 流暢的體驗

AEM持續提供全新且改良的工具，協助您使用AEM以外的內容，充份運用Fluid Experiences。

+ **內容** 片段支援版本比較/差異和註解。
+ **AEM Assets HTTP** API支援將 **內** 容片段直接顯示為 **JSON**。
   **體驗** 片段 **支援全** 文Search和AEM **Dispatcher快取無** 法供參考 **頁面**。

### 資產管理

AEM Assets繼續以其豐富的資產管理功能為基礎，改善DAM的使用、管理和了解。 AEM 6.5持續改善Adobe Creative Cloud與創意工作流程之間的整合。

+ **Adobe資** 產連結會從Adobe Creative Cloud工具將創作直接連線至AEM Assets。
+ **Adobe** Stock整合可讓您直接從AEM Assets體驗直接存取Adobe Stock影像，創造順暢的內容探索體驗。
+ **AEM案** 頭版本2.0，在改善效能和穩定性的同時重新規劃自己。
+ **連線** 資產支援獨立的AEM Sites執行個體，以順暢地存取和使用不同AEM Assets執行個體的資產。
+ 更新&#x200B;**Dynamic Media**&#x200B;中的視訊支援，包括&#x200B;**360 Video**&#x200B;和&#x200B;**自訂視訊縮圖**。

### 內容智慧

AEM持續透過智慧技術建立整合，運用機器學習和人工智慧來改善所有體驗。

+ **Adobe資** 產連結 **新增了視覺相似度搜尋**，讓您在Adobe Creative Cloud工具中輕鬆探索及使用類似 **的影像**。

### Integrations

AEM能夠與其他Adobe服務整合：

+ **體驗** 片段可支援 **以JSON匯出至** Adobe Target，以及 **從** Adobe Target **刪除體驗片段式** 選件，深化與Adobe ****&#x200B;目標的整合。

### AMS Cloud Manager

[Cloud Manager](https://adobe.ly/2HODmsv)是Adobe Managed Services(AMS)客戶的專屬功能，提供下列功能：

+ Cloud Manager支援將AEM部署支援從AEM Sites擴展至&#x200B;**AEM Assets**，包括&#x200B;**資產處理的自動效能測試**。
+ **以預先** 定義的臨界值自動縮放AEM Publish層級，確保提供最佳的使用者體驗。
+ **非生產管道** 可讓開發團隊運用Cloud Manager持續檢查程式碼品質，並部署至較低的環境（開發和QA）。
+ **CI/CD管道API** 可讓客戶以程式設計方式與Cloud Manager互動，深化與內部部署開發基礎架構整合的可能性。

## 基礎功能

以下為AEM提供之主要基礎功能的矩陣。 其中部分功能是在舊版中推出，並在每個版本中新增增量增強功能。

+ [AEM Foundation發行說明](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***✔<sup>+</sup> 此版本中功能的重大增強功能。***

***✔<sup></sup>  SP表示此功能可通過Service Pack或Feature Pack獲得。***

<table>
    <thead>
        <tr>
            <td>基礎功能</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <strong>Java 11支援： </strong> AEM支援Java 11（以及Java 8）。
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Oak Content Repository</a>:</strong> 提供比Jackrabbit 2更出色的效能和可擴充性。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">oak-run.jar索引支援</a>:</strong> 改善Oak索引的重新/索引、統計資料收集和一致性檢查。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank">自訂搜尋索引</a>: </strong>
                添加自定義索引定義以優化查詢效能和搜索相關性的功能。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/tw/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">線上修訂清除</a>:</strong>
                執行存放庫維護，不發生伺服器停機。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">TarMK或MongoMK存放庫儲存</a>:</strong>
                <br> 使用簡單、效能優異的檔案式儲存TarMK（新一代TarPM版本）,
                <br> 或透過MongoDB支援的存放庫與MongoMK水準縮放的選項。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank">MongoMK效能和穩定性</a>:</strong>
            自AEM 6.0推出以來，MongoMK已持續增強。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">Amazon S3 DataStore</a>:</strong>
            運用可擴充的雲端儲存解決方案來儲存二進位資產。</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>觸控式UI功能比較：</strong>
                持續增強UI編寫功能，提高生產力，並與傳統UI功能比肩。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Omnisearch:</strong>
                快速搜尋並導覽AEM。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">操作控制面板</a>:</strong>
 從AEM內執行維護、監控伺服器運作狀況和分析效能。</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">升級改善</a>:</strong>
            升級改善可讓AEM更輕鬆、更快速的就地升級。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/tw/experience-manager/htl/using/overview.html" target="_blank">HTL範本語言</a>:</strong>
            將呈現與邏輯分開的現代範本引擎。大幅縮短元件開發時間。 隨每個版本新增的增量功能。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Sling模型</a>:</strong>
            將JCR資源建模為商業物件和邏輯的彈性架構。隨每個版本新增的增量功能。
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">Cloud Manager</a>: </strong>
                Cloud Manager專屬於Adobe Managed Services(AMS)客戶，透過最新的CI/CD管道加速開發和部署。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## 安全性功能

以下是AEM提供的主要安全性功能矩陣。 其中部分功能是在舊版中推出，並在每個版本中新增增量增強功能。

+ [安全性發行說明](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***✔表示此版本中功能的顯著增強。***

***✔<sup> + </sup> 表示可透過Service Pack或Feature Pack使用此功能。***

<table>
    <thead>
        <tr>
            <td>安全功能</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">服務用</a></strong>
            <br> 戶劃分權限可避免不必要地使用管理權限。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/tw/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">密鑰儲存管</a></strong>
            <br> 理全局信任儲存、證書和密鑰均在儲存庫中管理。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong></strong> <strong></strong></a>
            <br> CSRFprotection跨站請求偽造保護（現成）。</td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong></strong> <strong></strong></a>
            <br> CORSsupport跨原始資源共用支援可提供更大的應用程式彈性。</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://docs.adobe.com/docs/en/aem/6-5/administer/security/saml-2-0-authenticationhandler.html" target="_blank">改善SAML驗證支</a><br>
 </strong>援改善SAML重新導向、最佳化群組資訊，以及解決金鑰加密問題。 
            <br>
        </td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP作為OSGi配置簡</a><br>
 </strong>化LDAP驗證的管理和更新。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong>OSGi對純文字密碼的加密支援<br>
 </strong>密碼和其他敏感值可以以加密形式保存並自動解密。</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">CUG增</a><br>
 </strong>強功能已重新編寫封閉用戶組實施，以解決效能和可擴充性問題。</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔<sup>+</sup></td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">SSL精</a></strong>
            <br> 靈UI可簡化SSL的設定和管理。</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">封裝代號</a></strong>
            <br> 支援不再需要「黏著」工作階段來支援跨發佈例項的水準驗證。</td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">AdobeIMS驗證支</a><br>
 </strong>援專屬於Adobe Managed Services(AMS)，可透過AdobeIMS(Identity Management系統)集中管理對AEM製作執行個體的存取。</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
    </tr>
</tbody>
</table>

## Sites功能

以下為AEM提供之主要Sites功能的矩陣。 其中部分功能是在舊版中推出，並在每個版本中新增增量增強功能。

+ [AEM Sites發行說明](https://helpx.adobe.com/experience-manager/6-5/release-notes/sites.html)

***✔<sup>+</sup> 此版本中功能的重大增強功能。***

***✔<sup></sup>  SP表示此功能可通過Service Pack或Feature Pack獲得。***

<table>
    <thead>
        <tr>
            <td><strong>網站功能</strong></td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">觸控最佳化頁面製作</a>:</strong>
            可讓編輯人員運用具有觸控螢幕的平板電腦和電腦。</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">回應式網站製作</a>:</strong>
                版面模式可讓編輯器根據回應式網站的裝置寬度來調整元件大小。</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank">可編輯的範本</a>: </strong>
            允許專業作者建立和編輯頁面範本。</td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/core-components/user-guide.html" target="_blank">核心元件</a>:</strong>
            加速網站開發。可在GitHub上取得，以因應頻繁的發行排程和彈性。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">SPA編輯器</a>:</strong>
            使用建置在React或Angular上的單頁應用程式(SPA)架構，建立可授權的網頁體驗。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/tw/experience-manager/6-5/release-notes/style-system-fp.html" target="_blank">樣式系統</a>:</strong>
            使用內容樣式系統定義AEM元件的視覺外觀，以增加元件的重複使用。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">多網站管理員(MSM)</a>:</strong>
            管理多個共用相同內容的網站（即多語言、多個品牌）。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">內容翻譯</a>:</strong>
            即插即用框架與業界領先的第三方翻譯服務整合。</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html" target="_blank">ContextHub</a>:</strong>
            用於個人化內容的新一代用戶端內容架構。</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/launches.html" target="_blank">啟動</a>:</strong>
            開發適用於未來版本的內容，而不會中斷日常編寫。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-understand.html" target="_blank">內容片段</a>:</strong>
            建立並組織與簡報中分離的編輯內容，以方便重複使用。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html" target="_blank">體驗片段</a>:</strong>
            建立可重複使用的體驗和變體，並針對案頭、行動裝置和社交管道最佳化。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/release-notes/content-services-fragments-featurepack.html" target="_blank">內容服務</a>:</strong>
            從AEM將內容匯出為JSON，以便在不同裝置和應用程式間耗用。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Adobe Analytics整合與內容深入分析：</strong>
                輕鬆整合Adobe Analytics與DTM。在製作環境中顯示效能資訊。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank">Adobe Target整合</a>:</strong>
            建立目標體驗、建立可重複使用的選件資料庫的逐步精靈。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Adobe Campaign整合</a>:</strong>
            輕鬆與新一代電子郵件行銷活動解決方案整合。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank">AdobeLaunch整合</a>:</strong>
            整合Adobe的新一代標籤管理雲端服務。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-screens-introduction.html" target="_blank">螢幕</a>:</strong>
            管理數位看板和資訊站的體驗。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank">電子商務</a>:</strong>
            透過網路、行動裝置及社交接觸點，提供品牌化的個人化購物體驗。
            </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html" target="_blank">社群</a>:</strong>
            論壇、串連式留言、事件日曆和許多其他功能可讓您與網站訪客進行深入互動。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Assets功能

以下為AEM提供之主要Assets功能的矩陣。 其中部分功能是在舊版中推出，並在每個版本中新增增量增強功能。

+ [AEM Assets發行說明](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html)

***✔表示此版本中功能的顯著增強。***

***✔<sup> + </sup> 表示可透過Service Pack或Feature Pack使用此功能。***

<table>
    <thead>
        <tr>
            <td>資產功能</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">觸控最佳化UI</a>:</strong>
            在桌上型電腦或觸控式裝置上管理資產。</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/metadata.html" target="_blank">進階中繼資料管理</a>:</strong>
            中繼資料範本、中繼資料結構編輯器和大量中繼資料編輯。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank"></a> 任務與 <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank"></a> 工作流程管理：</strong>
            運用AEM專案，預先建立審核及核准數位資產的工作流程與任務。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>可擴充性與效能：</strong>
            大規模擷取、上傳和儲存的增強支援。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">Assets HTTP API</a>:</strong>
            透過HTTP和JSON以程式設計方式與資產互動。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/link-sharing.html" target="_blank">連結共用</a>:</strong>
            簡單的數位資產臨機共用，不需登入。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html" target="_blank">Brand Portal</a>:</strong>
            雲端服務SAAS解決方案，可順暢地共用及發佈數位資產。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">連線資產</a>:</strong>
            AEM Sites執行個體可順暢地存取及使用不同AEM Assets執行個體的資產。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">資產深入分析</a>:</strong>
            運用Adobe Analytics擷取數位資產與AEM中檢視的客戶互動。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">多語言資產</a>:</strong>
            使用語言根自動支援資產中繼資料的翻譯。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank">智慧標籤與協調</a>:</strong>
            運用Adobe Sensei，以有用的中繼資料自動標籤影像。</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">智慧翻譯搜尋</a>:</strong>
            搜尋AEM Assets時自動翻譯搜尋詞。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html" target="_blank">Adobe InDesign Server整合</a>: </strong>
            產生產品目錄。根據InDesign範本製作手冊、傳單和印刷廣告。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/tw/experience-manager/desktop-app/aem-desktop-app.html" target="_blank">AEM案頭應用程式</a>:</strong>
            將資產同步至本機案頭，以便與Creative Suite產品編輯。
            </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Adobe影像處理程式庫</a>:</strong>
                <br> 用於高品質檔案操控的Photoshop和Acrobat PDF程式庫。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/tw/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Adobe資產連結</a>:</strong>
            直接從Adobe建立雲端應用程式存取AEM Assets。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Adobe Stock整合</a>:</strong>
            直接從AEM順暢地存取和使用Adobe Stock影像。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

### AEM AssetsDynamic Media

***✔<sup>+</sup> 此版本中功能的重大增強功能。***

***✔<sup></sup>  SP表示此功能可通過Service Pack或Feature Pack獲得。***


<table>
    <thead>
        <tr>
            <td>Dynamic Media功能</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3 + FP</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets.html" target="_blank">影像</a>:</strong>
            以不同大小和格式（包括智慧型裁切）動態傳送影像。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">視訊</a>:</strong>
            進階視訊編碼和最適化視訊串流</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/interactive-images.html" target="_blank">互動式媒體</a>: </strong>
            建立互動式橫幅、影片及可點按的內容，以展示重要選件。
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>集(<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">影像</a>、 <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">回轉</a>、 <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">混合媒體</a>):</strong>
            允許使用者縮放、平移、旋轉並模擬360度的檢視體驗。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://docs.adobe.com/docs/zh-Hant/aem/6-5/administer/content/dynamic-media/viewer-presets.html" target="_blank">檢視器</a>:</strong>
            自訂品牌的多媒體播放器和預設集，可支援不同的螢幕/裝置。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">傳送</a>:</strong>
            彈性的選項，可連結或內嵌Dynamic Media內容，以及透過HTTP/2通訊協定傳送。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>從Scene7升級至Dynamic Media:</strong>
            移轉主資產並繼續使用現有S7 URL的功能。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

## Forms功能

以下為AEM提供之主要AEM Forms附加元件功能的矩陣。 其中部分功能是在舊版中推出，並在每個版本中新增增量增強功能。

+ [AEM Forms發行說明](https://helpx.adobe.com/experience-manager/6-5/release-notes/forms.html)

***✔<sup>+</sup> 此版本中功能的重大增強功能。***

***✔<sup></sup>  SP表示此功能可通過Service Pack或Feature Pack獲得。***

<table>
    <thead>
        <tr>
            <td>Forms功能</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">適用性Forms編輯器</a>:</strong>
            根據裝置和瀏覽器設定，建立吸引人、回應式和最適化的表單。</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">記錄檔案</a>:</strong>
            建立檔案以確保資料擷取體驗或列印就緒版本的長期儲存。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/themes.html" target="_blank">主題編輯器</a>:</strong>
            建立可重複使用的主題，以設定表單的元件和面板的樣式。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/template-editor.html" target="_blank">範本編輯器</a>:</strong>
            標準化並實作最適化表單的最佳實務。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Adobe Sign整合</a>:</strong>
            允許部署Adobe Sign整合式表單的簽署案例。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/cm-overview.html" target="_blank">通信管理</a>:</strong>
            透過AEM Forms，您可以建立、管理及提供個人化和互動式的客戶通信。
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">第三方資料整合</a>:</strong>
            使用資料整合，系統會根據使用者在表單中的輸入，從不同的資料來源擷取資料。提交表單時，擷取的資料會回寫至資料來源。
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Forms處理的工作流程（在OSGi上）</a>:</strong>
            簡化表單核准程式的部署。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/user-guide.html?topic=/experience-manager/6-5/forms/morehelp/integrations.ug.js" target="_blank">與Marketing Cloud整合</a>:</strong>
            與Adobe Analytics和Adobe Target整合，以增強和衡量客戶體驗。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">表單管理器</a>:</strong>
            管理所有表單/檔案/通信的單一位置，例如啟用分析、翻譯、A/B測試、審核和發佈。
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank">AEM Forms應用程式</a>: </strong>
            允許在iOS、Android或Windows上的應用程式內處理線上/離線表單。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/adaptive-document.html" target="_blank">互動式通訊</a>:</strong>
            使用互動式元素（例如圖表）建立豐富通訊（例如目標陳述式）（先前稱為最適化檔案）。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/pdf/aem-forms/6-5/WorkbenchHelp.pdf" target="_blank">適用於Forms處理的工作流程(J2EE)</a>:</strong>
            運用直覺式IDE，建立複雜的表單/檔案導向工作流程。</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">AEM Forms檔案安全性</a>:</strong>
            安全存取及授權PDF和Office檔案。
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">測試架構</a>:</strong>
            使用Calvin架構和Chrome外掛程式來支援和除錯最適化表單。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

## 社群功能

以下為AEM提供之主要AEM Communities附加元件功能的矩陣。 其中部分功能是在舊版中推出，並在每個版本中新增增量增強功能。

+ [AEM Communities新功能摘要](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html#main-pars_text)

***✔<sup>+</sup> 此版本中功能的重大增強功能。***

***✔<sup></sup>  SP表示此功能可通過Service Pack或Feature Pack獲得。***

<table>
    <thead>
        <tr>
            <td> </td>
            <td>社群功能</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="7">Communities函式</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/forum.html" target="_blank">論壇</a>:</strong> （社交元件架構）建立新主題，或檢視、關注、搜尋和移動現有主題。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <p><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-qna.html" target="_blank">QnA</a>:</strong>
                詢問、檢視和回答問題。</p>
            </td>
            <td></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/blog-feature.html" target="_blank">部落格</a>:</strong>
                在發佈端建立部落格文章和留言。
            </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/ideation-feature.html" target="_blank">構想</a>: </strong>
                建立與與社群分享意見，或檢視、關注和評論現有的意見。
            </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/calendar.html" target="_blank">日曆</a>:</strong>
                （社交元件架構）提供社群事件資訊給網站訪客。
            </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/file-library.html" target="_blank">檔案庫</a>:</strong>
                在社群網站內上傳、管理和下載檔案。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/users.html#AboutCommunityGroups" target="_blank">使用者群組</a>:
            </strong>一組用戶可以屬於成員組，並可以集體分配角色。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong> </strong></td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">指派</a>:</strong>
            建立學習資源並指派給社群成員。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">啟用</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/catalog.html" target="_blank"></a> 目錄和 <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">資源管理</a>:</strong>
            從目錄存取啟用資源。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#CreateaLearningPath" target="_blank">學習路徑管理</a>:</strong>
            管理課程或啟用資源群組。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reports.html#main-pars_text_1739724213" target="_blank">啟用報表</a>:</strong>
            報告啟用資源和學習路徑。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#main-pars_text_899882038" target="_blank">啟用參與</a>: </strong>
            新增啟用資源的註解。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">啟用分析</a>:</strong>
            視訊分析、進度報告和指派報告</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="8">公域</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/comments.html" target="_blank"></a> 評論和附件：</strong>
            （社交元件架構）身為社群成員，分享關於社群網站內容的意見和知識。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>內容片段轉換：</strong>
            將UGC貢獻轉換為內容片段。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reviews.html" target="_blank">審閱</a>:</strong>
                （社交元件架構）身為社群成員，使用註解和評等功能的組合來審核內容片段。</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/rating.html" target="_blank">評等</a>:/strong&gt;（社交元件架構）身為社群成員，會對內容評分。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/voting.html" target="_blank">投票</a>:</strong>
                （社交元件架構）身為社群成員，對內容進行上選或下選。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tag-ugc.html" target="_blank">標籤</a>:</strong>
            附加含有內容的標籤（關鍵字或標籤）以快速找到內容。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/search.html" target="_blank">搜尋</a>:</strong>
            預測性和建議性搜尋。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/translate-ugc.html" target="_blank">翻譯</a>: </strong>
            使用者產生內容的機器翻譯。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="10">管理</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/create-site.html" target="_blank">網站管理</a>:</strong>
            使用社群功能建立網站。</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">範本</a>:</strong>
                <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank"></a> 網站和 <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tools-groups.html" target="_blank"></a> 群組範本，供精靈建立功能完整的社群網站。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>可編輯的範本： </strong>
            讓社群管理員能使用AEM可編輯的範本來建置豐富的體驗。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/creating-groups.html" target="_blank">群組或子社群</a>:</strong>
            在社群網站內動態建立子社群。
            </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/in-context.html" target="_blank">調節</a>:</strong>
            調節使用者產生的內容。
            </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderation.html" target="_blank">大量協調</a>:</strong>
            協調控制台，以大量管理使用者產生的內容。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderate-ugc.html#CommonModerationConcepts" target="_blank">垃圾訊息偵測和設定檔篩選</a>:</strong>
            自動垃圾訊息偵測。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/members.html" target="_blank">成員管理</a>:</strong>
            從成員管理區域管理用戶配置檔案和組。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html#main-pars_text_866731966" target="_blank">回應式設計</a>:</strong>
            AEM Communities網站回應式。
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Analytics</a>:</strong>
            整合Adobe Analytics，以獲得Communities網站使用情況的重要深入分析。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="4">成員</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/advanced.html" target="_blank">計分和徽章</a>:</strong>
            (由Adobe Sensei提供進階計分功能)識別社群成員為專家並給予獎勵。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/activities.html" target="_blank"></a> 活動 <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/notifications.html" target="_blank">與通知</a>:</strong>
            檢視最近的活動資料流，並收到有關感興趣事件的通知。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/configure-messaging.html" target="_blank">訊息</a>:</strong>
            將訊息導向至使用者和群組。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/social-login.html" target="_blank">社交登入</a>:</strong>
            使用其Facebook或Twitter帳戶登入。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">平台</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">MSRP（Mongo儲存）</a>:</strong>
            使用者產生的內容(UGC)會直接保存在本機MongoDB例項中</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">DSRP（資料庫儲存）</a>:</strong>
            使用者產生的內容(UGC)會直接保存在本機MySQL資料庫例項中。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">SRP（雲端儲存）</a>:</strong>
                使用者產生的內容(UGC)會遠端保存在由Adobe托管和管理的雲端服務中。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank"><strong>JSRP</a>:</strong>
                社群內容儲存在JCR中，而UGC可從發佈到的製作（或發佈）例項存取。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sync.html" target="_blank">使用者和群組同步</a>:</strong>
            使用發佈伺服器陣列拓撲時，可跨發佈執行個體同步使用者和群組。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

AEM Communities透過發行版本新增[增強功能](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html)，讓組織可參與並啟用其使用者，方法如下：

+ **@** mentionsupport（用戶生成的內容）。
+ 改善&#x200B;**Enablement**&#x200B;元件中的&#x200B;**鍵盤導覽**&#x200B;的協助工具。
+ 使用&#x200B;**自訂篩選器**&#x200B;改善&#x200B;**大量協調**。
+ **可編** 輯的範本，讓社群管理員能在AEM中建立豐富的社群體驗。
+ 使用者現在可以大量傳送&#x200B;**直接訊息給群組的所有成員。**
