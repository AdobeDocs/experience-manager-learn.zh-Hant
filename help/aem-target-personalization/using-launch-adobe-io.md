---
title: 利用Experience Platform Launch和Adobe I/O將Adobe Experience Manager與Adobe Target整合
seo-title: Integrating Adobe Experience Manager with Adobe Target using Experience Platform Launch and Adobe I/O
description: 逐步走進如何利用Experience Platform Launch和Adobe I/O將Adobe Experience Manager與Adobe Target融合
seo-description: Step by step walk-through on how to integrate Adobe Experience Manager with Adobe Target using Experience Platform Launch and Adobe I/O
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: b1d7ce04-0127-4539-a5e1-802d7b9427dd
source-git-commit: 4b47daf82e27f6bea4be30e3cdd132f497f4c609
workflow-type: tm+mt
source-wordcount: '1064'
ht-degree: 2%

---

# 通過Adobe I/O控制台使用Adobe Experience Platform Launch

## 必備條件

* [AEM作者和發佈實例](./implementation.md#set-up-aem) 分別運行在localhost埠4502和4503上
* **Experience Cloud**
   * 訪問您的組織Adobe Experience Cloud。 `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud配置了以下解決方案
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/O控制台](https://console.adobe.io)

      >[!NOTE]
      >您應有權在啟動中開發、批准、發佈、管理擴展和管理環境。 如果由於用戶介面選項不可用而無法完成這些步驟，請聯繫您的Experience Cloud管理員以請求訪問。 有關啟動權限的詳細資訊， [請參閱文檔](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/user-permissions.html)。


* **瀏覽器插件**
   * Adobe Experience Cloud調試器([鉻](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj))
   * 啟動和DTM交換機([鉻](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk))

## 涉及的用戶

對於此整合，需要參與以下受眾，要執行某些任務，您可能需要管理訪問權限。

* 開發人員
* 管AEM理
* Experience Cloud管理員

## 簡介

提AEM供與Experience Platform Launch的現成整合。 此整合使管AEM理員能夠通過易於使用的介面輕鬆配置Experience Platform Launch，從而減少了配置這兩種工具時的工作量和錯誤數量。 只要在Experience Platform Launch中添加Adobe Target分機，就能幫助我們在網頁上AEM使用Adobe Target的所有功能。

在本節中，我們將介紹以下整合步驟：

* 啟動
   * 建立啟動屬性
   * 添加目標擴展
   * 建立資料元素
   * 建立頁面規則
   * 安裝環境
   * 生成和發佈
* AEM
   * 建立Cloud Service
   * 建立

### 啟動

#### 建立啟動屬性

屬性是在將標籤部署到站點時用擴展、規則、資料元素和庫填充的容器。

1. 導航到您的組織 [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (`https://<yourcompany>.experiencecloud.adobe.com`)
2. 使用您的Adobe ID登錄，並確保您所在的組織正確。
3. 在解決方案切換器中，按一下 **啟動** ，然後選擇 **轉到啟動** 按鈕

   ![Experience Cloud — 啟動](assets/using-launch-adobe-io/exc-cloud-launch.png)

4. 確保您位於正確的組織中，然後繼續建立Launch屬性。
   ![Experience Cloud — 啟動](assets/using-launch-adobe-io/launch-create-property.png)

   *有關建立屬性的詳細資訊，請參見 [建立屬性](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/companies-and-properties.html?lang=en#create-or-configure-a-property) 中。*
5. 按一下 **新建屬性** 按鈕
6. 為屬性提供名稱(例如， *目AEM標教程*)
7. 作為域，輸入 *localhost.com* 因為這是運行WKND演示站點的域。 儘管「*域*&#39;欄位是必填項， Launch屬性將在實施該屬性的任何域上工作。 此欄位的主要用途是在規則生成器中預填充菜單選項。
8. 按一下 **保存** 按鈕

   ![啟動 — 新建屬性](assets/using-launch-adobe-io/exc-launch-property.png)

9. 開啟剛建立的屬性，然後按一下「擴展」頁籤。

#### 添加目標擴展

Adobe Target擴展支援使用Target JavaScript SDK在現代Web上的客戶端實現， `at.js`。 仍在使用目標舊庫的客戶， `mbox.js`。 [應升級到at.js](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/upgrading-from-atjs-1x-to-atjs-20.html) 以使用啟動。

目標擴展由兩個主要部分組成：

* 管理核心庫設定的擴展配置
* 執行以下操作的規則操作：
   * 載入目標(at.js)
   * 將參數添加到所有框
   * 將參數添加到全局框
   * 火災全球郵箱

1. 下 **擴展**，您可以看到已為Launch屬性安裝的擴展的清單。 ([Experience Platform Launch核心擴展](https://exchange.adobe.com/experiencecloud.details.100223.adobe-launch-core-extension.html) 預設安裝)
2. 按一下 **擴展目錄** 選項，並在篩選器中搜索目標。
3. 選擇Adobe Targetat.js的最新版本，然後按一下 **安裝** 的雙曲餘切值。
   ![啟動 — 新建屬性](assets/using-launch-adobe-io/launch-target-extension.png)

4. 按一下 **配置** 按鈕，您可以注意到已導入目標帳戶憑據的配置窗口以及此擴展的at.js版本。
   ![目標 — 擴展配置](assets/using-launch-adobe-io/launch-target-extension-2.png)

   當通過非同步Launch嵌入代碼部署Target時，應在Launch嵌入代碼之前在頁面上硬編碼預隱藏代碼段，以管理內容閃爍。 我們稍後會再瞭解那個預先隱藏的狙擊手。 您可以下載預隱藏的代碼段 [這裡](assets/using-launch-adobe-io/prehiding.js)

5. 按一下 **保存** 要完成將目標擴展添加到Launch屬性，您現在應能看到在 **已安裝** 的子菜單。

6. 重複上述步驟以搜索「Experience CloudID服務」擴展並安裝它。
   ![擴展 — Experience CloudID服務](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### 安裝環境

1. 按一下 **環境** 的子菜單。您可以看到為站點屬性建立的環境清單。 預設情況下，我們為開發、轉移和生產分別建立了一個實例。

![資料元素 — 頁名](assets/using-launch-adobe-io/launch-environment-setup.png)

#### 生成和發佈

1. 按一下 **發佈** 頁籤，然後建立庫以構建和將更改（資料元素、規則）部署到開發環境。
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. 將更改從「開發」發佈到「暫存」環境。
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. 運行 **「生成暫存」選項**。
4. 生成完成後，運行 **批准發佈**，將更改從暫存環境移到生產環境。
   ![暫存到生產](assets/using-launch-adobe-io/build-staging.png)
5. 最後，運行 **生成並發佈到生產** 按鈕。
   ![生成並發佈到生產](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> 授予Adobe I/O整合對選擇工作區的訪問權限，並使用相應的 [角色，允許中心團隊僅在幾個工作區中進行API驅動的更改](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html)。

1. 使用來自Adobe I/O的憑AEM據建立IMS整合。（01:12至03:55）
2. 在Experience Platform Launch中，建立屬性。 （覆蓋） [上](#create-launch-property))
3. 使用步驟1中的IMS整合，建立Experience Platform Launch整合以導入Launch屬性。
4. 在中AEM，使用瀏覽器配置將Experience Platform Launch整合映射到站點。 （05:28至06:14）
5. 手動驗證整合。 （06:15至06:33）
6. 使用Launch/DTM瀏覽器插件。 （06:34至06:50）
7. 使用Adobe Experience Cloud調試器瀏覽器插件。 （06:51至07:22）

此時，您已成功整合 [AEMAdobe Target用Adobe Experience Platform Launch](./using-aem-cloud-services.md#integrating-aem-target-options) 詳情載於備選案文1。

使用AEM體驗片段可支援個性化活動，讓您繼續下一章，並使用舊式雲服務AEM與Adobe Target整合。
