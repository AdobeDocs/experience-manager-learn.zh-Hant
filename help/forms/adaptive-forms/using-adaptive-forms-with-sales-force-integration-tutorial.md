---
title: 在AEM Forms 6.3和6.4中使用Salesforce設定DataSource
seo-title: 在AEM Forms 6.3和6.4中使用Salesforce設定DataSource
description: 使用表單資料模型整合AEM Forms與Salesforce
seo-description: 使用表單資料模型整合AEM Forms與Salesforce
uuid: 0124526d-f1a3-4f57-b090-a418a595632e
feature: 適用性Forms，表單資料模型
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 8e314fc3-62d0-4c42-b1ff-49ee34255e83
topic: 開發
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '934'
ht-degree: 0%

---


# 在AEM Forms 6.3和6.4{#configuring-datasource-with-salesforce-in-aem-forms-and}中使用Salesforce設定DataSource

## 必備條件 {#prerequisites}

在本文中，我們將逐步介紹使用Salesforce建立資料源的過程

本教學課程的必要條件：

* 捲動至此頁面底部，下載Swagger檔案並儲存硬碟。
* 啟用SSL的AEM Forms

   * [在AEM 6.3上啟用SSL的官方檔案](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/ssl-by-default.html)
   * [在AEM 6.4上啟用SSL的官方檔案](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/ssl-by-default.html)

* 您需要有Salesforce帳戶
* 您需要建立已連線的應用程式。 建立應用程式的Salesforce官方檔案列於[此處](https://help.salesforce.com/articleView?id=connected_app_create.htm&amp;type=0)。
* 為應用程式提供適當的OAuth作用域（我已選擇所有可用的OAuth作用域以進行測試）
* 提供回呼URL。 我的案例中為

   * 如果您使用&#x200B;**AEM Forms 6.3**，回呼URL將為https://gbedekar-w7-1:6443/etc/cloudservices/fdm/createlead.html。 在此URL中，建立引導是我的表單資料模型的名稱。

   * 如果您使用** AEM Forms 6.4**，回呼URL將為[https://gbedekar-w7-:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html](https://gbedekar-w7-1:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html)

在此示例中， gbedekar -w7-1:6443是我的伺服器的名稱以及運行AEM的埠。

建立連線應用程式後，請記下&#x200B;**使用者金鑰和機密金鑰**。 在AEM Forms中建立資料來源時，您需要這些項目。

現在您已建立連線的應用程式，然後需要為您需要在salesforce中執行的操作建立交換器檔案。 可下載資產中包含範例Swagger檔案。 此Swagger檔案可讓您在提交適用性表單時建立「銷售機會」物件。 請瀏覽此swagger檔案。

下一步是在AEM Forms中建立「資料來源」。 請根據您的AEM Forms版本，依照下列步驟執行

## AEM Forms 6.3 {#aem-forms}

* 使用https通訊協定登入AEM Forms
* 輸入https://&lt;servername>:&lt;serverport> /etc/cloudservices.html以導覽至雲端服務，例如https://gbedekar-w7-1:6443/etc/cloudservices.html
* 向下捲動至「表單資料模型」。
* 按一下「顯示配置」。
* 按一下「+」以新增配置
* 選擇「Rest Full Service」。 為設定提供有意義的標題和名稱。 例如，

   * 名稱：CreateLeadInSalesForce
   * 標題：CreateLeadInSalesForce

* 按一下「建立」

**在下一個畫面**

* 選擇「檔案」作為交換源檔案的選項。 瀏覽至您先前下載的檔案
* 選取驗證類型作為OAuth2.0
* 提供ClientID和用戶端密碼值
* OAuth Url是 — **https://login.salesforce.com/services/oauth2/authorize**
* 重新整理代號Url - **https://na5.salesforce.com/services/oauth2/token**
* **存取權限Url - https://na5.salesforce.com/services/oauth2/token**
* 授權範圍：** api   chatter_api完整id   openid   refresh_token visualforce web**
* 驗證處理程式：授權承載
* 按一下「連線至OAUTH」。如果一切順利，您不應看到任何錯誤

使用Salesforce建立表單資料模型後，您就可以使用您剛建立的資料來源建立表單資料整合。 建立表單資料整合的官方檔案為[此處](https://helpx.adobe.com/aem-forms/6-3/data-integration.html)。

請務必配置表單資料模型以包括POST服務以在SFDC中建立銷售機會對象。

您還必須為Lead對象配置讀寫服務。 請參閱本頁底部的螢幕截圖。

建立表單資料模型後，您可以根據此模型建立最適化Forms，並使用表單資料模型提交方法在SFDC中建立銷售機會。

## AEM Forms 6.4 {#aem-forms-1}

* 建立資料來源

   * [導覽至資料來源](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/global)

   * 按一下「建立」按鈕
   * 提供一些有意義的值

      * 名稱：CreateLeadInSalesForce
      * 標題：CreateLeadInSalesForce
      * 服務類型：RESTful服務
   * 按「下一步」
   * Swagger源：檔案
   * 瀏覽並選取您在上一步驟中下載的Swagger檔案
   * 驗證類型：OAuth 2.0。指定下列值
   * 提供ClientID和用戶端密碼值
   * OAuth Url是 — **https://login.salesforce.com/services/oauth2/authorize**
   * 重新整理代號Url - **https://na5.salesforce.com/services/oauth2/token**
   * 存取權杖Ur **l - https://na5.salesforce.com/services/oauth2/token**
   * 授權範圍：** api chatter_api完整id openid refresh_token visualforce web**
   * 驗證處理程式：授權承載
   * 按一下「連線至OAuth」按鈕。 如果您發現任何錯誤，請查看上述步驟，以確保正確輸入所有資訊。


使用SalesForce建立資料源後，您就可以使用剛建立的資料源建立表單資料整合。 其檔案連結為[here](https://helpx.adobe.com/experience-manager/6-4/forms/using/create-form-data-models.html)

請務必配置表單資料模型以包括POST服務以在SFDC中建立銷售機會對象。

您還必須為Lead對象配置讀寫服務。 請參閱本頁底部的螢幕截圖。

建立表單資料模型後，您可以根據此模型建立最適化Forms，並使用表單資料模型提交方法在SFDC中建立銷售機會。

>[!NOTE]
>
>請確定swagger檔案中的url與您的地區對應。 例如，範例Swagger檔案中的url是「na46.salesforce.com」，因為帳戶是在北美建立。 最簡單的方式是登入您的Salesforce帳戶並檢查url 。

![sfdc1](assets/sfdc1.gif)

![sfdc2](assets/sfdc2.png)

[SampleSwaggerFile](assets/swagger-sales-force-lead.json)
