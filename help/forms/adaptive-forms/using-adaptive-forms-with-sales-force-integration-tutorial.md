---
title: 在AEM Forms6.3和6.4中使用Salesforce設定DataSource
seo-title: 在AEM Forms6.3和6.4中使用Salesforce設定DataSource
description: 使用表單資料模型整合AEM Forms與Salesforce
seo-description: 使用表單資料模型整合AEM Forms與Salesforce
uuid: 0124526d-f1a3-4f57-b090-a418a595632e
feature: Adaptive Forms, Form Data Model
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 8e314fc3-62d0-4c42-b1ff-49ee34255e83
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '936'
ht-degree: 0%

---


# 在AEM Forms6.3和6.4{#configuring-datasource-with-salesforce-in-aem-forms-and}中使用Salesforce設定DataSource

## 必備條件 {#prerequisites}

在本文中，我們將逐步介紹使用Salesforce建立資料來源的程式

本教學課程的先決條件：

* 捲動至本頁底部，下載Swagger檔案並儲存您的硬碟。
* AEM Forms啟用SSL

   * [在6.3上啟用SSL的AEM正式檔案](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/ssl-by-default.html)
   * [在6.4上啟用SSL的AEM正式檔案](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/ssl-by-default.html)

* 您需要有Salesforce帳戶
* 您需要建立連線的應用程式。 建立應用程式的Salesforce官方檔案列於[這裡](https://help.salesforce.com/articleView?id=connected_app_create.htm&amp;type=0)。
* 為應用程式提供適當的OAuth示波器（我已選取所有可用的OAuth示波器以進行測試）
* 提供回呼URL。 我的回呼URL是

   * 如果您使用&#x200B;**AEM Forms6.3**，回呼URL將為https://gbedekar-w7-1:6443/etc/cloudservices/fdm/createlead.html。 在此URL中，createlead是我的表單資料模型名稱。

   * 如果您使用**AEM Forms6.4**，回呼URL將為[https://gbedekar-w7-:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html](https://gbedekar-w7-1:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html)

在此示例中，gbedekar -w7-1:6443是我的伺服器的名稱以及運行的端AEM口。

建立連線應用程式後，請注意&#x200B;**消費者金鑰和密碼金鑰**。 在AEM Forms建立資料源時，您需要這些。

現在您已建立連線的應用程式，您就需要針對您在salesforce中執行的作業建立Swagger檔案。 範例Swagger檔案會隨附於可下載資產中。 此Swagger檔案允許您在Adaptive Form提交時建立「Lead」對象。 請探索此Swagger檔案。

下一步是在AEM Forms建立資料來源。 請依照您的AEM Forms版，依照下列步驟執行

## AEM Forms6.3 {#aem-forms}

* 使用https通訊協定登入AEM Forms
* 輸入https://&lt;servername>:&lt;serverport> /etc/cloudservices.html，以導覽至雲端服務，例如https://gbedekar-w7-1:6443/etc/cloudservices.html
* 向下捲動至「表單資料模型」。
* 按一下「顯示配置」。
* 按一下「+」以新增設定
* 選擇「Rest Full Service」。 為設定提供有意義的標題和名稱。 例如，

   * 名稱：CreateLeadInSalesForce
   * 標題：CreateLeadInSalesForce

* 按一下「建立」

**在下一個畫面中**

* 選擇「檔案」作為Swagger來源檔案的選項。 瀏覽至先前下載的檔案
* 選擇「驗證類型」作為OAuth2.0
* 提供ClientID和Client Secret值
* OAuth Url為- **https://login.salesforce.com/services/oauth2/authorize**
* 重新整理Token Url - **https://na5.salesforce.com/services/oauth2/token**
* **存取Toke Url - https://na5.salesforce.com/services/oauth2/token**
* 授權範圍：** api   chatter_api完整ID   openid   refresh_token視覺化Force網路**
* 驗證處理常式：授權持有者
* 按一下「連線至OAUTH」。如果一切順利，您不應看到任何錯誤

使用Salesforce建立表單資料模型後，您就可以使用剛建立的資料來源建立表單資料整合。 建立表單資料整合的官方檔案為[此處](https://helpx.adobe.com/aem-forms/6-3/data-integration.html)。

請務必將表單資料模型配置為包括POST服務，以便在SFDC中建立Lead對象。

您還必須為Lead對象配置讀寫服務。 請參閱本頁底部的螢幕擷取畫面。

在建立表單資料模型後，可以基於此模型建立自適應Forms，並使用表單資料模型提交方法在SFDC中建立銷售線索。

## AEM Forms6.4 {#aem-forms-1}

* 建立資料來源

   * [導覽至Data Sources](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/global)

   * 按一下「建立」按鈕
   * 提供一些有意義的值

      * 名稱：CreateLeadInSalesForce
      * 標題：CreateLeadInSalesForce
      * 服務類型：REST風格的服務
   * 按「下一步」
   * Swagger來源：檔案
   * 瀏覽並選取您在上一步驟中下載的Swagger檔案
   * 驗證類型：OAuth 2.0.指定下列值
   * 提供ClientID和Client Secret值
   * OAuth Url為- **https://login.salesforce.com/services/oauth2/authorize**
   * 重新整理Token Url - **https://na5.salesforce.com/services/oauth2/token**
   * 存取Token Ur **l - https://na5.salesforce.com/services/oauth2/token**
   * 授權範圍：** api chatter_api完整ID openid refresh_token視覺化Force web**
   * 驗證處理常式：授權持有者
   * 按一下「連線至OAuth」按鈕。 如果您發現任何錯誤，請檢閱上述步驟，以確保正確輸入所有資訊。


使用SalesForce建立資料來源後，您就可以使用剛建立的資料來源建立表單資料整合。 其文檔連結為[here](https://helpx.adobe.com/experience-manager/6-4/forms/using/create-form-data-models.html)

請務必將表單資料模型配置為包括POST服務，以便在SFDC中建立Lead對象。

您還必須為Lead對象配置讀寫服務。 請參閱本頁底部的螢幕擷取畫面。

在建立表單資料模型後，可以基於此模型建立自適應Forms，並使用表單資料模型提交方法在SFDC中建立銷售線索。

>[!NOTE]
>
>請確定swagger檔案中的URL與您的地區相符。 例如，範例Swagger檔案中的URL是「na46.salesforce.com」，因為帳戶是在北美地區建立的。 最簡單的方式是登入您的Salesforce帳戶並檢查URL。

![sfdc1](assets/sfdc1.gif)

![sfdc2](assets/sfdc2.png)

[SampleSwaggerFile](assets/swagger-sales-force-lead.json)
