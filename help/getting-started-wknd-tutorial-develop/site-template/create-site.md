---
title: 建立網站
seo-title: AEM Sites快速入門 — 建立網站
description: 使用AEM Adobe Experience Manager中的「網站建立精靈」來產生新網站。 由Adobe提供的「標準網站範本」會作為新網站的起點。
sub-product: Sites
version: Cloud Service
type: Tutorial
topic: 內容管理
feature: 核心元件、頁面編輯器
role: Developer
level: Beginner
kt: 7496
thumbnail: KT-7496.jpg
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '902'
ht-degree: 0%

---


# 建立網站 {#create-site}

>[!CAUTION]
>
> 此處的快速網站建立功能將於2021年下半年推出。 相關檔案可供預覽。

本章說明如何在Adobe Experience Manager中建立新網站。 標準網站範本由Adobe提供，可作為起點。

## 必備條件 {#prerequisites}

本章中的步驟將在Adobe Experience Manager作為Cloud Service環境中進行。 確保您擁有AEM環境的管理存取權。 完成本教學課程時，建議您使用[沙箱方案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html)和[開發環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html)。

如需詳細資訊，請參閱[入門檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)。

## 目標 {#objective}

1. 了解如何使用「站點建立嚮導」生成新站點。
1. 了解網站範本的角色。
1. 探索產生的AEM網站。

## 登入Adobe Experience Manager Author {#author}

首先，以Cloud Service環境的身分登入AEM。 AEM環境會分割為&#x200B;**Author Service**&#x200B;和&#x200B;**Publish Service**。

* **作者服務**  — 建立、管理及更新網站內容的位置。通常只有內部使用者可存取&#x200B;**作者服務**，且位於登入畫面後。
* **發佈服務**  — 托管即時網站。這項服務是一般使用者會看到，且通常可公開使用。

大部分的教學課程將使用&#x200B;**Author Service**&#x200B;進行。

1. 導覽至Adobe Experience Cloud [https://experience.adobe.com/](https://experience.adobe.com/)。 使用您的個人帳戶或公司/學校帳戶登入。
1. 確保在菜單中選擇了正確的組織，然後按一下&#x200B;**Experience Manager**。

   ![Experience Cloud首頁](assets/create-site/experience-cloud-home-screen.png)

1. 在&#x200B;**Cloud Manager**&#x200B;下，按一下&#x200B;**Launch**。
1. 將滑鼠指標暫留在您要使用的方案上，然後按一下&#x200B;**Cloud Manager方案**&#x200B;圖示。

   ![Cloud Manager方案圖示](assets/create-site/cloud-manager-program-icon.png)

1. 在頂端功能表中，按一下「**環境**」以檢視布建的環境。

1. 找到您要使用的環境，然後按一下&#x200B;**製作URL**。

   ![存取開發作者](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >建議在本教學課程中使用&#x200B;**Development**&#x200B;環境。

1. 新索引標籤將啟動至AEM **作者服務**。 按一下「**使用Adobe**&#x200B;登入」，您應使用相同的Experience Cloud憑證自動登入。

1. 重新導向與驗證後，您現在應該會看到AEM開始畫面。

   ![AEM開始畫面](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> 存取Experience Manager時是否有問題？ 檢閱[入門檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)

## 下載基本網站範本

網站範本提供新網站的起點。 「網站範本」包含一些基本主題、頁面範本、設定和範例內容。 開發人員能決定「網站範本」中包含的內容。 Adobe提供&#x200B;**基本網站範本**&#x200B;以加速新實作。

1. 開啟新的瀏覽器標籤，並導覽至GitHub上的「基本網站範本」專案：[https://github.com/adobe/aem-site-template-basic](https://github.com/adobe/aem-site-template-basic)。 該項目是開源的，並授權任何人使用。
1. 按一下「**發行版本**」並導覽至「[最新發行版本](https://github.com/adobe/aem-site-template-basic/releases/latest)」。
1. 展開&#x200B;**Assets**&#x200B;下拉式清單，然後下載範本zip檔案：

   ![基本網站範本Zip](assets/create-site/template-basic-zip-file.png)

   此zip檔案將用於下一個練習。

   >[!NOTE]
   >
   > 本教學課程是使用基本網站範本的&#x200B;**5.0.0**&#x200B;版本撰寫。 如果啟動新專案，一律建議使用最新版本。

## 建立新網站

接下來，使用上一練習中的「網站範本」生成新網站。

1. 返回AEM環境。 從「AEM開始」畫面導覽至&#x200B;**Sites**。
1. 在右上角按一下「**Create** > **Site(Template)**」。 這會顯示&#x200B;**建立網站精靈**。
1. 在「**選取網站範本**」下，按一下「**匯入**」按鈕。

   上傳從上次練習下載的&#x200B;**.zip**&#x200B;範本檔案。

1. 選擇&#x200B;**基本AEM站點模板**，然後按一下&#x200B;**下一步**。

   ![選擇網站模板](assets/create-site/select-site-template.png)

1. 在&#x200B;**Site Details** > **Site title**&#x200B;下輸入`WKND Site`。
1. 在&#x200B;**站點名稱**&#x200B;下，輸入`wknd`。

   ![網站範本詳細資訊](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > 如果使用共用AEM環境，請將唯一識別碼附加至&#x200B;**網站名稱**。 例如`wknd-johndoe`。 這可確保多位使用者能完成相同的教學課程，不會有任何衝突。

1. 按一下&#x200B;**建立**&#x200B;以生成站點。 當AEM完成網站建立後，按一下&#x200B;**Success**&#x200B;對話方塊中的&#x200B;**Done**。

## 探索新網站

1. 導覽至AEM Sites主控台（如果尚未存在）。
1. 已生成新的&#x200B;**WKND站點**。 其中將包含具有多語言階層的網站結構。
1. 通過選擇頁面並按一下菜單欄中的&#x200B;**編輯**&#x200B;按鈕，開啟&#x200B;**英文** > **首頁**&#x200B;頁：

   ![WKND網站階層](assets/create-site/wknd-site-starter-hierarchy.png)

1. 已建立入門內容，並可將多個元件新增至頁面。 試用這些元件，以了解功能。 您將在下一章中了解元件的基本知識。

   ![開始首頁](assets/create-site/start-home-page.png)

   *網站範本提供的範例內容*

## 恭喜！ {#congratulations}

恭喜，您剛建立了第一個AEM網站！

### 後續步驟 {#next-steps}

使用AEMAdobe Experience Manager中的頁面編輯器，更新[製作內容和發佈](author-content-publish.md)章節中網站的內容。 了解如何設定原子元件以更新內容。 了解AEM製作和發佈環境之間的差異，並了解如何將更新發佈至即時網站。
