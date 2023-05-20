---
title: AEMas a Cloud Service內容遷移常見問題
description: 獲取有關內容遷移到as a Cloud Service的常見問題AEM的答案。
version: Cloud Service
doc-type: article
feature: Migration
topic: Migration
role: Architect, Developer
level: Beginner
kt: 11200
thumbnail: kt-11200.jpg
exl-id: bdec6cb0-34a0-4a28-b580-4d8f6a249d01
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '2283'
ht-degree: 0%

---

# AEMas a Cloud Service內容遷移常見問題

獲取有關內容遷移到as a Cloud Service的常見問題AEM的答案。

## 術語

+ **AEMaCS**: [AEMas a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/introduction.html)
+ **雙酚A**: [最佳做法分析器](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/best-practices-analyzer/overview-best-practices-analyzer.html)
+ **CTT**: [內容傳輸工具](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html)
+ **CAM**: [雲加速管理器](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/getting-started-cam.html)
+ **IMS**: [Identity Management系統](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/ims-support.html)
+ **DM**: [Dynamic Media](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/dynamicmedia/dm-journey/dm-journey-part1.html)

請使用下面的模板在建立與CTT相關的Adobe支援票證時提供詳細資訊。

![內容遷移Adobe支援票證模板](../../assets/faq/adobe-support-ticket-template.png) { align=&quot;center&quot; }

## 一般內容遷移問題

### 問：將內容作為Cloud Services遷移到的不同方AEM法是什麼？

有三種不同的方法

+ 使用內容傳輸工AEM具(6.3+ → AEMaaCS)
+ 通過包管理器(AEM→ AEMaaCS)
+ 現成資產批量導入服務(S3/Azure → AEMaaCS)

### 問：使用CTT可以傳輸的內容量是否有限制？

否. CTT作為工具可以從源端提取AEM，並導入AEMaaCS。 但是，在遷移前應考慮對AEMaaCS平台的特定限制。

有關詳細資訊，請參閱 [雲遷移先決條件](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/prerequisites-content-transfer-tool.html)。

### 問：我從源系統收到最新的BPA報告，我應如何處理？

將報告導出為CSV，然後將其上載到雲加速管理器， [與IMS組織關聯](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-acceleration-manager/using-cam/getting-started-cam.html)。 然後以以下方式完成審核過程： [就緒階段中概述](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-acceleration-manager/using-cam/cam-readiness-phase.html)。

請查看該工具提供的代碼和內容複雜性評估，並記下導致代碼重構積壓工作或雲遷移評估的相關措施項。

### 問：是否建議從源作者中抽取並輸入到AEMaaCS作者中並發佈？

始終建議在作者和發佈層之間執行1:1提取和攝取。 即提取源生產作者並將其導入開發、階段和生產CS是可接受的。

### 問：是否有一種方法來估計時間，使用CTT將內容從源遷AEM移到AEMaaCS?

由於遷移過程取決於Internet頻寬、為CTT進程分配的堆、可用空閒記憶體和磁碟IO（對每個源系統而言都是主觀的），因此建議提前執行遷移證明並推斷資料點以得出估計值。

### 問：如果啟動CTT提取AEM過程，如何影響源效能？

CTT工具在其自己的Java™進程中運行，該進程佔用多達4gb堆，可通過OSGi配置進行配置。 此數字可能會更改，但您可以為Java™進程進行篩選並查找。

如果安裝了AZCopy並/或啟用了「預複製」選項/驗證功能，則AZCopy進程會佔用CPU週期。

除jvm外，該工具還使用磁碟IO將資料儲存在過渡臨時空間上，並在提取週期後將對其進行清理。 除RAM、CPU和磁碟IO外，CTT工具還使用源系統的網路頻寬將資料上傳到Azure Blob儲存區。

CTT提取過程所佔用的資源量取決於節點數、Blob數及其聚合大小。 很難提供公式，因此建議執行小遷移證明以確定源伺服器的升級要求。

如果將克隆環境用於遷移，則它不會影響即時生產伺服器資源利用率，但在即時生產和克隆之間同步內容方面有其自身的缺點

### 問：在我的源作者系統中，我們為用戶配置了SSO，以驗證到作者實例中。 在這種情況下，我是否必須使用CTT的「用戶映射」功能？

簡單的回答是&quot;**是**。

CTT提取與攝取 **無** 用戶映射僅將內容、關聯原則（用戶、組）從源遷移AEM到AEMaaCS。 但是，Adobe IMS中存在這些用戶（標識）並且有（預配）對AEMaaCS實例的訪問權以成功驗證。 工作 [用戶映射工具](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-migration/content-transfer-tool/user-mapping-tool/overview-user-mapping-tool.html) 將本地用戶映射AEM到IMS用戶，使驗證和授權協同工作。

在這種情況下， SAML標識提供程式是針對Adobe IMS配置的，以使用聯合/Enterprise ID，而不是直接使AEM用驗證處理程式。

### 問：在我的源作者系統中，我們為用戶配置了基本身份驗證，以便用本地用戶驗證到作者實AEM例中。 在這種情況下，我是否必須使用CTT的「用戶映射」功能？

簡單的回答是&quot;**是**。

CTT提取和接收（不用用戶映射）確實將內容、相關原則（用戶、組）從源遷移AEM到AEMaaCS。 但是，Adobe IMS中存在這些用戶（標識）並且有（預配）對AEMaaCS實例的訪問權以成功驗證。 工作 [用戶映射工具](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-migration/content-transfer-tool/user-mapping-tool/overview-user-mapping-tool.html) 將本地用戶映射AEM到IMS用戶，使驗證和授權協同工作。

在這種情況下，用戶使用個人Adobe ID,IMS管理員使用Adobe ID來提供對AEMaaCS的訪問。

### 問：在CTT的上下文中，術語「bwe」和「overwrite」是什麼意思？

在 [提取相位](https://experienceleague.adobe.com/docs/experience-manager-cloud-servicemoving/cloud-migration/content-transfer-tool/extracting-content.html)，這些選項要麼用於從以前的提取週期覆蓋轉移容器中的資料，要麼將差異（添加/更新/刪除）添加到中。 暫存容器不是任何內容，但是與遷移集關聯的blob儲存容器。 每個遷移集都獲取其自己的轉移容器。

在 [攝入相](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-migration/content-transfer-tool/ingesting-content.html)，這些選項是+，用於替換AEMaaCS的整個內容儲存庫，或從暫存遷移容器同步差異（添加/更新/刪除）內容。

### 問：源系統中有多個網站、關聯資產、用戶、組。 是否可以分階段將它們遷移到AEMaaCS?

是的，這是可能的，但需要謹慎規劃：

+ 建立遷移集（假定站點、資產進入其各自的層次結構）
   + 驗證是否可以將所有資產作為一個遷移集的一部分進行遷移，然後分階段將正在使用這些資產的站點帶入
+ 在當前狀態下，作者攝取過程使作者實例無法用於內容創作，即使發佈層仍可以為內容提供服務
   + 這意味著，在將攝取完成為作者之前，內容創作活動將被凍結

在規劃遷移之前，請按照說明查看「Top up extraction and ingestion」過程。

### 問：即使AEMaaCS作者或發佈實例中都發生接收問題，我的網站是否仍可供最終用戶使用？

可以。最終用戶流量不會因內容遷移活動而中斷。 但是，作者攝取會凍結內容創作，直到完成。

### 問：BPA報告顯示與缺少原始格式副本相關的項目。 在提取前，我應該先清理一下？

可以。缺少的原始格式副本意味著資產二進位檔案最初沒有正確上載。 將其視為壞資料，請查看，使用包管理器進行備份（視需要而定），然後在運行提取之前從AEM源中刪除它們。 不良資料將對資產處理步驟產生負結果。

### 問：BPA報告包含與丟失相關的項 `jcr:content` 資料夾的節點。 我該怎麼處理他們？

當 `jcr:content` 在資料夾級別缺少，任何傳播設定的操作（如處理配置檔案等）都會丟失。 從父母那裡休息。 請查看丟失的原因 `jcr:content`。 儘管這些資料夾可以遷移，但請注意，此類資料夾會降低用戶體驗，並導致以後出現不必要的故障排除週期。

### 問：我已建立遷移集。 能檢查它的大小嗎？

是的，有個 [檢查大小](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html#migration-set-size) 屬於CTT的特徵。

### 問：我正在執行遷移（提取、攝取）。 是否可以驗證我提取的所有內容都被攝取到目標中？

是的，有個 [驗證](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/validating-content-transfers.html) 屬於CTT的特徵。

### 問：我的客戶要求在AEMaaCS環境之間移動內容，如從AEMaaCS Dev移動到AEMaaCS Stage或移動到AEMaaCS Prod。 我是否可以將這些使用情形使用內容傳輸工具？

不幸的是，沒有。 CTT的使用案例是將內容從本地/AMS托管的AEM6.3+源遷移到AEMaaCS雲環境。 [請閱讀CTT文檔](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html)。

### 問：在採掘過程中，預期會出現哪些問題？

抽取階段是一個涉及的過程，需要多個方面才能按預期工作。 瞭解可能發生的各種問題以及如何緩解這些問題將提高內容遷移的總體成功率。

公共文檔在學習的基礎上不斷得到改進，但下面是一些高級別問題類別和可能的根本原因。

![AEMas a Cloud Service內容遷移提取問題](../../assets/faq/extraction-issues.jpg) { align=&quot;center&quot; }

### 問：食物攝取期間會出現哪些問題？

攝入階段完全發生在雲平台中，需要訪問AEMaaCS基礎架構的資源提供幫助。 請建立支援票證以獲取更多幫助。

以下是可能的問題類別（請不要將此列為排他清單）

![AEMas a Cloud Service內容遷移問題](../../assets/faq/ingestion-issues.jpg) { align=&quot;center&quot; }



### 問：我的源伺服器是否需要具有出站Internet連接才能使CTT工作？

簡單的回答是&quot;**是**。

CTT流程需要連接到以下資源：

+ 目標AEMas a Cloud Service環境： `author-p<program_id>-e<env_id>.adobeaemcloud.com`
+ Azure Blob儲存服務： `casstorageprod.blob.core.windows.net`
+ 用戶映射IO終結點： `usermanagement.adobe.io`

有關 [源連接](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html#source-environment-connectivity)。

## 資產處理Dynamic Media相關問題

### 問：在AEMaaCS中接收後，資產是否將自動重新處理？

否. 要處理資產，必須啟動重新處理請求。

### 問：在AEMaaCS中接收後，資產是否將自動重新索引？

可以。根據AEMaaCS上可用的索引定義重新索引這些資產。

### 問：消息來AEM源與Dynamic Media合併。 在內容遷移之前是否必須考慮任何特定內容？

是的，當來源有Dynamic Media整合時，請AEM考慮以下內容。

+ AEMaaCS僅支援Dynamic MediaScene7模式。 如果源系統處於混合模式，則需要將DM遷移到Scene7模式。
+ 如果方法是從源克隆實例遷移，則可以安全地禁用將用於CTT的克隆上的DM整合。 此步驟純粹是為了避免向DM寫入任何內容或避免對DM通信的載入。
+ 請注意， CTT將節點、遷移集的元資料從源遷移AEM到AEMaaCS。 它不會直接對DM執行任何操作。

### 問：當源上存在DM整合時，有哪些不同的遷移方AEM法？

請先閱讀上述問題並回答

（這是兩種可能的選擇，但不限於這兩種選擇）。 它取決於客戶希望如何進行UAT 、效能測試、可用環境以及是否正在使用克隆進行遷移。 請將這兩者作為討論的起點

**選項1**

如果源環境中的資產/節點數量處於較低端(~100K)，假設這些資產/節點可以在24 + 72小時（包括提取和攝取）內遷移，則最好的方法是

+ 直接從生產環境中執行遷移
+ 使用 `wipe=true`
   + 此步驟遷移所有節點和二進位檔案
+ 繼續在內部/AMS Prod作者上工作
+ 從現在開始，運行所有其他遷移週期的證明 `wipe=true`
   + 請注意，此操作將遷移完整節點儲存，但只遷移已修改的Blob，而不遷移整個Blob。 上一組Blob位於目標AEMaaCS實例的Azure Blob儲存區中。
   + 使用此遷移證明來測量遷移持續時間、用戶映射、測試和驗證所有其他功能
+ 最後，在上線周之前，執行wabe=true遷移
   + 在AEMaCS上連接Dynamic Media
   + 從本地源斷開AEMDM配置

使用此選項，您可以運行一到一遷移，即On-prem Dev → AEMaaCS Dev，等等。 並將DM配置從各自的環境中移動

（如果計畫從克隆執行遷移）

**選項2**

+ 建立生產作者的克隆，從克隆中刪除DM配置
+ 遷移本地克隆→ AEMaaCS開發/階段
   + 將生產DM公司簡短連接到AEMaaCS開發/階段，以進行驗證
   + 在DM連接處於活動狀態期間，避免資產接收到AEMaaCS
   + 這使他們能夠驗證CTT、DM特定的驗證
+ 在AEMaCS上完成測試後
   + 運行從內部部署階段到AEMaaCS階段的擦除遷移

運行從內部部署Dev到AEMaaCS Dev的擦除遷移。

上述方法可僅用於測量遷移持續時間，但需要稍後進行清理。

## 其他資源

+ [遷移到雲中Experience Manager的技巧和技巧(Summit 2022)](https://business.adobe.com/summit/2022/sessions/tips-and-tricks-for-migrating-to-experience-manage-tw109.html)

+ [CTT專家系列視頻](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-servicemigration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.html)

+ [其他AEMaCS主題的專家系列視頻](https://experienceleague.adobe.com/docs/experience-manager-learncloud-service/aem-experts-series.html)
