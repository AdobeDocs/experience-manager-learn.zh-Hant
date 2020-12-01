---
title: 使用Experience Platform Launch和Adobe I/O整合Adobe Experience Manager與Adobe Target
seo-title: 使用Experience Platform Launch和Adobe I/O整合Adobe Experience Manager與Adobe Target
description: 逐步說明如何使用Experience Platform Launch和Adobe I/O將Adobe Experience Manager與Adobe Target整合
seo-description: 逐步說明如何使用Experience Platform Launch和Adobe I/O將Adobe Experience Manager與Adobe Target整合
translation-type: tm+mt
source-git-commit: 1209064fd81238d4611369b8e5b517365fc302e3
workflow-type: tm+mt
source-wordcount: '1098'
ht-degree: 2%

---


# 透過Adobe I/O Console使用Adobe Experience Platform Launch

## 必備條件

* [AEM作者和發佈例](./implementation.md#set-up-aem) 項取消分別位於localhost埠4502和4503
* **Experience Cloud**
   * 存取您的組織Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud已布建下列解決方案
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/O Console](https://console.adobe.io)

      >[!NOTE]
      >您應擁有在Launch中開發、核准、發佈、管理擴充功能及管理環境的權限。 如果您因為使用者介面選項不適用而無法完成上述任何步驟，請連絡您的Experience Cloud管理員以要求存取權。 如需啟動權限的詳細資訊，請參閱說明檔案[。](https://docs.adobelaunch.com/administration/user-permissions)


* **瀏覽器外掛程式**
   * Adobe Experience Cloud除錯程式([Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj))
   * 啟動和DTM交換機([Chrome](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk))

## 相關使用者

若要進行此整合，您需要參與下列對象，而若要執行某些工作，您可能需要管理存取權。

* 開發人員
* AEM管理員
* Experience Cloud管理員

## 簡介

AEM提供與Experience Platform Launch的立即可用整合。 此整合可讓AEM管理員透過簡單易用的介面輕鬆設定Experience Platform Launch，進而降低在設定這兩種工具時的工作量和錯誤數。 而且，只要將Adobe Target擴充功能新增至Experience Platform Launch，就可協助我們在AEM網頁上使用Adobe Target的所有功能。

在本節中，我們將討論下列整合步驟：

* 啟動
   * 建立啟動屬性
   * 新增Target擴充功能
   * 建立資料元素
   * 建立頁面規則
   * 設定環境
   * 建立和發佈
* AEM
   * 建立雲端服務
   * 建立

### 啟動

#### 建立啟動屬性

屬性是一個容器，當您將標籤部署至網站時，會填入擴充功能、規則、資料元素和程式庫。

1. 導覽至您的組織[Adobe Experience Cloud](https://experiencecloud.adobe.com/)(<https://>`<yourcompany>`.experiencecloud.adobe.com)
2. 使用您的Adobe ID登入，並確定您所在的組織正確無誤。
3. 在解決方案切換器中，按一下「啟動」，然後選取「移至啟動」按鈕。********

   ![Experience Cloud —— 啟動](assets/using-launch-adobe-io/exc-cloud-launch.png)

4. 請確定您位於正確的組織中，然後繼續建立Launch屬性。
   ![Experience Cloud —— 啟動](assets/using-launch-adobe-io/launch-create-property.png)

   *有關建立屬性的詳細資訊，請 [參閱產](https://docs.adobelaunch.com/administration/companies-and-properties#create-a-property) 品文檔中的建立屬性。*
5. 按一下&#x200B;**New Property**&#x200B;按鈕
6. 提供屬性的名稱（例如&#x200B;*AEM Target Tutorial*）
7. 作為域，輸入&#x200B;*localhost.com*，因為這是運行WKND演示站點的域。 雖然「*Domain*」欄位是必填欄位，但Launch屬性將可用於實作網域的任何網域。 此欄位的主要用途是在規則產生器中預先填入功能表選項。
8. 按一下&#x200B;**保存**&#x200B;按鈕。

   ![啟動——新屬性](assets/using-launch-adobe-io/exc-launch-property.png)

9. 開啟您剛建立的屬性，然後按一下「延伸模組」標籤。

#### 新增Target擴充功能

Adobe Target擴充功能支援使用適用於現代網路的Target JavaScript SDK的用戶端實作，`at.js`。 仍使用Target舊程式庫`mbox.js`、[的客戶應升級至at.js](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/upgrading-from-atjs-1x-to-atjs-20.html)以使用Launch。

Target擴充功能包含兩個主要部分：

* 擴充功能設定，可管理核心程式庫設定
* 執行下列動作的規則動作：
   * 載入目標(at.js)
   * 將參數新增至所有mbox
   * 將參數新增至全域Mbox
   * Fire Global Mbox

1. 在&#x200B;**Extensions**&#x200B;下，您可以看到已為Launch屬性安裝的擴充功能清單。 （[Experience Platform Launch Core Extension](https://exchange.adobe.com/experiencecloud.details.100223.adobe-launch-core-extension.html)預設已安裝）
2. 按一下&#x200B;**擴充目錄**&#x200B;選項，然後在篩選器中搜尋Target。
3. 選取最新版本的Adobe Target at.js，然後按一下&#x200B;**Install**選項。
   ![啟動——新屬性](assets/using-launch-adobe-io/launch-target-extension.png)

4. 按一下&#x200B;**Configure**按鈕，您就會注意到設定視窗中已匯入您的Target帳戶憑證，以及此擴充功能的at.js版本。
   ![Target —— 擴充功能設定](assets/using-launch-adobe-io/launch-target-extension-2.png)

   透過非同步的Launch內嵌代碼部署Target時，您應在Launch內嵌代碼之前，先在頁面上硬式編寫預先隱藏的程式碼片段，以管理內容閃爍。 我們稍後會進一步瞭解預先隱藏的狙擊手。 您可以下載預先隱藏的程式碼片段[這裡](assets/using-launch-adobe-io/prehiding.js)

5. 按一下&#x200B;**Save**&#x200B;以完成將Target擴充功能新增至Launch屬性，您現在應該可以看到&#x200B;**Installed**&#x200B;擴充功能清單下所列的Target擴充功能。

6. 重複上述步驟，以搜尋「Experience Cloud ID服務」擴充功能並加以安裝。
   ![擴充功能- Experience Cloud ID服務](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### 設定環境

1. 按一下站點屬性的&#x200B;**環境**&#x200B;頁籤，您可以看到為站點屬性建立的環境清單。 依預設，我們為開發、接移和生產分別建立一個例項。

![資料元素——頁面名稱](assets/using-launch-adobe-io/launch-environment-setup.png)

#### 建立和發佈

1. 按一下您網站屬性的&#x200B;**Publishing**&#x200B;標籤，讓我們建立程式庫，並將變更（資料元素、規則）部署至開發環境。
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. 將您的變更從開發發佈至測試環境。
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. 運行&#x200B;**「為測試構建」選項**。
4. 建置完成後，請執行&#x200B;**「批准發佈」，將變更從測試環境移至生產環境。**
   ![測試至生產](assets/using-launch-adobe-io/build-staging.png)
5. 最後，執行&#x200B;**建立並發佈至生產**選項，將變更推送至生產。
   ![建立並發佈至生產](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> 授予Adobe I/O整合對具有適當[角色之選取工作區的存取權，讓中央團隊僅在少數幾個工作區](https://docs.adobe.com/content/help/en/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html)中進行API導向的變更。

1. 使用Adobe I/O的認證，在AEM中建立IMS整合。（01:12至03:55）
2. 在Experience Platform Launch中，建立屬性。 （涵蓋[above](#create-launch-property)）
3. 使用步驟1的IMS整合，建立Experience Platform Launch整合以匯入您的Launch屬性。
4. 在AEM中，使用瀏覽器設定將Experience Platform Launch整合對應至網站。 （05:28至06:14）
5. 手動驗證整合。 （06:15至06:33）
6. 使用Launch/DTM瀏覽器外掛程式。 （06:34至06:50）
7. 使用Adobe Experience Cloud Debugger瀏覽器外掛程式。 （06:51至07:22）

目前，您已使用Adobe Experience Platform Launch](./using-aem-cloud-services.md#integrating-aem-target-options)成功將[AEM與Adobe Target整合，如選項1所詳述。

若要使用AEM體驗片段選件來強化您的個人化活動，請讓您繼續下一章，並使用舊版雲端服務將AEM與Adobe Target整合。
