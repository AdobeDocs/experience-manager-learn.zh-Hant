---
title: 建立網站 | AEM快速網站建立
description: 瞭解如何使用網站建立精靈來產生新網站。 Adobe提供的標準網站範本是新網站的起點。
version: Experience Manager as a Cloud Service
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
jira: KT-7496
thumbnail: KT-7496.jpg
doc-type: Tutorial
exl-id: 6d0fdc4d-d85f-4966-8f7d-d53506a7dd08
recommendations: noDisplay, noCatalog
duration: 198
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '959'
ht-degree: 0%

---

# 建立網站 {#create-site}

快速建立網站時，請使用Adobe Experience Manager (AEM)中的網站建立精靈來產生新網站。 Adobe提供的標準網站範本是作為新網站的起點。

## 先決條件 {#prerequisites}

本章中的步驟將在Adobe Experience Manager as a Cloud Service環境中進行。 確保您擁有AEM環境的管理存取權。 完成本教學課程時，建議使用[沙箱程式](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html)和[開發環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html)。

[生產計畫](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html)環境也可用於本教學課程；不過，請確保本教學課程的活動不會影響目標環境中正在執行的工作，因為本教學課程會將內容和程式碼部署到目標AEM環境。

[AEM SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html)可用於本教學課程的部分內容。 此教學課程依賴雲端服務的部分，例如[使用Cloud Manager的前端管道](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html)部署主題，無法在AEM SDK上執行。

如需詳細資訊，請檢閱[上線檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)。

## 目標 {#objective}

1. 瞭解如何使用網站建立精靈來產生新網站。
1. 瞭解網站範本的角色。
1. 探索產生的AEM網站。

## 登入Adobe Experience Manager Author {#author}

首先，請登入您的AEM as a Cloud Service環境。 AEM環境在&#x200B;**作者服務**&#x200B;和&#x200B;**發佈服務**&#x200B;之間分割。

* **作者服務** — 用來建立、管理和更新網站內容。 通常只有內部使用者才能存取&#x200B;**作者服務**，而且是在登入畫面之後。
* **發佈服務** — 裝載已上線的網站。 這是一般使用者會看到的服務，通常可公開使用。

大部分教學課程將使用&#x200B;**作者服務**&#x200B;進行。

1. 導覽至Adobe Experience Cloud [https://experience.adobe.com/](https://experience.adobe.com/)。 使用您的個人帳戶或公司/學校帳戶登入。
1. 確定在功能表中選取正確的組織，然後按一下&#x200B;**Experience Manager**。

   ![Experience Cloud首頁](assets/create-site/experience-cloud-home-screen.png)

1. 在&#x200B;**Cloud Manager**&#x200B;底下，按一下&#x200B;**啟動**。
1. 將游標停留在您要使用的程式上，然後按一下&#x200B;**Cloud Manager程式**&#x200B;圖示。

   ![Cloud Manager程式圖示](assets/create-site/cloud-manager-program-icon.png)

1. 在頂端功能表中按一下&#x200B;**環境**&#x200B;以檢視布建的環境。

1. 找到您要使用的環境，然後按一下&#x200B;**作者URL**。

   ![存取開發作者](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >建議在此教學課程中使用&#x200B;**開發**&#x200B;環境。

1. 已針對AEM **作者服務**&#x200B;啟動新索引標籤。 按一下&#x200B;**使用Adobe**&#x200B;登入，您應該使用相同的Experience Cloud認證自動登入。

1. 在重新導向並驗證之後，您現在應該會看到AEM開始畫面。

   ![AEM開始畫面](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> 存取Experience Manager時發生問題？ 檢閱[上線檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)

## 下載基本網站範本

網站範本提供新網站的起點。 網站範本包括一些基本主題、頁面範本、設定和範例內容。 確切地說，網站範本中包含的內容取決於開發人員。 Adobe提供&#x200B;**基本網站範本**，可加速新實作。

1. 開啟新的瀏覽器索引標籤，並導覽至GitHub上的基本網站範本專案： [https://github.com/adobe/aem-site-template-standard](https://github.com/adobe/aem-site-template-standard)。 此專案是開放原始碼的，並授權任何人使用。
1. 按一下&#x200B;**版本**&#x200B;並瀏覽至[最新版本](https://github.com/adobe/aem-site-template-standard/releases/latest)。
1. 展開&#x200B;**Assets**&#x200B;下拉式清單，然後下載範本zip檔案：

   ![基本網站範本Zip](assets/create-site/template-basic-zip-file.png)

   下個練習將使用此zip檔案。

   >[!NOTE]
   >
   > 本教學課程是使用&#x200B;**1.1.0**&#x200B;版的基本網站範本撰寫的。 啟動新專案以供生產使用時，一律建議使用最新版本。

## 建立新網站

接下來，使用上一個練習中的「網站範本」產生新網站。

1. 返回AEM環境。 從AEM開始畫面導覽至&#x200B;**網站**。
1. 按一下右上角的&#x200B;**建立** > **網站（範本）**。 這會開啟&#x200B;**建立網站精靈**。
1. 在&#x200B;**選取網站範本**&#x200B;下，按一下&#x200B;**匯入**&#x200B;按鈕。

   上傳從上一個練習下載的&#x200B;**.zip**&#x200B;範本檔案。

1. 選取&#x200B;**基本AEM網站範本**&#x200B;並按一下&#x200B;**下一步**。

   ![選取網站範本](assets/create-site/select-site-template.png)

1. 在&#x200B;**網站詳細資料** > **網站標題**&#x200B;下，輸入`WKND Site`。

   在真實世界的實作中，「WKND網站」將由您公司或組織的品牌名稱取代。 在本教學課程中，我們會模擬建立虛擬生活風格品牌「WKND」的網站。

1. 在&#x200B;**網站名稱**&#x200B;下，輸入`wknd`。

   ![網站範本詳細資料](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > 如果使用共用AEM環境，請將唯一識別碼附加至&#x200B;**網站名稱**。 例如 `wknd-site-johndoe`。這可確保多位使用者可完成相同的教學課程，而不會出現任何衝突。

1. 按一下&#x200B;**建立**&#x200B;以產生網站。 當AEM完成建立網站時，請在&#x200B;**成功**&#x200B;對話方塊中按一下&#x200B;**完成**。

## 探索新網站

1. 導覽至AEM Sites主控台（如果尚未存在）。
1. 已產生新的&#x200B;**WKND網站**。 它將包含具有多語言階層的網站結構。
1. 選取頁面，然後按一下功能表列中的&#x200B;**編輯**&#x200B;按鈕，開啟&#x200B;**英文** > **首頁**&#x200B;頁面：

   ![WKND網站階層](assets/create-site/wknd-site-starter-hierarchy.png)

1. 已建立入門內容，且有數個元件可新增至頁面。 嘗試使用這些元件以瞭解功能。 您將在下一章中學習元件的基本知識。

   ![開始首頁](assets/create-site/start-home-page.png)

   *網站範本所提供的範例內容*

## 恭喜！ {#congratulations}

恭喜，您剛剛建立了您的第一個AEM網站！

### 後續步驟 {#next-steps}

在Adobe Experience Manager、AEM中使用頁面編輯器，以更新[作者內容和發佈](author-content-publish.md)章節中的網站內容。 瞭解如何設定原子元件以更新內容。 瞭解AEM作者與發佈環境之間的差異，並瞭解如何發佈更新至已上線的網站。
