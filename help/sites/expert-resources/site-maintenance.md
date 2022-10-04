---
title: 您的例行網站維護指南
seo-title: Your Routine Site Maintenance Guide
description: 無論您是管理員、作者或開發人員，網站維護都會接觸到AEM Sites例項的各個層面。 使用本指南來確保您的策略已設定好以獲得成功。
seo-description: Whether you're an admin, author, or developer, site maintenance touches every aspect of your AEM Sites instance. Use this guide to ensure your strategy is set up for success.
audience: author, marketer, developer
source-git-commit: d545e7bb5e937959e2ede2b3c1ecfc312df5a044
workflow-type: tm+mt
source-wordcount: '1094'
ht-degree: 1%

---


# 網站維護提示與秘訣

安裝和維護AEM例項有三個選項

* AEMaCS（雲端服務） — 系統隨時待命、保持最新，並可視需要動態調整規模
* Adobe Managed Services，讓Adobe客戶服務工程師執行所有的每日/每週/每月維護工作，並確保已安裝所有Service Pack，且系統一律安全且正常運作
* 在本地運行it ，您必須負責整個系統，包括備份、升級和安全性。

如果您選擇在內部部署實施自己的系統，請謹記以下幾點，以確保您擁有安全、高效能的系統。 除了「照料和餵食」項目外，本文還將指出AEM開發人員應謹記的幾個項目，以協助系統保持正常運行。

## 管理員

備份 — 確保您有頻繁的完整和/或部分備份：

* 每日
* 每週
* 每月

許多客戶執行快照備份，假定基礎作業系統支援此類備份，只需幾分鐘時間。 確保這些備份被正確儲存(在AEM系統外)。 確保備份正常工作，並且可用於定期重新建立工作系統 — 沒有什麼比系統崩潰更糟的了，而且您的備份因某些原因而損壞！

您需要監控幾個項目，以確保無故障操作：

### 例行維護

#### [索引維護](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/practices/best-practices-for-queries-and-indexing.html?lang=en)

索引允許查詢盡可能快地運行，從而釋放資源以用於其他操作。 確保索引呈頂端形狀！ AEM會取消追蹤的查詢，而非使用索引來避免一個錯誤查詢影響整體AEM效能。

#### [Tar壓縮/修訂清除](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/revision-cleanup.html?lang=en)

每個對儲存庫的更新都會建立新的內容修訂。 因此，每次更新時，存放庫的大小都會增加。 為避免儲存庫增長失控，需要清理舊修訂版本以釋放磁碟資源。

#### [Lucene二進位檔清除](https://experienceleague.adobe.com/docs/experience-manager-64/administering/operations/operations-dashboard.html?lang=en#automated-maintenance-tasks)

清除lucene二進位檔，並減少執行中的資料存放區大小需求。

#### [資料儲存垃圾](https://experienceleague.adobe.com/docs/experience-manager-64/administering/operations/data-store-garbage-collection.html?lang=en)

刪除AEM中的資產時，可能會從節點階層中移除對基礎資料存放區記錄的參考，但資料存放區記錄本身仍會保留。 此未引用的資料儲存記錄會變成「垃圾」，不需要保留。 在存在許多未引用資產的情況下，將這些資產去除、保留空間、優化備份和檔案系統維護效能是有益的。

#### [工作流程清除](https://experienceleague.adobe.com/docs/experience-manager-64/administering/operations/workflows-administering.html?lang=en)

將工作流實例數減到最少會提高工作流引擎的效能，因此您可以定期從儲存庫中清除已完成或正在運行的工作流實例。

#### [審核日誌維護](https://experienceleague.adobe.com/docs/experience-manager-64/administering/operations/operations-audit-log.html?lang=en)

符合稽核記錄資格的AEM事件會產生許多封存資料。 由於複製、資產上傳和其他系統活動，此資料可能會隨著時間而快速增長。

#### [安全性](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-checklist.html?lang=en)

確保密切遵循安全性檢查清單最佳實務，以確保最安全的AEM例項。

#### Diskspace

監視diskspace以確保您有足夠的空間容納JCR儲存庫，並且再增加大約一半的空間 — tar壓縮在運行時會使用額外的空間。 磁碟空間用盡是JCR損壞的首要原因！

## 開發人員

請嘗試不使用自訂元件 — 使用 [核心元件](https://www.aemcomponents.dev/). 您的目標應是只節省80-90%的時間使用核心元件，並使用自訂元件。 這通常需要一種查看頁面上元件的新方式 — 您必須了解，使用CSS的前端開發人員可輕鬆重新設定元件的樣式。 同時，也要記住，這些核心元件可以相互嵌入，以取得相當複雜的結果。 創意！

### [樣式系統](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html?lang=en)

樣式系統可讓核心元件（甚至自訂元件）的外觀和風格有所改變，供作者自行決定，以建立全新的外觀元件。 這些風格變化通常只涉及前端設計師和知識淵博的作者（通常稱為「超級作者」）

### [啟動](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/launches/overview.html?lang=en)

啟動可讓新促銷、銷售或網站的工作完成，而不會影響目前部署的頁面。 此外，作者可以排程自動上線，不需出席或監督，讓作者今天即可完成下週（或下季）的工作，而不需在頁面開發上線的前一天匆忙上線 — 這真的是時代的禮物！)

### [內容片段](https://experienceleague.adobe.com/docs/experience-manager-64/assets/fragments/content-fragments.html?lang=en)

內容片段是可自訂的「區塊」資訊，可輕鬆在整個網站上重複使用。 如果需要更改，只需更改原始塊，更新就會顯示在使用它的所有位置 — 立即！

### [體驗片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)

雖然聽起來幾乎與內容片段相同，但體驗片段是頁面的小型、可見片段。 這些變數也可在您的網站上廣泛重複使用，並在AEM內的中央位置進行維護，以便在數秒內（而非數天或數週）輕鬆對您的網站進行潛在的全域變更。

請先思考，看看可能重複使用的項目。 腳？ 免責聲明？ 標題？ 特定類型的內容？ 所有這些內容都可以在整個站點之間共用，同時保持最低的維護。 需要更新免責聲明中的日期，但該日期位於您的網站上1,000個頁面上？ 如果您使用體驗片段，則為5秒的操作！

## 一般

通過持續學習，隨時了解AEM的變化 — 不要被困在過去。 使用 [Experience League](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/overview.html?lang=en) 和 [Adobe數位學習服務(ADLS)](https://learning.adobe.com/) 磨練你的技能。

## 結論

AEM可能是一個大型系統，而且需要許多類型的人才才能讓它「唱」。 從管理員到開發人員（前端和硬體Java開發人員），到作者 — 每個人都能找到適合的工具！ 如果您不想處理日常管理工作，一律會有AMS和AEMas a Cloud Service。
