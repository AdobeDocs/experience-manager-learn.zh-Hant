---
title: 瞭解升級原因
description: 針對考慮升級到最新版Adobe Experience Manager6的客戶，對關鍵功能進行了高級細分。
version: 6.5
topic: Upgrade
role: Leader, Architect, Developer, Admin, User
level: Beginner
exl-id: bf4030b0-67c4-4b00-af95-f63e6f79e995
source-git-commit: 81b96f59450448a3d5b17a61aa025acd60d0cce1
workflow-type: tm+mt
source-wordcount: '3460'
ht-degree: 3%

---

# 瞭解升級原因

針對考慮升級到最新版Adobe Experience Manager6的客戶，對關鍵功能進行了高級細分。

## 升級到6.AEM5的關鍵功能

+ [Adobe Experience Manager6.5發行說明](https://helpx.adobe.com/tw/experience-manager/6-5/release-notes.html)

### 基礎改進

Adobe Experience Manager6.5通過以下方式繼續提高系統的穩定性、效能和可支援性：

+ **爪哇11** 支援（同時維護Java 8支援）。

### 網站建立和管理

AEM Sites推出了一些旨在加快網站建立和構建的功能：

+ **編SPA輯器** 支援SPA允許（單頁應用程式）在中完全編寫AEM，支援豐富、對Marketer友好的創作體驗。
+_ **JavaScript SDK的**、項SPA目啟動工具包和支援的生成工具，允許前端開發人員開SPA發與編輯器相容的單頁應用程式，而不AEM受影響。
+ **核心元件** 增加了許多新元件， **元件庫** 以及對現有核心元件的各種增強。
+ 進一步 **翻譯** 增強了對AEM Sites的翻譯。

### 流體體驗

繼AEM續採用新的和改進的工具，以方便在外部使用內容，從而獲得Fluid ExperienceAEM。

+ **內容片段** 支援版本比較/比較和注釋。
+ **AEM資產HTTP API** 支援 **內容片段** 直接在DAM中 **JSON**。
   **體驗片段** 支援 **全文搜索** 和 **Dispatcher緩AEM存無效** 引用 **頁面**。

### 資產管理

AEM Assets繼續利用其豐富的資產管理能力來改進對DAM的使用、管理和理解。 AEM6.5繼續改進Adobe Creative Cloud與創造性工作流的融合。

+ **Adobe資產連結** 將創意產品直接與AEM Assets聯繫在一起。
+ **Adobe Stock** 整合可以直接從AEM Assets體驗中訪問Adobe Stock影像，創造無縫的內容發現體驗。
+ **桌AEM面應用** 版本2.0並重新設計自己，同時提高效能和穩定性。
+ **已連接資產** 支援離散的AEM Sites實例，以無縫訪問和使用來自不同AEM Assets實例的資產。
+ 更新的視頻支援 **Dynamic Media**，包括 **360視頻** 和 **自定義視頻縮略圖**。

### 內容智慧

繼AEM續利用機器學習和人工智慧改善所有體驗，與智慧技術融合。

+ **Adobe資產連結** 添加 **視覺相似性搜索**，允許在中輕鬆發現和使用類似的影像 **Adobe Creative Cloud工具**。

### 整合

增AEM強了與其他Adobe服務整合的能力：

+ **體驗片段** 深化了與 **Adobe Target** 通過支援 **導出為JSON** Adobe Target和 **刪除基於體驗片段的服務** 從 **Adobe Target**。

### AMS雲管理器

[雲管理器](https://adobe.ly/2HODmsv)是Adobe Managed Services(AMS)客戶獨有的，它提供以下功能：

+ 雲管理器支援將部AEM署支援從AEM Sites擴展到 **AEM Assets**，包括 **資產處理的自動效能測試**。
+ **自動縮放** 按預定義的閾值設定AEM發佈層，確保最佳最終用戶體驗。
+ **非生產管道** 允許開發團隊利用Cloud Manager不斷檢查代碼質量，並部署到較低的環境（開發和QA）。
+ **CI/CD管道API** 允許客戶以寫程式方式與Cloud Manager接觸，從而深化與內部開發基礎架構的整合可能性。

## 基礎功能

以下是提供的關鍵基礎功能的矩AEM陣。 其中一些功能是在早期版本中引入的，在每個版本中都添加了增量增強功能。

+ [AEM Foundation發行說明](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***✔<sup>+</sup> 對此版本中的功能進行了重大增強。***

***✔<sup>SP</sup> 表示功能可通過Service Pack或Feature Pack獲得。***

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
                <strong>Java 11支援：</strong> 支AEM持Java 11（以及Java 8）。
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
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Oak內容儲存庫</a>:</strong> 與前代Jackrabbit 2相比，提供了更高的效能和可擴充性。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">oak-run.jar索引支援</a>:</strong> 改進了Oak索引的重新/索引、統計收集和一致性檢查。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank">自定義搜索索引</a>: </strong>
                能夠添加自定義索引定義以優化查詢效能和搜索相關性。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/tw/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">聯機修訂版清除</a>:</strong>
                在不停機的情況下執行儲存庫維護。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">TarMK或MongoMK儲存庫儲存</a>:</strong>
                <br> 使用簡單、高效能的基於檔案的TarMK儲存（下一代TarPM版本）的選項
                <br> 或使用帶MongoMK的MongoDB備份儲存庫水準擴展。</td>
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
            自MongoMK推出以來，MongoMK的功能不斷AEM增強。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">AmazonS3資料儲存</a>:</strong>
            利用可擴展的雲儲存解決方案來儲存二進位資產。</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>觸摸UI功能奇偶校驗：</strong>
                不斷對創作UI進行增強，以提高工作效率並與經典UI實現功能奇偶校驗。</td>
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
                快速搜索和導航AEM。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">操作儀表板</a>:</strong>
 從內部執行維護、監控伺服器運行狀況並分析AEM效能。</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">升級改進</a>:</strong>
            升級改進可以更輕鬆、更快地就地升AEM級。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/tw/experience-manager/htl/using/overview.html" target="_blank">HTL模板語言</a>:</strong>
            一種將呈現與邏輯分離的現代模板引擎。 顯著縮短元件開發時間。 隨每個版本添加的增量功能。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">吊具模型</a>:</strong>
            一種將JCR資源建模為業務對象和邏輯的靈活框架。 隨每個版本添加的增量功能。
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
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">雲管理器</a>: </strong>
                Cloud Manager僅對Adobe Managed Services(AMS)客戶而言，它通過最先進的CI/CD管道加快了開發和部署。</td>
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

## 安全功能

下面是提供的主要安全功能清單AEM。 其中一些功能是在早期版本中引入的，在每個版本中都添加了增量增強功能。

+ [安全發行說明](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***✔表示此版本中功能的顯著增強。***

***✔<sup>+</sup> 表示功能可通過Service Pack或Feature Pack獲得。***

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
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">服務用戶</a></strong>
            <br> 區分權限，避免不必要地使用管理員權限。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/tw/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">密鑰儲存管理</a></strong>
            <br> 全局信任儲存、證書和密鑰都在儲存庫中管理。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong>CSRF</strong> <strong>保護</strong></a>
            <br> 跨站點請求偽造保護開箱。</td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/tw/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong>CORS</strong> <strong>支援</strong></a>
            <br> 跨源資源共用支援，以提高應用程式靈活性。</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://experienceleague.adobe.com/docs/" target="_blank">改進的SAML身份驗證支援</a><br>
 </strong>已解決改進的SAML重定向、優化的組資訊和密鑰加密問題。
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
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP作為OSGi配置</a><br>
 </strong>簡化LDAP身份驗證的管理和更新。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong>OSGi純文字檔案密碼加密支援<br>
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
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">CUG增強</a><br>
 </strong>已重新編寫封閉用戶組實施，以解決效能和可擴充性問題。</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔<sup>+</sup></td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">SSL嚮導</a></strong>
            <br> UI簡化SSL的設定和管理。</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">封裝的令牌支援</a></strong>
            <br> 「粘滯」會話不再需要支援發佈實例間的水準身份驗證。</td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Adobe IMS身份驗證支援</a><br>
 </strong>除Adobe Managed Services(AMS)外，通過Adobe IMS(Identity Management系統)集中管理對AEM Author實例的訪問。</td>
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

## 站點功能

以下是提供的主要站點功能清單AEM。 其中一些功能是在早期版本中引入的，在每個版本中都添加了增量增強功能。

+ [AEM Sites發佈說明](https://helpx.adobe.com/experience-manager/6-5/release-notes/sites.html)

***✔<sup>+</sup> 對此版本中的功能進行了重大增強。***

***✔<sup>SP</sup> 表示功能可通過Service Pack或Feature Pack獲得。***

<table>
    <thead>
        <tr>
            <td><strong>站點功能</strong></td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">觸控優化頁面創作</a>:</strong>
            允許編輯使用帶觸摸屏的平板電腦和電腦。</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">響應性站點創作</a>:</strong>
                佈局模式允許編輯器基於響應站點的設備寬度調整元件的大小。</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank">可編輯模板</a>:</strong>
            允許專業作者建立和編輯頁面模板。</td>
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
            加快站點開發。 GitHub上提供，可頻繁發佈計畫和靈活性。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">編SPA輯器</a>:</strong>
            使用基於React構建的單頁應用程式(SPA)框架建立可授權的Web體驗。</td>
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
            通過AEM使用上下文樣式系統定義元件的可視外觀來增加元件的重用。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/tw/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">多站點管理器(MSM)</a>:</strong>
            管理多個共用公共內容的網站（即多語言、多品牌）。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/tw/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">內容翻譯</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html" target="_blank">上下文中心</a>:</strong>
            用於個性化內容的下一代客戶端上下文框架。</td>
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
            為未來版本開發內容，而不中斷日常創作。</td>
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
            建立和管理與演示文稿脫聯的編輯內容，以便方便地重新使用。</td>
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
            建立可重複使用的體驗和針對案頭、移動和社交渠道優化的變體。</td>
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
            將內容從AEMJSON導出，以便跨設備和應用程式進行消耗。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Adobe Analytics整合和內容透視：</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank">Adobe Target整合</a>:</strong>
            逐步嚮導，建立目標體驗，建立可重用的服務庫。</td>
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
            輕鬆與新一代電子郵件營銷解決方案整合。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank">Adobe啟動整合</a>:</strong>
            與Adobe的下一代標籤管理雲服務整合。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>螢幕：</strong>
            管理數字標牌和售貨亭的體驗。</td>
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
            通過Web、移動和社交接入點提供品牌化、個性化的購物體驗。
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html" target="_blank">社區</a>:</strong>
            論壇、線程化注釋、事件日曆和許多其他功能允許與站點訪問者進行深入接觸。</td>
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

## 資產功能

下面是提供的關鍵資產功能的總AEM表。 其中一些功能是在早期版本中引入的，在每個版本中都添加了增量增強功能。

+ [AEM Assets發佈說明](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html)

***✔表示此版本中功能的顯著增強。***

***✔<sup>+</sup> 表示功能可通過Service Pack或Feature Pack獲得。***

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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">觸控優化UI</a>:</strong>
            管理台式電腦或啟用觸摸的設備上的資產。</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/metadata.html" target="_blank">高級元資料管理</a>:</strong>
            元資料模板、元資料架構編輯器和批量元資料編輯。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank">任務</a> 和 <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank">工作流</a> 管理：</strong>
            利用項目審核和批准數字資產的預構建工作流和任AEM務。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>可擴充性和效能：</strong>
            增強了對接收、上傳和儲存的大規模支援。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">資產HTTP API</a>:</strong>
            通過HTTP和JSON以寫程式方式與資產交互。</td>
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
            無需登錄即可輕鬆地共用數字資產。</td>
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
            雲服務SAAS解決方案，實現數字資產的無縫共用和分發。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">已連接資產</a>:</strong>
            AEM Sites實例可以無縫訪問和使用來自不同AEM Assets實例的資產。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">資產透視</a>:</strong>
            利用Adobe Analytics獲取數字資產和視圖的客戶交互AEM。</td>
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
            使用語言根自動支援資產元資料的翻譯。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank">智慧標籤和審核</a>:</strong>
            利用Adobe Sensei自動使用有用的元資料標籤影像。</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">智慧翻譯搜索</a>:</strong>
            搜索AEM Assets時自動翻譯搜索詞。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html" target="_blank">Adobe InDesign Server整合</a>:</strong>
            生成產品目錄。 根據InDesign模板建立手冊、傳單和印刷廣告。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/tw/experience-manager/desktop-app/aem-desktop-app.html" target="_blank">桌AEM面應用</a>:</strong>
            將資產同步到本地案頭，以便使用Creative Suite產品進行編輯。
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Adobe影像庫</a>:</strong>
                <br> Photoshop和AcrobatPDF庫用於高質量檔案操作。</td>
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
            直接從Adobe建立雲應用程式訪問AEM Assets。</td>
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
            直接從無縫訪問和使用Adobe StockAEM影像。</td>
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

***✔<sup>+</sup> 對此版本中的功能進行了重大增強。***

***✔<sup>SP</sup> 表示功能可通過Service Pack或Feature Pack獲得。***


<table>
    <thead>
        <tr>
            <td>Dynamic Media特徵</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets.html" target="_blank">成像</a>:</strong>
            以不同大小和格式動態傳送影像，包括Smart Crop。</td>
            <td> </td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">視頻</a>:</strong>
            高級視頻編碼和自適應視頻流</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/interactive-images.html" target="_blank">互動式媒體</a>:</strong>
            建立互動式橫幅、帶可點擊內容的視頻，以展示關鍵產品。
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
            <td><strong>集(<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">影像</a>。 <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">自旋</a>。 <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">混合介質</a>):</strong>
            允許用戶縮放、平移、旋轉和模擬360度的查看體驗。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/" target="_blank">查看者</a>:</strong>
            定製品牌富媒體播放器和預設，支援不同的螢幕/設備。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">交貨</a>:</strong>
            用於連結或嵌入Dynamic Media內容和通過HTTP/2協定傳遞的靈活選項。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>從Scene7升級到Dynamic Media:</strong>
            能夠遷移主資產並繼續使用現有S7 URL。</td>
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

下面是由AEM Forms提供的主要附加功能清單AEM。 其中一些功能是在早期版本中引入的，在每個版本中都添加了增量增強功能。

***✔<sup>+</sup> 對此版本中的功能進行了重大增強。***

***✔<sup>SP</sup> 表示功能可通過Service Pack或Feature Pack獲得。***

<table>
    <thead>
        <tr>
            <td>Forms特徵</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">自適應Forms編輯器</a>:</strong>
            根據設備和瀏覽器設定建立接洽、響應和自適應表單。</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">記錄文檔</a>:</strong>
            建立文檔以確保長期儲存資料捕獲體驗或打印就緒版本。</td>
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
            建立可重用的主題以設定窗體元件和面板的樣式。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/template-editor.html" target="_blank">模板編輯器</a>:</strong>
            標準化並實施適應性表單的最佳做法。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Acrobat Sign整合</a>:</strong>
            允許部署基於Acrobat Sign的綜合表單的簽名方案。</td>
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
            通過AEM Forms，您可以建立、管理和提供個性化和互動式的客戶通信。
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
            使用資料整合，基於表單中的用戶輸入從不同資料源獲取資料。 在提交表單時，捕獲的資料被寫回資料源。
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">用於Forms處理的工作流（在OSGi上）</a>:</strong>
            簡化表單批准流程的部署。</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">窗體管理器</a>:</strong>
            管理所有表單/文檔/通信（如啟用分析、翻譯、A/B測試、審閱和發佈）的單一位置。
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank">AEM Forms應用</a>:</strong>
            允許在iOS、Android或Windows上的應用內進行聯機/離線表單處理。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/adaptive-document.html" target="_blank">互動式通信</a>:</strong>
            建立豐富的通信，如具有交互元素（如圖表）的目標語句（以前稱為「自適應文檔」）。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>用於Forms處理的工作流(J2EE):</strong>
            利用直觀的IDE構建複雜的表單/以文檔為中心的工作流。</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">AEM Forms文檔安全</a>:</strong>
            安全訪問和授權PDF和Office文檔。
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">測試框架</a>:</strong>
            使用Calvin框架和Chrome插件支援和調試自適應表單。</td>
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

## 社區功能

下面是由AEM Communities提供的主要附加功能清單AEM。 其中一些功能是在早期版本中引入的，在每個版本中都添加了增量增強功能。

***✔<sup>+</sup> 對此版本中的功能進行了重大增強。***

***✔<sup>SP</sup> 表示功能可通過Service Pack或Feature Pack獲得。***

<table>
    <thead>
        <tr>
            <td> </td>
            <td>社區功能</td>
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
            <td rowspan="7">社區功能</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/forum.html" target="_blank">論壇</a>:</strong> （社會元件框架）建立新主題，或查看、跟蹤、搜索和移動現有主題。</td>
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
                詢問、查看和回答問題。</p>
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
                在發佈端建立部落格和評論。
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
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/ideation-feature.html" target="_blank">想像</a>:</strong>
                與社區建立和共用思想，或查看、遵循和評論現有思想。
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
                （社交元件框架）向站點訪問者提供社區事件資訊。
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
                上傳、管理和下載社區站點內的檔案。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/users.html#AboutCommunityGroups" target="_blank">用戶組</a>:
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">分配</a>:</strong>
            建立學習資源並將其分配給社區成員。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">啟用</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/catalog.html" target="_blank">目錄</a> 和 <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">資源管理</a>:</strong>
            從目錄訪問支援資源。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#CreateaLearningPath" target="_blank">學習路徑管理</a>:</strong>
            管理課程或支援資源組。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reports.html#main-pars_text_1739724213" target="_blank">支援報告</a>:</strong>
            報告支援資源和學習路徑。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#main-pars_text_899882038" target="_blank">關於支援的項目</a>:</strong>
            添加對啟用資源的注釋。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">支援分析</a>:</strong>
            視頻分析、進度報告和分配報告</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="8">公地</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/comments.html" target="_blank">注釋</a> 和附件：</strong>
            （社會構成框架）作為社區成員在社區站點上共用有關內容的意見和知識。</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reviews.html" target="_blank">評論</a>:</strong>
                （社會元件框架）作為社區成員，使用注釋和評級功能的組合來審閱內容。</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/rating.html" target="_blank">評級</a>:/strong&gt;（社交元件框架）作為社區成員對內容進行分級。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/voting.html" target="_blank">投票</a>:</strong>
                （社會構成框架）作為社區成員對某一內容進行投票或否決。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tag-ugc.html" target="_blank">標籤</a>:</strong>
            將標籤（關鍵字或標籤）與內容連接以快速查找內容。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/search.html" target="_blank">搜索</a>:</strong>
            預測性和建議性搜索。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/translate-ugc.html" target="_blank">翻譯</a>:</strong>
            用戶生成內容的機器翻譯。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="10">管理</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/create-site.html" target="_blank">站點管理</a>:</strong>
            建立具有社區功能的站點。</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">模板</a>:</strong>
                <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">站點</a> 和 <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tools-groups.html" target="_blank">組</a> 用於建立完全功能的社區站點的基於嚮導的模板。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>可編輯模板：</strong>
            使社區管理員能夠使用可編輯模板構建豐富AEM的體驗。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/creating-groups.html" target="_blank">組或子社區</a>:</strong>
            在社區站點內動態建立子社區。
            </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/in-context.html" target="_blank">審核</a>:</strong>
            調節用戶生成的內容。
            </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderation.html" target="_blank">批量審核</a>:</strong>
            審核控制台用於批量管理用戶生成的內容。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderate-ugc.html#CommonModerationConcepts" target="_blank">垃圾郵件檢測和粗放性篩選器</a>:</strong>
            自動垃圾郵件檢測。</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html#main-pars_text_866731966" target="_blank">響應性設計</a>:</strong>
            AEM Communities網站反應迅速。
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">分析</a>:</strong>
            與Adobe Analytics整合，瞭解社區站點使用情況的關鍵見解。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="4">成員</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/advanced.html" target="_blank">評分與簽名</a>:</strong>
            (由Adobe Sensei提供的高級評分)確定社區成員為專家並獎勵他們。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/activities.html" target="_blank">活動</a> 和 <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/notifications.html" target="_blank">通知</a>:</strong>
            查看最近的活動流，並獲得有關所關注事件的通知。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/configure-messaging.html" target="_blank">消息</a>:</strong>
            將消息直接發送給用戶和組。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/social-login.html" target="_blank">社交登錄</a>:</strong>
            使用其Facebook或Twitter帳戶登錄。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">Platform</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">MSRP（Mongo儲存）</a>:</strong>
            用戶生成的內容(UGC)直接保留在本地MongoDB實例中</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">DSRP（資料庫儲存）</a>:</strong>
            用戶生成的內容(UGC)直接保留在本地MySQL資料庫實例中。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">SRP（雲儲存）</a>:</strong>
                用戶生成的內容(UGC)被遠程保留在由Adobe托管和管理的雲服務中。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank"><strong>JSRP</a>:</strong>
                社區內容儲存在JCR中，UGC可以從發佈到的作者（或發佈）實例訪問。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sync.html" target="_blank">用戶和組同步</a>:</strong>
            使用發佈場拓撲時，在發佈實例之間同步用戶和組。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

AEM Communities通過以下方式通過發行增加了增強功能，使組織能夠參與並使其用戶能夠使用：

+ **@mention** 支援用戶生成的內容。
+ 通過以下方式改進可訪問性： **鍵盤導航** 在 **支援** 元件。
+ 改進 **批量審核** 使用 **自定義篩選器**。
+ **可編輯模板** 使社區管理員能夠在中構建豐富的社區體AEM驗。
+ 用戶現在可以發送 **批量直接消息** 組的所有成員。
