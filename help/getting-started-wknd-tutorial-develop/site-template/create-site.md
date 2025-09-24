---
title: 建立網站 | AEM Quick Site Creation
description: 了解如何使用網站建立精靈來產生新網站。Adobe 提供的標準網站範本是新網站的起點。
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
workflow-type: ht
source-wordcount: '959'
ht-degree: 100%

---

# 建立網站 {#create-site}

作為 Quick Site Creation 的一部分，在 Adobe Experience Manager (AEM) 中使用網站建立精靈來產生新網站。Adobe 所提供的標準網站範本可用作新網站的起點。

## 先決條件 {#prerequisites}

本章節中的步驟將在 Adobe Experience Manager as a Cloud Service 環境中進行。請確認您擁有 AEM 環境的管理存取權。在完成本教學課程時，建議使用[沙箱程式](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html)和[開發環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html)。

本教學課程也可以使用[生產程式](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html)環境；但是，請確保本教學課程的活動不會影響在目標環境上執行的工作，因為本教學課程會把內容和程式碼部署到目標 AEM 環境。

此教學課程的部分內容可以使用 [AEM SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html)。本教學課程中依賴雲端服務的部分，例如[使用 Cloud Manager 的前端管道部署主題](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html)，無法在 AEM SDK 上執行。

檢閱[上線文件](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)了解更多詳細資訊。

## 目標 {#objective}

1. 了解如何使用網站建立精靈來產生新網站。
1. 了解網站範本的角色。
1. 探索所產生的 AEM 網站。

## 登入 Adobe Experience Manager Author {#author}

第一步，登入您的 AEM as a Cloud Service 環境。AEM 環境分為 **Author 服務**&#x200B;和 **Publish 服務**。

* **Author 服務** - 建立、管理和更新網站內容的地方。通常只有內部使用者可以存取 **Author 服務**，並且在登入畫面後才能存取。
* **Publish 服務** - 託管即時網站。這是終端使用者將會看到的服務，而且通常公開使用。

教學課程的大部分內容將在 **Author 服務**&#x200B;中進行。

1. 導覽至 Adobe Experience Cloud [https://experience.adobe.com/](https://experience.adobe.com/)。使用個人帳戶或公司/學校帳戶登入。
1. 務必在選單中選取正確的組織，然後按一下 **Experience Manager**。

   ![Experience Cloud 首頁](assets/create-site/experience-cloud-home-screen.png)

1. 在 **Cloud Manager** 之下，按一下「**啟動**」。
1. 將滑鼠游標停留在您想要使用的程式上，然後按一下「**Cloud Manager 程式**」圖示。

   ![Cloud Manager 程式圖示](assets/create-site/cloud-manager-program-icon.png)

1. 在頂部選單中按一下「**環境**」來檢視所佈建的環境。

1. 找到您想要使用的環境並按一下「**作者 URL**」。

   ![存取開發作者](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >此教學課程建議使用&#x200B;**開發**&#x200B;環境。

1. AEM **Author 服務**&#x200B;啟動一個新的索引標籤。按一下「**使用 Adobe 登入**」，而您應該會使用相同的 Experience Cloud 認證自動登入。

1. 經過重新導向和驗證之後，您現在應該會看到 AEM 開始畫面。

   ![AEM 啟動畫面](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> 無法存取 Experience Manager？檢閱[上線文件](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)

## 下載基本網站範本

網站範本是新網站的起點。網站範本包括一些基本主題、頁面範本、設定和範例內容。網站範本中具體包含的內容由開發人員決定。Adobe 提供&#x200B;**基本網站範本**&#x200B;來加速新的實施。

1. 開啟新的瀏覽器標籤並導覽到 GitHub 上的基本網站範本專案：[https://github.com/adobe/aem-site-template-standard](https://github.com/adobe/aem-site-template-standard)。那是開放原始碼的專案，且授權開放給所有人使用。
1. 按一下「**發佈版本**」並導覽至「[最新版本](https://github.com/adobe/aem-site-template-standard/releases/latest)」。
1. 展開「**資產**」下拉式選單並下載範本 zip 檔：

   ![基本網站範本 Zip](assets/create-site/template-basic-zip-file.png)

   此 zip 檔案會在下一個練習中使用。

   >[!NOTE]
   >
   > 本教學課程是使用基本網站範本 **1.1.0** 版本編寫。當開始供生產使用的新專案時，始終建議使用最新版本。

## 建立新網站

接下來，使用上一個練習中的網站範本產生一個新網站。

1. 返回 AEM 環境。從 AEM 開始畫面導覽到「**Sites**」。
1. 在右上角點按一下「**建立** > **網站 (範本)**」。這樣會啟動「**建立網站精靈**」。
1. 在「**選取網站範本**」下，按一下「**匯入**」按鈕。

   上傳從上一個練習下載的 **.zip** 範本檔案。

1. 選取「**基本 AEM 網站範本**」並按一下「**下一步**」。

   ![選取網站範本](assets/create-site/select-site-template.png)

1. 在「**網站詳細資料** > **網站標題**」下輸入「`WKND Site`」。

   在真實世界的實施中，您的公司或組織的品牌名稱會取代「WKND Site」。在本教學課程中，我們模擬為虛構的生活風格品牌「WKND」建立一個網站。

1. 在「**網站名稱**」下輸入「`wknd`」。

   ![網站範本詳細資訊](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > 若是使用共用的 AEM 環境，請在「**網站名稱**」上附加唯一識別碼。例如 `wknd-site-johndoe`。這樣將確保多位使用者可以完成相同的教學課程，而不會發生任何衝突。

1. 按一下「**建立**」來產生網站。當 AEM 建立網站完成後，按一下「**成功**」對話框中的「**完成**」。

## 探索新網站

1. 導覽至 AEM Sites 控制台，若您不在那裡的話。
1. 已產生新的 **WKND 網站**。其中包括一個具有多語言階層的網站結構。
1. 選取頁面並按一下選單列中的「**編輯**」按鈕，開啟「**英文** > **首頁**」頁面：

   ![WKND 網站階層](assets/create-site/wknd-site-starter-hierarchy.png)

1. 已經建立入門內容，並提供數個可供新增至頁面的元件。使用這些元件進行實驗，以便對其功能有所了解。在下一章，您將了解元件的基礎知識。

   ![開始首頁](assets/create-site/start-home-page.png)

   *網站範本提供的範例內容*

## 恭喜！ {#congratulations}

恭喜，您剛剛已建立第一個 AEM 網站！

### 後續步驟 {#next-steps}

使用 Adobe Experience Manager (AEM) 中的頁面編輯器，更新「[製作內容並發佈](author-content-publish.md)」章節中的網站內容。了解如何設定原子元件來更新內容。了解 AEM 作者和發佈環境之間的區別，並學習如何將更新發佈到即時網站。
