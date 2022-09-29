---
title: 建立網站 | AEM快速網站建立
description: 了解如何使用「網站建立」精靈產生新網站。 Adobe提供的標準網站範本是新網站的起點。
sub-product: sites
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
kt: 7496
thumbnail: KT-7496.jpg
exl-id: 6d0fdc4d-d85f-4966-8f7d-d53506a7dd08
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '912'
ht-degree: 0%

---

# 建立網站 {#create-site}

快速建立網站時，請使用AEM Adobe Experience Manager中的「網站建立精靈」來產生新網站。 由Adobe提供的「標準網站範本」會作為新網站的起點。

## 必備條件 {#prerequisites}

本章中的步驟將在Adobe Experience Manager as a Cloud Service環境中進行。 確保您擁有AEM環境的管理存取權。 建議使用 [沙箱方案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html) 和 [開發環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html) 完成本教學課程時。

檢閱 [入門檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html) 以取得更多詳細資訊。

## 目標 {#objective}

1. 了解如何使用「站點建立嚮導」生成新站點。
1. 了解網站範本的角色。
1. 探索產生的AEM網站。

## 登入Adobe Experience Manager Author {#author}

首先，登入您的AEMas a Cloud Service環境。 AEM環境會分割為 **作者服務** 和 **發佈服務**.

* **作者服務**  — 建立、管理和更新網站內容的位置。 通常只有內部使用者可存取 **作者服務** 和位於登入畫面後。
* **發佈服務**  — 托管即時網站。 這項服務是一般使用者會看到，且通常可公開使用。

大部分的教學課程將使用 **作者服務**.

1. 導覽至Adobe Experience Cloud [https://experience.adobe.com/](https://experience.adobe.com/). 使用您的個人帳戶或公司/學校帳戶登入。
1. 確保在菜單中選擇了正確的組織，然後按一下 **Experience Manager**.

   ![Experience Cloud首頁](assets/create-site/experience-cloud-home-screen.png)

1. 在 **Cloud Manager** 按一下 **Launch**.
1. 將滑鼠指標暫留在您要使用的方案上，然後按一下 **Cloud Manager計畫** 表徵圖。

   ![Cloud Manager方案圖示](assets/create-site/cloud-manager-program-icon.png)

1. 在頂端功能表中按一下 **環境** 檢視布建的環境。

1. 找到您要使用的環境，然後按一下 **作者URL**.

   ![存取開發作者](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >建議使用 **開發** 環境。

1. 新索引標籤會啟動至AEM **作者服務**. 按一下 **使用Adobe登入** 且您應使用相同的Experience Cloud憑證自動登入。

1. 重新導向與驗證後，您現在應該會看到AEM開始畫面。

   ![AEM開始畫面](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> 存取Experience Manager時是否有問題？ 檢閱 [入門檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)

## 下載基本網站範本

網站範本提供新網站的起點。 「網站範本」包含一些基本主題、頁面範本、設定和範例內容。 開發人員能決定「網站範本」中包含的內容。 Adobe提供 **基本網站範本** 來加速新實作。

1. 開啟新的瀏覽器標籤，並導覽至GitHub上的「基本網站範本」專案： [https://github.com/adobe/aem-site-template-standard](https://github.com/adobe/aem-site-template-standard). 該項目是開源的，並授權任何人使用。
1. 按一下 **版本** 並導覽至 [最新版本](https://github.com/adobe/aem-site-template-standard/releases/latest).
1. 展開 **資產** 下拉式清單和下載範本zip檔案：

   ![基本網站範本Zip](assets/create-site/template-basic-zip-file.png)

   此zip檔案將用於下一個練習。

   >[!NOTE]
   >
   > 本教學課程是使用版本撰寫 **1.1.0** 的下限。 啟動新專案供生產使用時，一律建議使用最新版本。

## 建立新網站

接下來，使用上一練習中的「網站範本」生成新網站。

1. 返回AEM環境。 從AEM開始畫面導覽至 **網站**.
1. 在右上角按一下 **建立** > **網站（範本）**. 這會顯示 **建立站點嚮導**.
1. 在 **選擇網站模板** 按一下 **匯入** 按鈕。

   上傳 **.zip** 從上次練習下載的範本檔案。

1. 選取 **基本AEM網站範本** 按一下 **下一個**.

   ![選擇網站模板](assets/create-site/select-site-template.png)

1. 在 **網站詳細資訊** > **網站標題** 輸入 `WKND Site`.

   在實際實作中，「WKND網站」將會取代為您的公司或組織的品牌名稱。 在本教學課程中，我們模擬為虛構的生活風格品牌「WKND」建立網站。

1. 在 **網站名稱** 輸入 `wknd`.

   ![網站範本詳細資訊](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > 如果使用共用AEM環境，請將唯一識別碼附加至 **網站名稱**. 例如 `wknd-site-johndoe`. 這可確保多位使用者能完成相同的教學課程，不會有任何衝突。

1. 按一下 **建立** 來產生網站。 按一下 **完成** 在 **成功** 對話框。

## 探索新網站

1. 導覽至AEM Sites主控台（如果尚未存在）。
1. 新 **WKND站點** 已產生。 其中將包含具有多語言階層的網站結構。
1. 開啟 **英文** > **首頁** 頁面，方法是選取頁面並按一下 **編輯** 按鈕（在菜單欄中）:

   ![WKND網站階層](assets/create-site/wknd-site-starter-hierarchy.png)

1. 已建立入門內容，並可將多個元件新增至頁面。 試用這些元件，以了解功能。 您將在下一章中了解元件的基本知識。

   ![開始首頁](assets/create-site/start-home-page.png)

   *網站範本提供的範例內容*

## 恭喜！ {#congratulations}

恭喜，您剛建立了第一個AEM網站！

### 後續步驟 {#next-steps}

使用AEM Adobe Experience Manager中的頁面編輯器，更新 [製作內容與發佈](author-content-publish.md) 章節。 了解如何設定原子元件以更新內容。 了解AEM製作和發佈環境之間的差異，並了解如何將更新發佈至即時網站。
