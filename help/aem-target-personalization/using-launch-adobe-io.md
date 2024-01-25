---
title: 使用Experience Platform Launch和Adobe Developer整合Adobe Experience Manager與Adobe Target
description: 有關如何使用Experience Platform Launch和Adobe Developer將Adobe Experience Manager與Adobe Target整合的逐步逐步解說
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: b1d7ce04-0127-4539-a5e1-802d7b9427dd
duration: 714
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1021'
ht-degree: 1%

---

# 透過Adobe Developer Console使用Adobe Experience Platform Launch

## 先決條件

* [AEM作者和發佈執行個體](./implementation.md#set-up-aem) 分別在localhost連線埠4502和4503上執行
* **Experience Cloud**
   * 存取您的組織Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * 透過下列解決方案提供Experience Cloud
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe Developer Console](https://developer.adobe.com/console/)

     >[!NOTE]
     >您應具有在Launch中開發、核准、發佈、管理擴充功能及管理環境的許可權。 如果您因無法使用的使用者介面選項而無法完成其中任何步驟，請聯絡Experience Cloud管理員以請求存取許可權。 如需Launch許可權的詳細資訊， [請參閱檔案](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/user-permissions.html).

* **瀏覽器外掛程式**
   * Adobe Experience Cloud Debugger ([鉻黃](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob))
   * Launch與DTM交換器([鉻黃](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk))

## 相關使用者

針對此整合，必須涉及以下對象，而若要執行某些工作，您可能需要管理存取權。

* 開發人員
* AEM管理員
* Experience Cloud管理員

## 簡介

AEM提供與Experience Platform Launch的現成整合。 此整合可讓AEM管理員透過簡單易用的介面輕鬆設定Experience Platform Launch，因此在設定這兩個工具時可減少工作量和錯誤次數。 此外，只要將Adobe Target擴充功能新增至Experience Platform Launch，就能協助我們在AEM網頁上使用Adobe Target的所有功能。

在本節中，我們將介紹下列整合步驟：

* 啟動
   * 建立Launch屬性
   * 新增Target擴充功能
   * 建立資料元素
   * 建立頁面規則
   * 設定環境
   * 建置並發佈
* AEM
   * 建立Cloud Service
   * 建立

### 啟動

#### 建立Launch屬性

屬性是一個容器，當您將標籤部署至網站時，在其中裝入擴充功能、規則、資料元素和程式庫。

1. 導覽至您的組織 [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (`https://<yourcompany>.experiencecloud.adobe.com`)
2. 使用您的Adobe ID登入，並確認您隸屬於正確的組織。
3. 在解決方案切換器中，按一下 **Launch** 然後選取 **前往啟動** 按鈕。

   ![Experience Cloud — 啟動](assets/using-launch-adobe-io/exc-cloud-launch.png)

4. 確定您隸屬於正確的組織，然後繼續建立Launch屬性。
   ![Experience Cloud — 啟動](assets/using-launch-adobe-io/launch-create-property.png)

   *如需建立屬性的詳細資訊，請參閱 [建立屬性](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/companies-and-properties.html?lang=en#create-or-configure-a-property) 產品檔案內。*
5. 按一下 **新增屬性** 按鈕
6. 提供屬性的名稱(例如， *AEM Target教學*)
7. 網域請輸入 *localhost.com* 因為這是WKND示範網站執行所在的網域。 雖然&#39;*網域*「欄位」為必填欄位，則Launch屬性可用於所實施的任何網域。 此欄位的主要用途是在規則產生器中預先填入功能表選項。
8. 按一下 **儲存** 按鈕。

   ![Launch — 新屬性](assets/using-launch-adobe-io/exc-launch-property.png)

9. 開啟您剛建立的屬性，然後按一下「擴充功能」標籤。

#### 新增Target擴充功能

Adobe Target擴充功能支援將Target JavaScript SDK用於現代網路的使用者端實施。 `at.js`. 仍在使用Target舊版程式庫的客戶， `mbox.js`， [應升級至at.js](https://experienceleague.adobe.com/docs/target-dev/developer/client-side/at-js-implementation/upgrading-from-atjs-1x-to-atjs-20.html) 以使用Launch。

Target擴充功能包含兩個主要部分：

* 管理核心程式庫設定的擴充功能設定
* 規則動作，可執行下列操作：
   * 載入Target (at.js)
   * 新增引數至所有Mbox
   * 將引數新增至全域mbox
   * 引發全域mbox

1. 在 **擴充功能**，您會看到已針對Launch屬性安裝的擴充功能清單。 ([Experience Platform Launch核心擴充功能](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) 預設會安裝)
2. 按一下 **擴充功能目錄** 選項，並在篩選器中搜尋Target。
3. 選取最新版的Adobe Target at.js，然後按一下 **安裝** 選項。
   ![Launch — 新增屬性](assets/using-launch-adobe-io/launch-target-extension.png)

4. 按一下 **設定** 按鈕，而且您可以注意到已匯入Target帳戶憑證的設定視窗，以及此擴充功能的at.js版本。
   ![Target — 擴充功能設定](assets/using-launch-adobe-io/launch-target-extension-2.png)

   透過非同步Launch內嵌程式碼部署Target時，您應在頁面上於Launch內嵌程式碼之前對預先隱藏的程式碼片段進行硬式編碼，以便管理內容閃爍問題。 我們稍後將深入瞭解預先隱藏的Snipper。 您可以下載預先隱藏的程式碼片段 [此處](assets/using-launch-adobe-io/prehiding.js)

5. 按一下 **儲存** 若要將Target擴充功能新增至您的Launch屬性，您現在應該能夠看到Target擴充功能列於 **已安裝** 擴充功能清單。

6. 重複上述步驟以搜尋「Experience CloudID服務」擴充功能並加以安裝。
   ![擴充功能 — Experience CloudID服務](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### 設定環境

1. 按一下 **環境** 標籤中，您可以看到為您的網站屬性建立的環境清單。 依預設，我們針對開發、測試和生產各建立一個例項。

![資料元素 — 頁面名稱](assets/using-launch-adobe-io/launch-environment-setup.png)

#### 建置並發佈

1. 按一下 **發佈** 索引標籤進行標籤，接著建立程式庫以建置變更（資料元素、規則）並部署至開發環境。
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. 從開發發佈變更到測試環境。
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. 執行 **為測試環境建置選項**.
4. 建置完成後，請執行 **核准以發佈**，這會將您的變更從測試環境移至生產環境。
   ![暫存至生產環境](assets/using-launch-adobe-io/build-staging.png)
5. 最後，執行 **建置並發佈至生產環境** 將變更推送至生產環境的選項。
   ![建置並發佈至生產環境](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> 授予Adobe Developer整合功能可使用適當功能選取工作區的存取權 [角色，允許中央團隊僅在少數幾個工作區中進行API導向的變更](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html).

1. 使用來自Adobe Developer的憑證在AEM中建立IMS整合。 （01:12至03:55）
2. 在Experience Platform Launch中建立屬性。 (涵蓋 [以上](#create-launch-property))
3. 使用步驟1中的IMS整合來建立Experience Platform Launch整合，以匯入您的Launch屬性。
4. 在AEM中，使用瀏覽器設定將Experience Platform Launch整合對應至網站。 （05:28至06:14）
5. 手動驗證整合。 （06:15至06:33）
6. 使用Launch/DTM瀏覽器外掛程式。 （06:34至06:50）
7. 使用Adobe Experience Cloud Debugger瀏覽器外掛程式。 （06:51至07:22）

此時，您已成功整合 [使用Adobe Experience Platform Launch的AEM與Adobe Target](./using-aem-cloud-services.md#integrating-aem-target-options) 如選項1所詳述。

若要使用AEM體驗片段選件來強化您的個人化活動，請前往下一章，並使用舊版雲端服務整合AEM與Adobe Target。
