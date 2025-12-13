---
title: AEM as a Cloud Service內容移轉常見問題集
description: 取得有關將內容移轉至AEM as a Cloud Service的常見問題解答。
version: Experience Manager as a Cloud Service
doc-type: article
topic: Migration
feature: Migration
role: Developer
level: Beginner
jira: KT-11200
thumbnail: kt-11200.jpg
exl-id: bdec6cb0-34a0-4a28-b580-4d8f6a249d01
duration: 399
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1877'
ht-degree: 0%

---

# AEM as a Cloud Service內容移轉常見問題集

取得有關將內容移轉至AEM as a Cloud Service的常見問題解答。

## 術語

+ **AEMaaCS**： [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/introduction.html?lang=zh-Hant)
+ **BPA**： [最佳做法分析工具](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/best-practices-analyzer/overview-best-practices-analyzer.html?lang=zh-Hant)
+ **CTT**： [內容轉移工具](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html?lang=zh-Hant)
+ **攝影機**： [Cloud Acceleration Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/getting-started-cam.html?lang=zh-Hant)
+ **IMS**： [Identity Management系統](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/ims-support.html?lang=zh-Hant)
+ **DM**： [動態媒體](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/dynamicmedia/dm-journey/dm-journey-part1.html?lang=zh-Hant)

建立CTT相關的Adobe支援票證時，請使用以下範本以提供詳細資訊。

![內容移轉Adobe支援票證範本](../../assets/faq/adobe-support-ticket-template.png) {align="center"}

## 一般內容移轉問題

### 問：將內容移轉至AEM as Cloud Service有哪些不同方法？

有三種不同方法可供使用

+ 使用內容轉移工具(AEM 6.3+ → AEMaaCS)
+ 透過封裝管理員(AEM→AEMaaCS)
+ Assets (S3/Azure→AEMaaCS)適用的現成大量匯入服務

### 問：使用CTT可傳輸的內容數量是否有限制？

不行。CTT工具可從AEM來源擷取並內嵌至AEMaaCS。 不過，在移轉前應考量AEMaaCS平台的特定限制。

如需詳細資訊，請參閱[雲端移轉必要條件](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/prerequisites-content-transfer-tool.html?lang=zh-Hant)。

### 問：我有來自來源系統的最新BPA報告，應該怎麼做？

將報表匯出為CSV，然後將其上傳到與您的IMS組織[相關聯的](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/getting-started-cam.html?lang=zh-Hant)Cloud Acceleration Manager。 然後依照準備階段[中所述的](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/cam-readiness-phase.html?lang=zh-Hant)進行稽核程式。

請檢閱工具提供的程式碼和內容複雜性評估，並記下導致程式碼重構待處理或雲端移轉評估的相關動作專案。

### 問：建議您在來源作者上擷取資料，然後內嵌至AEMaaCS作者和發佈中嗎？

一律建議在作者與發佈層級之間執行1:1擷取和內嵌。 也就是說，擷取來源生產作者並將其擷取到開發、中繼和生產CS中是可接受的作法。

### 問：是否有方法可以估計使用CTT將內容從來源AEM移轉至AEMaaCS所需的時間？

由於移轉程式取決於網際網路頻寬、配置給CTT程式的棧積、可用的記憶體，以及每個來源系統都比較主觀的磁碟IO，因此建議及早執行移轉證明，並推斷資料點以得出預估值。

### 問：如果我啟動CTT擷取程式，來源AEM的效能會受到哪些影響？

CTT工具會在自己的Java™流程中執行，最多需要4gb的棧積，這是透過OSGi設定可設定的。 此數字可能會變更，但您可以對Java™流程進行深思熟慮並找出原因。

如果已安裝AZCopy且/或已啟用預先複製選項/驗證功能，則AZCopy程式會使用CPU週期。

除了jvm之外，此工具也會使用磁碟IO將資料儲存在可轉換的暫存空間中，且在擷取週期之後會加以清除。 除了RAM、CPU和磁碟IO之外，CTT工具也會使用來源系統的網路頻寬，將資料上傳至Azure blob存放區。

CTT擷取流程所需的資源量取決於節點數量、Blob數量及其彙總大小。 很難提供公式，因此建議執行小型移轉證明，以判斷來源伺服器大小需求。

如果使用複製環境進行移轉，則不會影響即時生產伺服器資源使用率，但在即時生產與複製之間同步內容方面，有其本身的缺點

### 問：在CTT的情境下，「擦去」和「覆寫」這兩個辭彙是什麼意思？

在[擷取階段](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html?lang=zh-Hant#extraction-setup-phase)的內容中，選項是覆寫暫存容器中先前擷取循環的資料，或將差異（新增/更新/刪除）新增至其中。 「暫存容器」並不重要，但與移轉集相關聯的blob儲存容器。 每個移轉集都有各自的暫存容器。

在[擷取階段](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/ingesting-content.html?lang=zh-Hant)的內容中，選項為+以取代AEMaaCS的整個內容存放庫，或從中繼移轉容器同步差異化（新增/更新/刪除）內容。

### 問：來源系統中有多個網站、相關資產、使用者和群組。 是否可以分階段將其移轉至AEMaaCS？

可以，但需要謹慎規劃：

+ 建立移轉集時會假設網站和資產位於其各自的階層中
   + 確認是否可接受在一個移轉集中移轉所有資產，然後分階段使用它們的網站
+ 在目前狀態中，即使發佈層仍可提供內容，作者擷取程式仍會使作者例項無法用於內容製作
   + 這表示在擷取完成至作者之前，內容製作活動都會凍結
+ 雖然群組已遷移，但使用者已不再遷移。

在規劃移轉之前，請先詳閱追加擷取和擷取程式，如檔案所述。

### 問：即使AEMaaCS作者或發佈執行個體正在進行內嵌，我的網站是否仍可供一般使用者使用？

可以。一般使用者流量不會因內容移轉活動而中斷。 不過，作者擷取會凍結內容製作，直到完成為止。

### 問：BPA報表會顯示與遺失原始轉譯相關的專案。 我是否應在擷取之前清理來源？

可以。缺少原始轉譯表示資產二進位檔一開始並未正確上傳。 將其視為不良資料，請檢閱、使用封裝管理員（視需要）進行備份，並在執行擷取之前從來源AEM中將其移除。 不良資料會在資產處理步驟上造成負面結果。

### 問：BPA報告包含與資料夾缺少`jcr:content`節點相關的專案。 我該拿它們做什麼？

當資料夾層級缺少`jcr:content`時，任何傳播設定的動作，例如處理設定檔等 來自父項的將在此層級中斷。 請檢視遺失`jcr:content`的原因。 雖然這些資料夾可以移轉，但請注意，這類資料夾會降低使用者體驗，並造成不必要的疑難排解週期。

### 問：我已經建立了移轉集。 是否可以檢查其大小？

是，有[檢查大小](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html?lang=zh-Hant#migration-set-size)功能是CTT的一部分。

### 問：我正在執行移轉（擷取、擷取）。 是否可以驗證我擷取的所有內容是否已擷取至Target？

是，有[驗證](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/validating-content-transfers.html?lang=zh-Hant)功能，是CTT的一部分。

### 問：我的客戶需要在AEMaaCS環境（例如從AEMaaCS Dev到AEMaaCS Stage或到AEMaaCS Prod）之間移動內容。 我可以針對這些使用案例使用內容轉移工具嗎？

很遺憾，否。 CTT的使用案例是將內容從內部部署/AMS託管的AEM 6.3+來源移轉到AEMaaCS雲端環境。 [請閱讀CTT檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html?lang=zh-Hant)。

### 問：擷取期間可能會出現哪些問題？

提取階段是一個涉及的過程，需要多個方面才能如預期般運作。 瞭解可能會發生的不同問題以及如何緩解這些問題，可提高內容移轉的整體成功率。

公開檔案會根據學習內容持續改善，以下是一些高階問題類別和可能的基本原因。

![AEM as a Cloud Service內容移轉擷取問題](../../assets/faq/extraction-issues.jpg) {align="center"}

### 問：在擷取期間可能會出現哪些問題？

擷取階段會完全在雲端平台中進行，並且需要有權存取AEMaaCS基礎結構的資源的協助。 請建立支援票證以取得更多說明。

以下是可能的問題類別（請勿將此視為專屬清單）

![AEM as a Cloud Service內容移轉擷取問題](../../assets/faq/ingestion-issues.jpg) {align="center"}



### 問：我的來源伺服器需要輸出網際網路連線才能讓CTT正常運作嗎？

簡短答案是&quot;**是**&quot;。

CTT流程需要連線至以下資源：

+ 目標AEM as a Cloud Service環境： `author-p<program_id>-e<env_id>.adobeaemcloud.com`
+ Azure Blob儲存服務： `casstorageprod.blob.core.windows.net`

請參閱檔案以取得有關[來源連線能力](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html?lang=zh-Hant#source-environment-connectivity)的詳細資訊。

## 資產處理Dynamic Media相關問題

### 問：資產在AEMaaCS中擷取後是否會自動重新處理？

不行。若要處理資產，必須起始重新處理的請求。

### 問：資產在AEMaaCS中擷取後是否會自動重新索引？

可以。資產會根據AEMaaCS上可用的索引定義重新編制索引。

### 問：來源AEM與Dynamic Media整合。 內容移轉前是否有任何必須考量的特定事項？

是，當來源AEM具有Dynamic Media整合時，請考慮下列事項。

+ AEMaaCS僅支援Dynamic Media Scene7模式。 如果來源系統處於混合模式，則需要將DM移轉至Scene7模式。
+ 如果方法是從來源翻制例項移轉，則可以安全地停用用於CTT的翻制上的DM整合。 此步驟純粹是為了避免任何寫入DM或避免DM流量載入。
+ 請注意，CTT會將節點、移轉集的中繼資料從來源AEM移轉到AEMaaCS。 不會直接在DM上執行任何作業。

### 問：Source AEM上出現DM整合時，有哪些不同的移轉方法？

請先閱讀上述問題和答案

（這兩個選項皆可，但不限於上述兩個）。 這取決於客戶希望如何進行UAT、效能測試、可用的環境，以及是否使用翻制進行移轉。 請將這兩者視為討論的起點

**選項1**

如果來源環境中的資產/節點數量位於低端（約100K），假設這些資產可於24至72小時內移轉（包括擷取和擷取），則較佳的方法為

+ 直接從生產環境執行移轉
+ 使用`wipe=true`執行初始擷取並擷取至AEMaaCS
   + 此步驟會移轉所有節點和二進位檔
+ 繼續使用內部部署/AMS Prod作者
+ 從現在開始，請使用`wipe=true`執行所有其他移轉週期證明
   + 請注意，此操作會移轉完整節點存放區，但只會移轉修改過的Blob，而不是整個Blob。 前一組的Blob位於目標AEMaaCS例項的Azure Blob存放區中。
   + 使用此移轉證明來測量移轉持續時間、測試、驗證所有其他功能
+ 最後，在上線當週之前，執行擦去=true移轉
   + 在AEMaaCS上連線Dynamic Media
   + 中斷與AEM內部部署來源的DM設定

使用此選項，您可以執行一對一移轉，也就是內部部署Dev→AEMaaCS Dev等。 並從個別環境中移動DM設定

（如果移轉計畫從翻制執行）

**選項2**

+ 建立生產作者的翻制，從翻制中移除DM設定
+ 將內部部署複製移→AEMaaCS Dev/Stage
   + 將生產DM公司短暫連線到AEMaaCS Dev/Stage以進行驗證
   + 在DM連線作用中期間，避免將資產擷取到AEMaaCS
   + 這可讓他們驗證CTT、DM特定驗證
+ 在AEMaaCS上完成測試後
   + 執行從內部部署階段到AEMaaCS階段的抹除移轉

執行從內部部署Dev到AEMaaCS Dev的清除移轉。

上述方法僅可用於測量移轉期間，但需要稍後再加以清除。

## 其他資源

+ [在雲端中移轉至Experience Manager ( Summit 2022)的秘訣與技巧](https://business.adobe.com/tw/summit/2022/sessions/tips-and-tricks-for-migrating-to-experience-manage-tw109.html)

+ [CTT專家系列影片](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.html?lang=zh-Hant)

+ 其他AEMaaCS主題的[專家系列影片](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/expert-resources/aem-experts-series.html?lang=zh-Hant)
