---
title: AEMas a Cloud Service內容移轉常見問題集
description: 取得關於內容移轉至AEMas a Cloud Service的常見問題解答。
version: Cloud Service
doc-type: article
feature: Migration
topic: Migration
role: Architect, Developer
level: Beginner
kt: 11200
thumbnail: kt-11200.jpg
source-git-commit: bc222867c937b7d498e7b56bebc0aac18289ad03
workflow-type: tm+mt
source-wordcount: '2289'
ht-degree: 0%

---


# AEMas a Cloud Service內容移轉常見問題集

取得關於內容移轉至AEMas a Cloud Service的常見問題解答。

## 術語

+ **AEMaCS**: [AEMas a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/introduction.html)
+ **BPA**: [Best Practices Analyzer](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/best-practices-analyzer/overview-best-practices-analyzer.html)
+ **CTT**: [內容轉移工具](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html)
+ **CAM**: [Cloud Acceleration Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/getting-started-cam.html)
+ **IMS**: [Identity Management系統](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/ims-support.html)

請使用下列範本，在建立CTT相關Adobe支援票證時提供更多詳細資訊。

![內容移轉Adobe支援票證範本](../../assets/faq/adobe-support-ticket-template.png) { align=&quot;center&quot; }

### 問：將內容以Cloud Services形式移轉至AEM的不同方法為何？

答：有三種不同的方法可用

+ 使用內容轉移工具(AEM 6.3+ → AEMaCS)
+ 透過套件管理器(AEM → AEMaCS)
+ 資產的立即可用大量匯入服務(S3/Azure → AEMaCS)

### 問：使用CTT可傳輸的內容量是否有限制？

答：不。 CTT作為工具可從AEM來源擷取並內嵌至AEMaCS。 不過，AEMaaCS平台有某些特定限制，移轉前應先考慮這些限制。

如需詳細資訊，請參閱 [雲端移轉必要條件](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/prerequisites-content-transfer-tool.html).

### 問：我有來源系統的最新BPA報告，該怎麼辦？

答：將報表匯出為CSV，然後上傳至Cloud Acceleration Manager、 [與您的IMS組織相關聯](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-acceleration-manager/using-cam/getting-started-cam.html). 接著，請依照 [在準備階段中概述](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-acceleration-manager/using-cam/cam-readiness-phase.html).

請檢閱此工具提供的程式碼和內容複雜度評估，並記下導致程式碼重構積壓工作或雲端移轉評估的相關動作項目。

### 問：是否建議對來源作者擷取，並擷取至AEMaaCS作者和發佈中？

答：建議一律在製作層級和發佈層級之間執行1:1提取和擷取。 也就是說，可以提取源生產作者，並將其擷取到開發、預備和生產CS中。

### 問：是否有方法可預估時間，使用CTT將內容從來源AEM移轉至AEMaCS所需的時間？

答：由於遷移過程取決於網際網路頻寬、為CTT過程分配的堆、可用記憶體和磁碟IO，這些對每個源系統都是主觀的，因此建議提前執行遷移證明，並推斷資料點以得出估計值。

### 問：如果啟動CTT提取程式，會如何影響來源AEM效能？

答：CTT工具會在其專屬的Java™程式中執行，該程式最多需要4gb堆，可透過OSGi設定設定。 此數字可能會變更，但您可以對Java™程式進行整理，並加以了解。

如果安裝了AZCopy並且/或啟用了預複製選項/驗證功能，則AZCopy進程將佔用CPU週期。

除jvm外，該工具還使用磁碟IO將資料儲存在過渡臨時空間上，並將在提取週期後清理。 除了RAM、CPU和磁碟IO外，CTT工具還使用源系統的網路頻寬將資料上傳到Azure Blob儲存區。

CTT擷取程式所佔用的資源量取決於節點數、Blob數量及其匯總大小。 很難提供公式，因此建議執行小型遷移證明以確定源伺服器的升級要求。

如果將克隆環境用於遷移，則它不會影響即時生產伺服器資源利用率，但在即時生產和克隆之間同步內容方面，它有其自身的缺點

### 問：在我的來源製作系統中，已設定SSO，讓使用者驗證至製作例項。 在此情況下，我是否必須使用CTT的使用者對應功能？

答：簡單的答案是「**是**」。

CTT擷取和擷取 **無** 使用者對應僅將內容、相關原則（使用者、群組）從來源AEM移轉至AEMaCS。 但Adobe IMS中必須有這些使用者（身分），且必須擁有（布建）AEMaCS例項的存取權，才能成功驗證。 工作 [使用者對應工具](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-migration/content-transfer-tool/user-mapping-tool/overview-user-mapping-tool.html) 是將本機AEM使用者對應至IMS使用者，讓驗證和授權可共同運作。

在此情況下，SAML身分提供者會根據Adobe IMS來設定，以使用Federated /Enterprise ID，而非使用驗證處理常式直接傳送至AEM。

### 問：在我的來源製作系統中，我們已設定基本驗證，讓使用者透過本機AEM使用者驗證到製作例項中。 在此情況下，我是否必須使用CTT的使用者對應功能？

答：簡單的答案是「**是**」。

不使用者對應的CTT擷取和擷取功能會將內容、相關原則（使用者、群組）從來源AEM移轉至AEMaCS。 但Adobe IMS中必須有這些使用者（身分），且必須擁有（布建）AEMaCS例項的存取權，才能成功驗證。 工作 [使用者對應工具](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-migration/content-transfer-tool/user-mapping-tool/overview-user-mapping-tool.html) 是將本機AEM使用者對應至IMS使用者，讓驗證和授權可共同運作。

在此情況下，使用者會使用個人Adobe ID，而IMS管理員會使用Adobe ID來提供AEMaCS的存取權。

### 問：在CTT中，「擦去」和「覆寫」一詞代表什麼意思？

答：在 [提取階段](https://experienceleague.adobe.com/docs/experience-manager-cloud-servicemoving/cloud-migration/content-transfer-tool/extracting-content.html)，選項是從先前的擷取週期覆寫測試容器中的資料，或將差異（新增/更新/刪除）新增至其中。 測試容器不是任何項目，但是Blob儲存容器與移轉集相關聯。 每個移轉集都有各自的測試容器。

在 [擷取階段](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-migration/content-transfer-tool/ingesting-content.html)，選項為+ ，可取代AEMaaCS的整個內容存放庫，或從測試移轉容器同步差異（新增/更新/刪除）內容。

### 問：來源系統中有多個網站、相關資產、使用者、群組。 可以分階段移轉至AEMaCS嗎？

答：是的，這是可能的，但需要謹慎規劃：

+ 建立移轉集時，假設網站、資產會進入其個別階層
   + 驗證是否可以將所有資產作為一個遷移集的一部分進行遷移，然後分階段將使用這些資產的站點帶入
+ 在目前狀態中，即使發佈層級仍可提供內容，製作擷取程式仍會讓製作例項無法用於內容編寫
   + 這表示在內嵌完成為製作之前，內容製作活動會凍結

在規劃遷移之前，請先查看已記錄的追加提取和提取過程。

### 問：即使擷取發生在AEMaaCS作者或發佈執行個體中，我的網站仍可供一般使用者使用嗎？

答：是。 內容移轉活動不會中斷一般使用者流量。 不過，製作擷取會凍結內容製作，直到完成為止。

### 問：BPA報表會顯示與遺失原始轉譯相關的項目。 提取前應先在來源上清除嗎？

答：是。 遺失的原始轉譯表示資產二進位檔一開始無法正確上傳。 將其視為壞資料，請查看，使用Package Manager進行備份（視需要），並在運行提取之前從源AEM中刪除它們。 不良資料會對資產處理步驟產生負面結果。

### 問：BPA報告包含與缺失相關的項目 `jcr:content` 資料夾的節點。 我該怎麼處理它們？

答：當 `jcr:content` 在資料夾層級遺失，則會傳播設定（例如處理設定檔等）的任何動作。 從父母那裡分手。 請查看丟失的原因 `jcr:content`. 即使這些資料夾可以遷移，請注意，這些資料夾會降低用戶體驗，並在以後造成不必要的故障排除週期。

### 問：我已建立移轉集。 可以檢查它的大小嗎？

答：是，有 [檢查大小](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html#migration-set-size) 屬於CTT的功能。

### 問：我正在執行移轉（提取、擷取）。 是否可驗證我擷取的所有內容都擷取到target中？

答：是，有 [驗證](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/validating-content-transfers.html) 屬於CTT的功能。

### 問：我的客戶需要在AEMaaCS環境之間移動內容，例如從AEMaaCS Dev移動到AEMaaCS Stage或AEMaCS Prod。 我可以針對這些使用案例使用內容轉移工具嗎？

答：很遺憾，不。 CTT的使用案例是將內容從內部部署/AMS托管的AEM 6.3+來源移轉至AEMaCS雲端環境。 [請參閱CTT檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html).

### 問：在採掘過程中會遇到哪些問題？

答：提取階段是相關的程式，需要多個方面才能如預期般運作。 了解可能發生的不同問題以及如何緩解這些問題，可提升內容移轉的整體成功。

根據經驗不斷改進公共檔案，但以下是一些高級別問題類別和可能的根本原因。

![AEMas a Cloud Service內容移轉提取問題](../../assets/faq/extraction-issues.jpg) { align=&quot;center&quot; }

### 問：擷取期間會預期出現哪些問題？

答：擷取階段完全發生在雲端平台中，需要可存取AEMaCS基礎架構之資源的協助。 請建立支援票證以獲得更多幫助。

以下是可能的問題類別（請勿將此列為專屬清單）

![AEMas a Cloud Service內容移轉問題](../../assets/faq/ingestion-issues.jpg) { align=&quot;center&quot; }



### 問：我的源伺服器是否需要有出站Internet連接才能CTT工作？

答：簡單的答案是「**是**」。

CTT程式需要連接以下資源：

+ 目標AEMas a Cloud Service環境： `author-p<program_id>-e<env_id>.adobeaemcloud.com`
+ Azure blob儲存服務： `casstorageprod.blob.core.windows.net`
+ 用戶映射IO終結點： `usermanagement.adobe.io`

如需詳細資訊，請參閱本檔案 [源連接](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html#source-environment-connectivity).

## 資產處理Dynamic Media相關問題

### 問：在AEMaaCS中擷取後，資產是否會自動重新處理？

答：不。 若要處理資產，必須起始重新處理的請求。

### 問：在AEMaaCS中擷取後，資產是否會自動重新編列索引？

答：是。 系統會根據AEMaaCS上可用的索引定義，對資產重新編列索引。

### 問：來源AEM與Dynamic Media整合。 內容移轉之前是否必須考慮任何特定事項？

答：是的，當來源AEM具有Dynamic Media整合時，請考量下列事項。

+ AEMaCS僅支援Dynamic Media Scene7模式。 如果源系統處於混合模式，則需要將DM遷移到Scene7模式。
+ 如果方法是從源克隆實例遷移，則可以禁用用於CTT的克隆上的DM整合。 此步驟純粹是為了避免對DM進行任何寫入，或避免DM流量載入。
+ 請注意，CTT會將節點、移轉集的中繼資料從來源AEM移轉至AEMaCS。 它不會直接對DM執行任何操作。

### 問：Source AEM上存在DM整合時，有哪些不同的移轉方法？

答：請先閱讀上述問題並回答

（這是兩個可能的選項，但不限於這兩個選項）。 它取決於客戶希望如何進行UAT 、效能測試、可用環境，以及是否正在使用克隆進行遷移。 請將這兩者作為討論的起點

**選項1**

如果來源環境中的資產/節點數量位於較低端(~100K)，假設這些資產可在24 + 72小時（包括提取和擷取）期間移轉，則最好的方法是

+ 直接從生產環境執行遷移
+ 使用執行初始擷取並擷取至AEMaCS `wipe=true`
   + 此步驟會移轉所有節點和二進位檔
+ 繼續進行內部部署/ AMS Prod製作
+ 從現在開始，使用 `wipe=true`
   + 請注意，此操作會遷移整個節點儲存區，但只遷移已修改的Blob，而不是整個Blob。 目標AEMaaCS例項的Azure blob儲存區中存在先前的Blob集。
   + 使用此遷移證明來測量遷移持續時間、用戶映射、測試、驗證所有其他功能
+ 最後，在上線當周前，執行wave=true遷移
   + 在AEMaaCS上連線Dynamic Media
   + 從AEM本地源斷開DM配置

透過此選項，您可以執行一對一的移轉，亦即內部開發→ AEMaCS Dev等。 並將DM配置從各個環境中移動

（如果計畫從克隆執行遷移）

**選項2**

+ 建立生產製作的克隆，從克隆中刪除DM配置
+ 移轉內部部署→ AEMaCS Dev / Stage
   + 將生產DM公司簡短連結至AEMaCS開發/預備，以用於驗證目的
   + 在DM連線作用中期間，請避免將資產擷取至AEMaaCS
   + 這可讓他們驗證CTT、DM特定驗證
+ 在AEMaaCS上完成測試後
   + 執行從內部部署階段移轉至AEMaCS階段的擦去作業

執行從內部部署Dev移轉至AEMaCS Dev的擦去程式。

以上方法只能用於測量遷移持續時間，但需要稍後清理。

### 其他資源

+ [雲端中移轉至Experience Manager的秘訣與技巧(Summit 2022)](https://business.adobe.com/summit/2022/sessions/tips-and-tricks-for-migrating-to-experience-manage-tw109.html)

+ [CTT專家系列影片](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-servicemigration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.html)

+ [其他AEMaaCS主題的專家系列影片](https://experienceleague.adobe.com/docs/experience-manager-learncloud-service/aem-experts-series.html)
