---
title: 瞭解升級的原因
description: 為考慮升級至最新版Adobe Experience Manager 6的客戶提供主要功能的高層級劃分。
version: 6.5
topic: Upgrade
feature: Release Information
role: Leader, Architect, Developer, Admin, User
level: Beginner
doc-type: Article
exl-id: bf4030b0-67c4-4b00-af95-f63e6f79e995
duration: 538
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '2588'
ht-degree: 1%

---

# 瞭解升級的原因

為考慮升級至最新版Adobe Experience Manager 6的客戶提供主要功能的高層級劃分。

## 升級至AEM 6.5的重要功能

+ [Adobe Experience Manager 6.5發行說明](https://helpx.adobe.com/tw/experience-manager/6-5/release-notes.html)

### 基礎改良

Adobe Experience Manager 6.5透過下列方式，持續增強系統的穩定性、效能和支援能力：

+ **Java 11**&#x200B;支援（同時維持Java 8支援）。

### 網站建立與管理

AEM Sites引進了多項功能，旨在加速網站的建立和建置：

+ **SPA Editor**支援可讓SPA （單頁應用程式）在AEM中完全撰寫，支援豐富、適合行銷人員的撰寫體驗。
+_**JavaScript SDK** (SPA專案入門套件和支援的組建工具)可讓前端開發人員獨立開發SPA Editor相容的單頁應用程式，而不受AEM影響。
+ **核心元件**&#x200B;新增了大量新元件、**元件庫**&#x200B;以及現有核心元件的各種增強功能。
+ 進一步&#x200B;**翻譯**&#x200B;增強功能可簡化AEM Sites的翻譯。

### 流暢的使用體驗

AEM持續採用流動體驗，包括全新及改良的工具，協助您使用AEM以外的內容。

+ **內容片段**&#x200B;支援版本比較/差異和註解。
+ **AEM的Assets HTTP API**&#x200B;支援在DAM中以&#x200B;**JSON**&#x200B;形式直接公開&#x200B;**內容片段**。
  **體驗片段**&#x200B;支援&#x200B;**全文檢索搜尋**&#x200B;和&#x200B;**AEM Dispatcher快取失效**，以參照&#x200B;**頁面**。

### 資產管理

AEM Assets繼續以其豐富的資產管理功能集為基礎，以改進對DAM的使用、管理和瞭解。 AEM 6.5持續改善Adobe Creative Cloud與創意工作流程之間的整合。

+ **Adobe Asset Link**&#x200B;會從Adobe Creative Cloud工具將創意內容直接連線至AEM Assets。
+ **Adobe Stock**&#x200B;整合可讓您直接從AEM Assets體驗存取Adobe Stock影像，創造順暢的內容探索體驗。
+ **AEM Desktop App**&#x200B;發行版本2.0，在改善效能和穩定性的同時重新構想自身。
+ **連線的Assets**&#x200B;支援獨立的AEM Sites執行個體，可順暢存取及使用其他AEM Assets執行個體的資產。
+ 更新&#x200B;**Dynamic Media**&#x200B;中的視訊支援，包括&#x200B;**360視訊**&#x200B;和&#x200B;**自訂視訊縮圖**。

### 內容智慧

AEM持續建立與智慧技術的整合，運用機器學習和人工智慧來改善所有體驗。

+ **Adobe資產連結**&#x200B;新增&#x200B;**視覺相似度搜尋**，以便在&#x200B;**Adobe Creative Cloud工具**&#x200B;中輕鬆探索和使用類似的影像。

### 整合

AEM加強與其他Adobe服務整合的能力：

+ **體驗片段**&#x200B;透過支援&#x200B;**匯出為JSON**&#x200B;至Adobe Target以及從&#x200B;**Adobe Target**&#x200B;中&#x200B;**刪除體驗片段型選件**&#x200B;的功能，深化了與&#x200B;**Adobe Target**&#x200B;的整合。

### AMS CLOUD MANAGER

[Cloud Manager](https://adobe.ly/2HODmsv)是AdobeManaged Services (AMS)客戶的專屬產品，可提供下列功能：

+ Cloud Manager支援將AEM部署支援從AEM Sites延伸至&#x200B;**AEM Assets**，包括&#x200B;**資產處理的自動化效能測試**。
+ 以預先定義的臨界值自動縮放AEM Publish層級的&#x200B;**個**，確保最佳的一般使用者體驗。
+ **非生產管道**&#x200B;可讓開發團隊運用Cloud Manager來持續檢查程式碼品質並部署至較低環境（開發和QA）。
+ **CI/CD Pipeline API**&#x200B;可讓客戶以程式設計方式與Cloud Manager互動，深化與內部部署開發基礎結構的整合可能性。

## 基礎功能

以下為AEM所提供之主要基礎功能的對照表。 其中部分功能已在每個版本中新增的早期版本增量增強功能中引入。

+ [AEM Foundation發行說明](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***✔<sup>+</sup>此版本中功能的重要增強功能。***

***✔<sup>SP</sup>表示此功能可透過Service Pack或Feature Pack取得。***

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
                <strong>Java 11支援：</strong> AEM支援Java 11 （以及Java 8）。
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
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Oak內容存放庫</a>：</strong>提供比前置任務Jackrabbit 2更優異的效能與擴充性。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">oak-run.jar索引支援</a>：</strong>已改善Oak索引的重新索引/索引、統計資料收集和一致性檢查。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank">自訂搜尋索引</a>： </strong>
                可新增自訂索引定義，以最佳化查詢效能和搜尋關聯性。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">線上修訂清除</a>：</strong>
                執行存放庫維護，避免伺服器停機時間。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">TarMK或MongoMK存放庫儲存空間</a>：</strong>
                <br>使用TarMK （新一代的TarPM版本）簡單、高效能檔案式儲存的選項
                使用MongoDB支援的儲存庫和MongoMK水平縮放<br>。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank">MongoMK效能和穩定性</a>：</strong>
            自MongoMK推出AEM 6.0以來，已持續增強功能。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">Amazon S3資料存放區</a>：</strong>
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
                持續增強編寫UI以提高速度，並透過傳統UI提高生產力和功能同位性。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Omnisearch：</strong>
                快速搜尋和導覽AEM。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">操作儀表板</a>：</strong>
 從AEM內執行維護、監控伺服器健康狀態以及分析效能。</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">升級改善</a>：</strong>
            升級改良功能可讓您更輕鬆、更快速地就地升級AEM。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/tw/experience-manager/htl/using/overview.html" target="_blank">HTL範本語言</a>：</strong>
            將表示與邏輯分隔的現代範本引擎。 大幅縮短元件開發時間。 每個版本都新增了增量功能。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Sling模型</a>：</strong>
            將JCR資源建模為商業物件和邏輯的彈性架構。 每個版本都新增了增量功能。
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
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">Cloud Manager</a>： </strong>
                專屬於AdobeManaged Services (AMS)客戶，Cloud Manager透過最先進的CI/CD管道加速開發和部署。</td>
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

以下為AEM提供的主要安全性功能矩陣。 其中部分功能已在每個版本中新增的早期版本增量增強功能中引入。

+ [安全性發行說明](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***✔表示此版本中的功能有重大增強功能。***

***✔<sup>+</sup>表示此功能可透過Service Pack或Feature Pack取得。***

<table>
    <thead>
        <tr>
            <td>安全性功能</td>
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
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">服務使用者</a></strong>
            <br>劃分許可權，避免不必要地使用管理員許可權。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">金鑰庫管理</a></strong>
            <br>全域信任存放區、憑證和金鑰都在存放庫中管理。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong>CSRF</strong> <strong>保護</strong></a>
            <br>立即可用的跨網站要求偽造保護。</td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong>CORS</strong> <strong>支援</strong></a>
            <br>跨原始資源共用支援，提供更大的應用程式彈性。</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://experienceleague.adobe.com/docs/" target="_blank">已改善SAML驗證支援</a><br>
 </strong>已改善SAML重新導向、最佳化的群組資訊，以及已解決的金鑰加密問題。
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
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP作為OSGi設定</a><br>
 </strong>簡化LDAP驗證的管理和更新。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong>純文字密碼的OSGi加密支援<br>
 </strong>密碼和其他敏感值可以加密形式儲存，並自動解密。</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">CUG增強功能</a><br>
 </strong>已重新寫入封閉式使用者群組實作，以解決效能和擴充性問題。</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔<sup>+</sup></td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">SSL精靈</a></strong>
            <br> UI以簡化SSL的設定和管理。</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">封裝的Token支援</a></strong>
            <br>不再需要「粘性」工作階段來支援跨發佈執行個體的水準驗證。</td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Adobe IMS驗證支援</a><br>
 </strong>AdobeManaged Services (AMS)專用，透過Adobe IMS (Identity Management System)集中管理對AEM Author執行個體的存取。</td>
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

以下為AEM提供之主要Sites功能的矩陣。 其中部分功能已在每個版本中新增的早期版本增量增強功能中引入。

+ [AEM Sites發行說明](https://helpx.adobe.com/experience-manager/6-5/release-notes/sites.html)

***✔<sup>+</sup>此版本中功能的重要增強功能。***

***✔<sup>SP</sup>表示此功能可透過Service Pack或Feature Pack取得。***

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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">觸控最佳化頁面製作</a>：</strong>
            可讓編輯利用平板電腦和配備觸控熒幕的電腦。</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">回應式網站編寫</a>：</strong>
                版面模式可讓編輯器根據回應式網站的裝置寬度調整元件大小。</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank">可編輯的範本</a>：</strong>
            允許專業作者建立及編輯頁面範本。</td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/core-components/user-guide.html" target="_blank">核心元件</a>：</strong>
            加速網站開發。 GitHub提供頻繁發行排程和彈性。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">SPA編輯器</a>：</strong>
            使用以React為基礎的單頁應用程式(SPA)架構，建立可授權的Web體驗。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>樣式系統：</strong>
            透過使用內容樣式系統定義其視覺外觀，提高AEM元件重複使用率。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">多站台管理員(MSM)</a>：</strong>
            管理共用相同內容的多個網站（即多語言、多個品牌）。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">內容翻譯</a>：</strong>
            隨插即用架構整合了業界領先的第三方翻譯服務。</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html" target="_blank">ContextHub</a>：</strong>
            用於個人化內容的新一代使用者端內容架構。</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/launches.html" target="_blank">啟動</a>：</strong>
            在不中斷日常製作的前提下，為未來版本開發內容。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>內容片段：</strong>
            建立和組織從簡報中分離的編輯內容，以方便重複使用。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html" target="_blank">體驗片段</a>：</strong>
            建立針對案頭、行動裝置和社交頻道最佳化的可重複使用體驗和變數。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>內容服務：</strong>
            從AEM將內容匯出為JSON，以跨裝置和應用程式使用。</td>
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
                輕鬆整合Adobe Analytics和DTM。 在作者環境中顯示效能資訊。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank">Adobe Target整合</a>：</strong>
            逐步精靈以建立鎖定目標的體驗，並建立可重複使用的選件程式庫。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Adobe Campaign整合</a>：</strong>
            與新一代電子郵件行銷活動解決方案輕鬆整合。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>Adobe Experience Platform整合中的<strong><a href="https://helpx.adobe.com/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank">標籤</a>：</strong>
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
            <td><strong>Screens：</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank">電子商務</a>：</strong>
            在網頁、行動裝置和社交接觸點之間，提供品牌和個人化的購物體驗。
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html" target="_blank">社群</a>：</strong>
            論壇、執行緒評論、活動行事曆和許多其他功能允許與網站訪客進行深入互動。</td>
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

以下為AEM所提供之主要Assets功能的對照表。 其中部分功能已在每個版本中新增的早期版本增量增強功能中引入。

+ [AEM Assets發行說明](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html)

***✔表示此版本中的功能有重大增強功能。***

***✔<sup>+</sup>表示此功能可透過Service Pack或Feature Pack取得。***

<table>
    <thead>
        <tr>
            <td>Assets功能</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">觸控最佳化UI</a>：</strong>
            在桌上型電腦或觸控裝置上管理資產。</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/metadata.html" target="_blank">進階中繼資料管理</a>：</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank">任務</a>和工作流程管理：</strong>
            運用AEM專案預先建立的工作流程與工作，用於數位資產的稽核與核准。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>擴充性與效能：</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">Assets HTTP API</a>：</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/link-sharing.html" target="_blank">連結共用</a>：</strong>
            數位資產的簡單臨機共用，無需登入。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html" target="_blank">Brand Portal</a>：</strong>
            雲端服務SAAS解決方案，提供數位資產順暢的共用和分送。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">已連線的Assets</a>：</strong>
            AEM Sites例項可順暢地存取及使用其他AEM Assets例項的資產。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">資產分析</a>：</strong>
            運用Adobe Analytics擷取客戶與數位資產的互動，並在AEM中檢視。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">多語言Assets</a>：</strong>
            使用語言根自動翻譯支援資產中繼資料。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank">智慧標籤與管制</a>：</strong>
            運用Adobe Sensei，使用有用的中繼資料自動標籤影像。</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">智慧型翻譯搜尋</a>：</strong>
            搜尋AEM Assets時自動翻譯搜尋字詞。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html" target="_blank">Adobe InDesign Server整合</a>：</strong>
            產生產品目錄。 根據InDesign範本製作手冊、傳單和印刷廣告。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/tw/experience-manager/desktop-app/aem-desktop-app.html" target="_blank">AEM案頭應用程式</a>：</strong>
            將資產同步至本機案頭，以便使用Creative Suite產品進行編輯。
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Adobe影像程式庫</a>：</strong>
                <br>用於高品質檔案操作的Photoshop和AcrobatPDF資料庫。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/tw/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Adobe資產連結</a>：</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Adobe Stock整合</a>：</strong>
            直接從AEM順暢存取及使用Adobe Stock影像。</td>
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

### AEM Assets Dynamic Media

***✔<sup>+</sup>此版本中功能的重要增強功能。***

***✔<sup>SP</sup>表示此功能可透過Service Pack或Feature Pack取得。***


<table>
    <thead>
        <tr>
            <td>Dynamic Media功能</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3 +FP</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets.html" target="_blank">影像處理</a>：</strong>
            以動態方式傳送不同大小和格式的影像，包括智慧型裁切。</td>
            <td> </td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">影片</a>：</strong>
            進階視訊編碼和自我調整視訊串流</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/interactive-images.html" target="_blank">互動媒體</a>：</strong>
            建立互動式橫幅、包含可點選內容的影片，以展示重要選件。
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
            <td><strong>集合（<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">影像</a>，<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">迴轉</a>，<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">混合媒體</a>）：</strong>
            允許使用者縮放、平移、旋轉和模擬360度的檢視體驗。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/" target="_blank">檢視者</a>：</strong>
            自訂品牌多媒體播放器和預設集，並支援不同的熒幕/裝置。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">傳遞</a>：</strong>
            透過HTTP/2通訊協定連結或內嵌Dynamic Media內容及傳遞的彈性選項。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>從Scene7升級至Dynamic Media：</strong>
            可移轉主要資產並繼續使用現有S7 URL。</td>
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

以下為AEM所提供之主要AEM Forms附加功能的對照表。 其中部分功能已在每個版本中新增的早期版本增量增強功能中引入。

***✔<sup>+</sup>此版本中功能的重要增強功能。***

***✔<sup>SP</sup>表示此功能可透過Service Pack或Feature Pack取得。***

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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">最適化Forms編輯器</a>：</strong>
            根據裝置和瀏覽器設定，建立吸引人、回應式且最適化表單。</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">記錄檔案</a>：</strong>
            建立檔案以確保資料擷取體驗或可列印版本的長期儲存。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/themes.html" target="_blank">主題編輯器</a>：</strong>
            建立可重複使用的佈景主題來設定元件和表單面板的樣式。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/template-editor.html" target="_blank">範本編輯器</a>：</strong>
            標準化及實作最適化表單的最佳作法。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Acrobat Sign整合</a>：</strong>
            允許根據簽署情境部署Acrobat Sign整合式表單。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/cm-overview.html" target="_blank">通訊管理</a>：</strong>
            透過AEM Forms，您可以建立、管理和提供個人化和互動式客戶對應。
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">協力廠商資料整合</a>：</strong>
            透過資料整合，系統會根據使用者在表單中的輸入，從不同的資料來源擷取資料。 在提交表單時，擷取的資料會回寫至資料來源。
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
            <td>用於Forms處理的<strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">工作流程（在OSGi上）</a>：</strong>
            簡化表單核准流程的部署。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/user-guide.html?topic=/experience-manager/6-5/forms/morehelp/integrations.ug.js" target="_blank">與Marketing Cloud</a>整合：</strong>
            與Adobe Analytics和Adobe Target整合，以強化和測量客戶體驗。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">表單管理員</a>：</strong>
            管理所有表單/檔案/信件的單一位置，例如啟用分析、翻譯、A/B測試、稽核和發佈。
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank">AEM Forms應用程式</a>：</strong>
            允許在iOS、Android或Windows上的應用程式內進行線上/離線表單處理。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/adaptive-document.html" target="_blank">互動式通訊</a>：</strong>
            使用互動式元素（例如圖表，以前稱為Adaptive Documents）建立豐富的通訊（例如目標陳述式）。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td>適用於Forms處理的<strong>工作流程(J2EE)：</strong>
            使用直覺式IDE建置複雜的表單/以檔案為中心的工作流程。</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">AEM Forms Document Security</a>：</strong>
            安全地存取和授權PDF和Office檔案。
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">正在測試架構</a>：</strong>
            使用Calvin框架和Chrome外掛程式來支援及偵錯調適型表單。</td>
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

