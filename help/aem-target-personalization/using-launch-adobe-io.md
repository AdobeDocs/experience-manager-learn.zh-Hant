---
title: 使用Experience Platform Launch和Adobe I/O整合Adobe Experience Manager與Adobe Target
seo-title: 使用Experience Platform Launch和Adobe I/O整合Adobe Experience Manager與Adobe Target
description: 逐步逐步說明如何使用Experience Platform Launch和Adobe I/O將Adobe Experience Manager與Adobe Target整合
seo-description: 逐步逐步說明如何使用Experience Platform Launch和Adobe I/O將Adobe Experience Manager與Adobe Target整合
feature: 體驗片段
topic: 個性化
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1101'
ht-degree: 2%

---


# 透過Analytics Console使用Adobe Experience Platform Launch

## 必備條件

* [AEM製作和](./implementation.md#set-up-aem) 發佈localhost埠4502和4503上的instancerunning
* **Experience Cloud**
   * 存取您的組織Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud已布建以下解決方案
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/O主控台](https://console.adobe.io)

      >[!NOTE]
      >您應該有在Launch中開發、核准、發佈、管理擴充功能及管理環境的權限。 如果您因無法使用的使用者介面選項而無法完成其中任何步驟，請聯絡您的Experience Cloud管理員以要求存取權。 如需Launch權限的詳細資訊，請[參閱本檔案](https://docs.adobelaunch.com/administration/user-permissions)。


* **瀏覽器外掛程式**
   * Adobe Experience Cloud偵錯器([Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj))
   * Launch和DTM開關([Chrome](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk))

## 相關使用者

若要進行這項整合，需要涉及下列對象，而若要執行某些工作，您可能需要管理存取權。

* 開發人員
* AEM管理
* Experience Cloud管理員

## 簡介

AEM提供立即可用的與Experience Platform Launch整合。 此整合可讓AEM管理員透過簡單易用的介面輕鬆設定Experience Platform Launch，借此降低在設定這兩種工具時的工作量和錯誤數量。 只要將Adobe Target擴充功能新增至Experience Platform Launch，就能協助我們使用AEM網頁上Adobe Target的所有功能。

在本節中，我們將說明下列整合步驟：

* 啟動
   * 建立Launch屬性
   * 新增Target擴充功能
   * 建立資料元素
   * 建立頁面規則
   * 設定環境
   * 建置和發佈
* AEM
   * 建立Cloud Service
   * 建立

### 啟動

#### 建立Launch屬性

屬性是一個容器，當您將標籤部署至網站時，在其中裝入擴充功能、規則、資料元素和程式庫。

1. 導覽至您的組織[Adobe Experience Cloud](https://experiencecloud.adobe.com/)(<https://>`<yourcompany>`.experiencecloud.adobe.com)
2. 使用Adobe ID登入，並確定您所在的組織正確無誤。
3. 在解決方案切換器中，按一下&#x200B;**Launch**，然後選取&#x200B;**Go To Launch**&#x200B;按鈕。

   ![Experience Cloud — 啟動](assets/using-launch-adobe-io/exc-cloud-launch.png)

4. 請確定您位在正確的組織中，然後繼續建立Launch屬性。
   ![Experience Cloud — 啟動](assets/using-launch-adobe-io/launch-create-property.png)

   *如需建立屬性的詳細資訊，請 [參閱產](https://docs.adobelaunch.com/administration/companies-and-properties#create-a-property) 品檔案中的建立屬性。*
5. 按一下&#x200B;**新屬性**&#x200B;按鈕
6. 提供屬性的名稱(例如&#x200B;*AEM Target教學課程*)
7. 作為域，請輸入&#x200B;*localhost.com*，因為這是運行WKND演示站點的域。 雖然「*Domain*」欄位為必要欄位，但Launch屬性可用於實作它的任何網域。 此欄位的主要用途是預先填入規則產生器中的功能表選項。
8. 按一下&#x200B;**Save**&#x200B;按鈕。

   ![Launch — 新屬性](assets/using-launch-adobe-io/exc-launch-property.png)

9. 開啟您剛建立的屬性，然後按一下「擴充功能」標籤。

#### 新增Target擴充功能

Adobe Target擴充功能支援將Target JavaScript SDK用於現代網路`at.js`的用戶端實作。 仍在使用Target舊程式庫`mbox.js`、[的客戶應升級至at.js](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/upgrading-from-atjs-1x-to-atjs-20.html)以使用Launch。

Target擴充功能包含兩個主要部分：

* 管理核心程式庫設定的擴充功能組態
* 執行下列動作的規則動作：
   * 載入Target(at.js)
   * 將參數新增至所有mbox
   * 將參數新增至全域mbox
   * 引發全域mbox

1. 在&#x200B;**擴充功能**&#x200B;下，您可以看到已針對Launch屬性安裝的擴充功能清單。 ([Experience Platform Launch核心擴充功能](https://exchange.adobe.com/experiencecloud.details.100223.adobe-launch-core-extension.html)預設安裝)
2. 按一下&#x200B;**擴充功能目錄**&#x200B;選項，然後在篩選器中搜尋Target。
3. 選取最新版本的Adobe Target at.js ，然後按一下&#x200B;**Install**選項。
   ![Launch — 新屬性](assets/using-launch-adobe-io/launch-target-extension.png)

4. 按一下&#x200B;**設定**按鈕，您就會注意到設定視窗中已匯入您的Target帳戶憑證，以及此擴充功能的at.js版本。
   ![Target — 擴充功能設定](assets/using-launch-adobe-io/launch-target-extension-2.png)

   透過非同步Launch內嵌程式碼部署Target時，您應在Launch內嵌程式碼之前，在頁面上以硬式編碼撰寫預先隱藏的程式碼片段，以管理內容忽隱忽現的情形。 我們稍後會進一步了解預先隱藏的Snipper。 您可以在此處[下載預先隱藏的程式碼片段](assets/using-launch-adobe-io/prehiding.js)

5. 按一下&#x200B;**儲存**&#x200B;以完成將Target擴充功能新增至您的Launch屬性，現在應該能看到&#x200B;**Installed**&#x200B;擴充功能清單下方列出的Target擴充功能。

6. 重複上述步驟，搜尋「Experience CloudID服務」擴充功能並加以安裝。
   ![擴充功能 — Experience CloudID服務](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### 設定環境

1. 按一下網站屬性的&#x200B;**Environment**&#x200B;標籤，您就會看到為網站屬性建立的環境清單。 依預設，我們分別為開發、測試和生產建立一個例項。

![資料元素 — 頁面名稱](assets/using-launch-adobe-io/launch-environment-setup.png)

#### 建置和發佈

1. 按一下網站屬性的&#x200B;**Publishing**&#x200B;標籤，接著我們建立程式庫，以建置變更，並將變更（資料元素、規則）部署至開發環境。
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. 將變更從開發發佈至測試環境。
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. 執行&#x200B;**為測試環境建置選項**。
4. 建置完成後，執行&#x200B;**核准以發佈**，將變更從測試環境移至生產環境。
   ![測試至生產](assets/using-launch-adobe-io/build-staging.png)
5. 最後，執行&#x200B;**建置並發佈至生產環境**選項，將變更推送至生產環境。
   ![建置並發佈到生產環境](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> 授予Adobe I/O整合功能可使用適當[角色選取工作區的存取權，以允許中央團隊僅在幾個工作區](https://docs.adobe.com/content/help/en/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html)中進行API導向的變更。

1. 使用Adobe I/O的憑證在AEM中建立IMS整合。（01:12至03:55）
2. 在Experience Platform Launch中，建立屬性。 （覆蓋[以上](#create-launch-property)）
3. 使用步驟1提供的IMS整合，建立Experience Platform Launch整合以匯入您的Launch屬性。
4. 在AEM中，使用瀏覽器設定將Experience Platform Launch整合對應至網站。 (05:28 - 06:14)
5. 手動驗證整合。 (06:15 - 06:33)
6. 使用Launch/DTM瀏覽器外掛程式。 (06:34 - 06:50)
7. 使用Adobe Experience Cloud Debugger瀏覽器外掛程式。 (06:51 - 07:22)

此時，您已使用Adobe Experience Platform Launch](./using-aem-cloud-services.md#integrating-aem-target-options)成功將[AEM與Adobe Target整合，如選項1所述。

如需使用AEM體驗片段選件來支援您的個人化活動，請讓您繼續進行下一章節，並使用舊版雲端服務整合AEM與Adobe Target。
