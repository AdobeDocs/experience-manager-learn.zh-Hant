---
title: 建立網站
seo-title: 開始使用AEM Sites-建立網站
description: 使用Adobe Experience Manager的網站建立精靈AEM產生新網站。 Adobe提供的「標準網站範本」是新網站的起點。
sub-product: Sites
version: Cloud Service
type: Tutorial
topic: 內容管理
feature: 核心元件，頁面編輯器
role: Developer
level: Beginner
kt: 7496
thumbnail: KT-7496.jpg
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '902'
ht-degree: 0%

---


# 建立站點{#create-site}

>[!CAUTION]
>
> 此處展示的快速網站建立功能將於2021年下半年推出。 相關檔案可供預覽使用。

本章介紹在Adobe Experience Manager建立新網站。 「標準網站範本」由Adobe提供，做為起點。

## 必備條件 {#prerequisites}

本章中的步驟將作為Cloud Service環境在Adobe Experience Manager進行。 確保您擁有對環境的管AEM理訪問。 在完成本教學課程時，建議使用[沙盒程式](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html)和[開發環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html)。

請參閱[入門檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)以取得詳細資訊。

## 目標 {#objective}

1. 瞭解如何使用網站建立精靈來產生新網站。
1. 瞭解網站範本的角色。
1. 探索產生的AEM網站。

## 登入Adobe Experience Manager作者{#author}

首先，以Cloud Service環境AEM的身分登入。 環境AEM分割為&#x200B;**作者服務**&#x200B;和&#x200B;**發佈服務**。

* **作者服務** -建立、管理和更新網站內容的地方。通常只有內部使用者可存取&#x200B;**Author Service**，而且位於登入畫面後方。
* **發佈服務** -代管即時網站。這是一般使用者將看到的服務，通常是公開提供的。

大部份的教學課程都會使用&#x200B;**Author Service**&#x200B;進行。

1. 導覽至Adobe Experience Cloud[https://experience.adobe.com/](https://experience.adobe.com/)。 使用您的個人帳戶或公司／學校帳戶登入。
1. 確保在菜單中選擇了正確的「組織」 ，然後按一下&#x200B;**Experience Manager**。

   ![Experience Cloud首頁](assets/create-site/experience-cloud-home-screen.png)

1. 在「**Cloud Manager**」下方，按一下「**Launch**」。
1. 將滑鼠指標暫留在您要使用的程式上，然後按一下&#x200B;**Cloud Manager程式**&#x200B;圖示。

   ![Cloud Manager方案圖示](assets/create-site/cloud-manager-program-icon.png)

1. 在頂部菜單中，按一下&#x200B;**Environments**&#x200B;以查看已設定的環境。

1. 尋找您想要使用的環境，然後按一下「**作者URL**」。

   ![存取開發人員作者](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >建議在本教學課程中使用&#x200B;**Development**&#x200B;環境。

1. 新標籤將啟AEM動至&#x200B;**Author Service**。 按一下&#x200B;**使用Adobe登錄** ，您應使用相同的Experience Cloud憑據自動登錄。

1. 在重新導向並驗證後，您現在應該會看到AEM開始畫面。

   ![開始AEM畫面](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> 存取Experience Manager時遇到問題？ 檢閱[入門檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)

## 下載基本網站範本

「網站範本」提供新網站的起點。 網站範本包含一些基本主題、頁面範本、設定和範例內容。 「網站範本」中包含的內容，完全由開發人員決定。 Adobe提供&#x200B;**基本網站範本**&#x200B;以加速新實作。

1. 開啟新的瀏覽器標籤，並導覽至GitHub上的「基本網站範本」專案：[https://github.com/adobe/aem-site-template-basic](https://github.com/adobe/aem-site-template-basic)。 本專案是開放採購的，授權任何人使用。
1. 按一下「**發行**」並導航至[最新發行](https://github.com/adobe/aem-site-template-basic/releases/latest)。
1. 展開&#x200B;**Assets**&#x200B;下拉式清單並下載範本zip檔案：

   ![基本網站範本郵遞區號](assets/create-site/template-basic-zip-file.png)

   此zip檔案將用於下個練習。

   >[!NOTE]
   >
   > 本教學課程是使用「基本站點模板」的&#x200B;**5.0.0**&#x200B;版編寫的。 如果啟動新專案，建議您一律使用最新版本。

## 建立新網站

接著，使用上一練習中的「網站範本」產生新網站。

1. 返回環AEM境。 從「開AEM始」畫面導覽至&#x200B;**Sites**。
1. 在右上角按一下「建立&#x200B;**> >**&#x200B;網站（範本）**」。**&#x200B;這將顯示&#x200B;**建立站點嚮導**。
1. 在&#x200B;**選擇站點模板**&#x200B;下，按一下&#x200B;**導入**&#x200B;按鈕。

   上傳從上一練習下載的&#x200B;**.zip**&#x200B;範本檔案。

1. 選擇&#x200B;**基本AEM站點模板** ，然後按一下&#x200B;**Next**。

   ![選取網站範本](assets/create-site/select-site-template.png)

1. 在「**站點詳細資訊** > **站點標題**」下輸入`WKND Site`。
1. 在&#x200B;**站點名稱**&#x200B;下輸入`wknd`。

   ![網站範本詳細資訊](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > 如果使用共AEM用環境，請將唯一標識符附加到&#x200B;**站點名稱**。 例如`wknd-johndoe`。 如此可確保多位使用者都能完成相同的教學課程，而不會有任何衝突。

1. 按一下&#x200B;**建立**&#x200B;生成站點。 完成網站建立後，在&#x200B;**Success**&#x200B;對話方塊中按一下&#x200B;**Done&lt;a1/AEM>。**

## 探索新網站

1. 導覽至AEM Sites主控台（如果尚未到達）。
1. 已生成新的&#x200B;**WKND站點**。 它將包含具有多語言階層的網站結構。
1. 選擇頁面並按一下菜單欄中的&#x200B;**編輯**&#x200B;按鈕，開啟&#x200B;**英文** > **首頁**&#x200B;頁面：

   ![WKND網站階層](assets/create-site/wknd-site-starter-hierarchy.png)

1. 已建立入門內容，並有數個元件可供新增至頁面。 嘗試這些元件，以瞭解其功能。 您將在下一章中學習元件的基本知識。

   ![開始首頁](assets/create-site/start-home-page.png)

   *網站範本提供的範例內容*

## 恭喜！{#congratulations}

恭喜，您剛建立了第一個網AEM站！

### 後續步驟{#next-steps}

使用Adobe Experience Manager的「頁面」編AEM輯器，更新[「作者」內容和publish](author-content-publish.md)章節中網站的內容。 瞭解如何設定原子元件以更新內容。 瞭解AEM Author和Publish環境之間的差異，並瞭解如何將更新發佈至即時網站。
