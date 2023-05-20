---
title: 在AEM Forms6.3和6.4中使用Salesforce配置DataSource
description: 利用表單資料模型將AEM Forms與Salesforce整合
feature: Adaptive Forms, Form Data Model
topics: integrations
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 7a4fd109-514a-41a8-a3fe-53c1de32cb6d
last-substantial-update: 2020-02-14T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '892'
ht-degree: 0%

---

# 在AEM Forms6.3和6.4中使用Salesforce配置DataSource{#configuring-datasource-with-salesforce-in-aem-forms-and}

## 必備條件 {#prerequisites}

在本文中，我們通過Salesforce建立資料源的過程

本教程的先決條件：

* 滾動到此頁底部，下載交換機檔案並保存硬碟。
* 啟用SSL的AEM Forms

   * [在6.3上啟用SSL的AEM正式文檔](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/ssl-by-default.html)
   * [在6.4上啟用SSL的AEM正式文檔](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/ssl-by-default.html)

* 您需要有Salesforce帳戶
* 您需要建立已連接的應用。 列出了用於建立應用的Salesforce官方文檔 [這裡](https://help.salesforce.com/articleView?id=connected_app_create.htm&amp;type=0)。
* 為應用提供適當的OAuth作用域（我已選擇所有可用的OAuth作用域以進行測試）
* 提供回叫URL。 我的回調URL是

   * 如果您使用 **AEM Forms6.3**，回調URL為https://gbedekar-w7-1:6443/etc/cloudservices/fdm/createlead.html。 在此URL createlead中是我的表單資料模型的名稱。

   * 如果您使用**AEM Forms6.4**，回叫URL為https://gbedekar-w7-:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html

在本示例中， gbedekar -w7-1:6443是我的伺服器的名稱以及正在運行AEM的埠。

建立連接的應用程式後， **用戶密鑰和密鑰**。 在AEM Forms建立資料源時需要這些。

現在，您已建立了已連接的應用，您需要為需要在salesforce中執行的操作建立一個swagger檔案。 示例交換器檔案作為可下載資產的一部分被包括。 此交換器檔案允許您在自適應表單提交時建立「Lead」對象。 請瀏覽此變換檔案。

下一步是在AEM Forms建立資料源。 請按照您的AEM Forms版本執行以下步驟

## AEM Forms6.3 {#aem-forms}

* 使用https協定登錄AEM Forms
* 鍵入https://導航至雲服務&lt;servername>:&lt;serverport> /etc/cloudservices.html，例如https://gbedekar-w7-1:6443/etc/cloudservices.html
* 向下滾動到「表單資料模型」。
* 按一下「顯示配置」。
* 按一下「+」添加新配置
* 選擇「Rest Full Service」。 為配置提供有意義的標題和名稱。 比如說，

   * 名稱：CreateLeadInSalesForce
   * 標題：CreateLeadInSalesForce

* 按一下「建立」

**在下一螢幕中**

* 選擇「檔案」作為交換源檔案的選項。 瀏覽到您先前下載的檔案
* 選擇身份驗證類型作為OAuth2.0
* 提供ClientID和Client Secret值
* OAuth Url為 —  **https://login.salesforce.com/services/oauth2/authorize**
* 刷新標籤URL - **https://na5.salesforce.com/services/oauth2/token**
* **訪問Toke URL - https://na5.salesforce.com/services/oauth2/token**
* 授權範圍：** api chatter_api完整id openid refresh_token可視化力量web**
* 驗證處理程式：授權持有者
* 按一下「連接到OAUTH」。如果一切正常，則不應看到任何錯誤

使用Salesforce建立表單資料模型後，您就可以使用剛剛建立的資料源建立表單資料整合。 用於建立表單資料整合的正式文檔是 [這裡](https://helpx.adobe.com/aem-forms/6-3/data-integration.html)。

確保將表單資料模型配置為包括POST服務以在SFDC中建立Lead對象。

您還必須為Lead對象配置讀寫服務。 請參閱本頁底部的螢幕截圖。

建立表單資料模型後，可以基於此模型建立自適應Forms，並使用表單資料模型提交方法在SFDC中建立Lead。

## AEM Forms6.4 {#aem-forms-1}

* 建立資料源

   * [導航到資料源](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/global)

   * 按一下「建立」按鈕
   * 提供一些有意義的值

      * 名稱：CreateLeadInSalesForce
      * 標題：CreateLeadInSalesForce
      * 服務類型：REST風格服務
   * 按一下「下一步」
   * Swagger源：檔案
   * 瀏覽並選擇您在上一步中下載的交換器檔案
   * 驗證類型：OAuth 2.0。指定以下值
   * 提供ClientID和Client Secret值
   * OAuth Url為 —  **https://login.salesforce.com/services/oauth2/authorize**
   * 刷新標籤URL - **https://na5.salesforce.com/services/oauth2/token**
   * 訪問令牌Ur **l - https://na5.salesforce.com/services/oauth2/token**
   * 授權範圍：** api chatter_api完整id openid refresh_token可視化力量web**
   * 驗證處理程式：授權持有者
   * 按一下「連接到OAuth」按鈕。 如果您看到任何錯誤，請查看上述步驟以確保準確輸入了所有資訊。


使用SalesForce建立資料源後，您就可以使用剛剛建立的資料源建立表單資料整合。 其文檔連結是 [這裡](https://helpx.adobe.com/experience-manager/6-4/forms/using/create-form-data-models.html)

確保將表單資料模型配置為包括POST服務以在SFDC中建立Lead對象。

您還必須為Lead對象配置讀寫服務。 請參閱本頁底部的螢幕截圖。

建立表單資料模型後，可以基於此模型建立自適應Forms，並使用表單資料模型提交方法在SFDC中建立Lead。

>[!NOTE]
>
>確保swagger檔案中的url與您的區域相對應。 例如，在北美建立帳戶時，示例swagger檔案中的url為&quot;na46.salesforce.com&quot;。 最簡單的方法是登錄到Salesforce帳戶並檢查url。

![sfdc1](assets/sfdc1.gif)

![sfdc2](assets/sfdc2.png)

[示例Swagger檔案](assets/swagger-sales-force-lead.json)
