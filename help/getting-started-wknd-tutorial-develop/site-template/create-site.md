---
title: 建立網站 | AEM快速網站建立
description: 瞭解如何使用網站建立精靈來產生新網站。 Adobe提供的標準網站範本是新網站的起點。
version: Cloud Service
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
jira: KT-7496
thumbnail: KT-7496.jpg
doc-type: Tutorial
exl-id: 6d0fdc4d-d85f-4966-8f7d-d53506a7dd08
recommendations: noDisplay, noCatalog
duration: 235
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '959'
ht-degree: 0%

---

# 建立網站 {#create-site}

快速建立網站時，請使用Adobe Experience Manager AEM中的網站建立精靈來產生新網站。 Adobe提供的標準網站範本是作為新網站的起點。

## 先決條件 {#prerequisites}

本章中的步驟將在Adobe Experience Manager as a Cloud Service環境中進行。 確保您擁有AEM環境的管理存取權。 建議使用 [沙箱計畫](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html) 和 [開發環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html) 完成本教學課程時。

[生產計畫](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html) 環境也可以用於本教學課程；但是，請確保本教學課程的活動不會影響目標環境中正在執行的工作，因為本教學課程會將內容和程式碼部署到目標AEM環境。

此 [AEM SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html) 可用於本教學課程的部分內容。 本教學課程依賴雲端服務的方面，例如 [使用Cloud Manager的前端管道部署主題](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html)，無法在AEM SDK上執行。

檢閱 [入門檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html) 以取得更多詳細資料。

## 目標 {#objective}

1. 瞭解如何使用網站建立精靈來產生新網站。
1. 瞭解網站範本的角色。
1. 探索產生的AEM網站。

## 登入Adobe Experience Manager Author {#author}

首先，請登入您的AEMas a Cloud Service環境。 AEM環境分為兩種 **作者服務** 和 **發佈服務**.

* **作者服務**  — 建立、管理和更新網站內容的位置。 通常只有內部使用者才能存取 **作者服務** 和位於登入畫面後面。
* **發佈服務**  — 託管已上線的網站。 這是一般使用者會看到的服務，通常可公開使用。

大部分的教學課程將使用 **作者服務**.

1. 導覽至Adobe Experience Cloud [https://experience.adobe.com/](https://experience.adobe.com/). 使用您的個人帳戶或公司/學校帳戶登入。
1. 確定在功能表中選取了正確的組織，然後按一下 **Experience Manager**.

   ![Experience Cloud首頁](assets/create-site/experience-cloud-home-screen.png)

1. 在 **Cloud Manager** 按一下 **Launch**.
1. 將游標停留在您要使用的程式上，然後按一下 **Cloud Manager計畫** 圖示。

   ![Cloud Manager計畫圖示](assets/create-site/cloud-manager-program-icon.png)

1. 在頂端功能表中按一下 **環境** 以檢視布建的環境。

1. 找到您要使用的環境，然後按一下 **作者URL**.

   ![存取開發作者](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >建議使用 **開發** 本教學課程的環境。

1. 新索引標籤會啟動至AEM **作者服務**. 按一下 **使用Adobe登入** 而且您應該會使用相同的Experience Cloud憑證自動登入。

1. 在重新導向並驗證之後，您現在應該會看到AEM開始畫面。

   ![AEM開始畫面](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> 存取Experience Manager時發生問題？ 檢閱 [入門檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)

## 下載基本網站範本

網站範本提供新網站的起點。 網站範本包括一些基本主題、頁面範本、設定和範例內容。 確切地說，網站範本中包含的內容取決於開發人員。 Adobe提供 **基本網站範本** 加速新實作。

1. 開啟新的瀏覽器索引標籤，並導覽至GitHub上的基本網站範本專案： [https://github.com/adobe/aem-site-template-standard](https://github.com/adobe/aem-site-template-standard). 此專案是開放原始碼的，並授權任何人使用。
1. 按一下 **發行版本** 並導覽至 [最新版本](https://github.com/adobe/aem-site-template-standard/releases/latest).
1. 展開 **資產** 下拉式清單並下載範本zip檔案：

   ![基本網站範本Zip](assets/create-site/template-basic-zip-file.png)

   下個練習將使用此zip檔案。

   >[!NOTE]
   >
   > 本教學課程使用版本撰寫 **1.1.0** 基本網站範本的。 啟動新專案以供生產使用時，一律建議使用最新版本。

## 建立新網站

接下來，使用上一個練習中的「網站範本」產生新網站。

1. 返回AEM環境。 從AEM開始畫面導覽至 **網站**.
1. 在右上角按一下 **建立** > **網站（範本）**. 這會顯示 **建立網站精靈**.
1. 在 **選取網站範本** 按一下 **匯入** 按鈕。

   上傳 **.zip** 從上一個練習下載的範本檔案。

1. 選取 **基本AEM網站範本** 並按一下 **下一個**.

   ![選取網站範本](assets/create-site/select-site-template.png)

1. 在 **網站詳細資訊** > **網站標題** 進入 `WKND Site`.

   在真實世界的實作中，「WKND網站」將由您公司或組織的品牌名稱取代。 在本教學課程中，我們會模擬建立虛擬生活風格品牌「WKND」的網站。

1. 在 **網站名稱** 進入 `wknd`.

   ![網站範本詳細資料](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > 如果使用共用AEM環境，請將唯一識別碼附加至 **網站名稱**. 例如 `wknd-site-johndoe`. 這可確保多位使用者可完成相同的教學課程，而不會出現任何衝突。

1. 按一下 **建立** 以產生「網站」。 按一下 **完成** 在 **成功** AEM對話方塊。

## 探索新網站

1. 導覽至AEM Sites主控台（如果尚未存在）。
1. 新 **WKND網站** 「 」已產生。 它將包含具有多語言階層的網站結構。
1. 開啟 **英文** > **首頁** 選取頁面並按一下 **編輯** 功能表列中的按鈕：

   ![WKND網站階層](assets/create-site/wknd-site-starter-hierarchy.png)

1. 已建立入門內容，且有數個元件可新增至頁面。 嘗試使用這些元件以瞭解功能。 您將在下一章中學習元件的基本知識。

   ![開始首頁](assets/create-site/start-home-page.png)

   *網站範本提供的範例內容*

## 恭喜！ {#congratulations}

恭喜，您剛剛建立了您的第一個AEM網站！

### 後續步驟 {#next-steps}

使用Adobe Experience Manager AEM中的頁面編輯器，更新以下位置的網站內容： [製作內容與發佈](author-content-publish.md) 章節。 瞭解如何設定原子元件以更新內容。 瞭解AEM作者與發佈環境之間的差異，並瞭解如何發佈更新至已上線的網站。
