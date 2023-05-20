---
title: 日常站點維護指南
seo-title: Your Routine Site Maintenance Guide
description: 無論您是管理員、作者還是開發人員，站點維護都涉及您的AEM Sites實例的各個方面。 使用本指南確保您的策略已設定為成功。
seo-description: Whether you're an admin, author, or developer, site maintenance touches every aspect of your AEM Sites instance. Use this guide to ensure your strategy is set up for success.
audience: author, marketer, developer
exl-id: 37ee3234-f91c-4f0a-b0b7-b9167e7847a9
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '1084'
ht-degree: 2%

---

# 站點維護技巧

在安裝和維護實例時有三個選AEM項

* AEMaaCS（雲服務） — 系統始終處於開機狀態、最新狀態，並根據需要動態擴展
* Adobe托管服務，Adobe客戶服務工程師可以執行每日/每週/每月的所有維護，並確保安裝了所有Service Pack，並且系統始終安全且運行順利
* 在本地運行它，您必須負責整個系統，包括備份、升級和安全。

如果您選擇在內部部署您自己的系統，請記住以下幾點，以確保您擁有安全、高效能的系統。 除了「照顧和餵食」項目外，本文還將指出開發AEM人員應注意的幾個項目，以幫助系統保持良好運行。

## 管理員

備份 — 確保按照頻繁的時間表執行完整和/或部分備份：

* 每日
* 每週
* 月

許多客戶都執行快照備份，假定底層作業系統支援此類備份，這隻需幾分鐘。 確保這些備份儲存正確(在系統AEM外)。 確保備份正常工作，並且可以定期用於重新建立工作系統 — 最糟糕的莫過於系統崩潰，而且您的備份因某種原因而損壞！

您需要監控以下幾項以確保無故障操作：

### 例行維護

#### [索引維護](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/practices/best-practices-for-queries-and-indexing.html?lang=en)

索引允許查詢盡可能快地運行，從而釋放資源用於其他操作。 確保索引處於頂端形狀！ 取AEM消travere的查詢，而不是使用索引來防止一個錯誤查詢影響整體AEM效能。

#### [焦油壓縮/修訂版清理](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/revision-cleanup.html?lang=en)

每個對儲存庫的更新都建立一個新的內容修訂版本。 因此，每次更新後，儲存庫的大小都會增大。 為避免儲存庫增長失控，需要清理舊版本以釋放磁碟資源。

#### [Lucene二進位檔案清理](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/operations-dashboard.html#automated-maintenance-tasks)

清除lucene二進位檔案並減少正在運行的資料儲存大小要求。

#### [資料儲存垃圾](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/data-store-garbage-collection.html)

當刪除中的AEM資產時，對基礎資料儲存記錄的引用可以從節點層次中刪除，但資料儲存記錄本身仍然保留。 此未引用的資料儲存記錄將變成「垃圾」，無需保留。 在存在許多未引用資產的情況下，將這些資產去除、保留空間、優化備份和檔案系統維護效能是有益的。

#### [工作流程清除](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/workflows-administering.html)

最大限度地減少工作流實例的數量會提高工作流引擎的效能，因此您可以定期從儲存庫中清除已完成或正在運行的工作流實例。

#### [審核日誌維護]https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/operations-audit-log.html

符合審AEM計日誌記錄的事件會生成大量存檔資料。 由於複製、資產上載和其他系統活動，此資料可以隨著時間的推移而快速增長。

#### [安全性](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-checklist.html?lang=zh-Hant)

確保密切遵循安全核對表最佳做法，以確保最安全的實例AEM。

#### 磁碟空間

監控磁碟空間以確保您有足夠的空間容納JCR儲存庫，再加上大約一半的空間 — tar壓縮在運行時會使用額外的空間。 磁碟空間不足是JCR損壞的頭號原因！

## 開發人員

嘗試不使用自定義元件 — 使用 [核心元件](https://www.aemcomponents.dev/)。 您的目標應是僅少使用80-90%的核心元件和定制元件。 這通常需要一種查看頁面上元件的新方法 — 您必須認識到，使用CSS的前端開發人員可以輕鬆地重新設定元件的樣式。 還要記住，這些核心元件可以相互嵌入，以取得相當複雜的結果。 有創意！

### [樣式系統](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html?lang=en)

樣式系統允許核心元件，甚至定制元件，根據作者的判斷來改變其外觀和感覺，以建立全新外觀元件。 這些風格變化通常只涉及前端設計師和知識淵博的作者（通常稱為「超級作者」）

### [Launch](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/launches/overview.html?lang=en)

啟動允許完成新的促銷、銷售或網站部署的工作，而不會影響當前部署的頁面。 此外，它們可以自動即時運行，無需出席或監督，使作者能夠在今天完成下週（或下季度）的工作，而不會在頁面開發工作開始前匆忙進行 — 這真的是TIME的禮物！)

### [內容片段](https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments.html)

內容片段是可定製的「區塊」資訊，可在整個站點上輕鬆重用。 如果需要更改，只需更改原始區塊，並且更新會在使用它的任何位置立即顯示！

### [體驗片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)

雖然聽起來幾乎與內容片段相同，但「體驗片段」是頁面的小片、可見片段。 這些內容還可以在您的站點中廣泛重複使用，並在中心位置進行維護AEM，以便在幾秒內（而不是幾天或幾週內）輕鬆完成整個站點進行潛在全局更改的任務。

請先考慮，看看可以重新使用的內容。 頁腳？ 免責聲明？ 標題？ 某些類型的內容？ 所有這些都可以在整個站點上共用，同時將維護工作保持在最低水準。 需要在免責聲明中更新日期，但該日期在您網站上的1,000頁？ 如果您使用體驗片段，則需要5秒的操作！

## 一般

通過繼續學AEM習跟上變化 — 不要被困在過去。 使用 [Experience League](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/overview.html?lang=en) 和 [Adobe數字學習服務(ADLS)](https://learning.adobe.com/) 磨練你的技能。

## 結論

可AEM以是一個大系統，需要很多人才能「唱」。 從管理員到開發人員（前端和硬體Java開發人員），再到作者 — 每個人都有自己的選擇！ 如果您不想處理日常管理，則總有AMS和AEMas a Cloud Service。
