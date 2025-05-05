---
title: 設定AEM Forms通訊API
description: 設定以OpenAPI為基礎的AEM Forms Communication API，以進行伺服器對伺服器驗證
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: b9c0b04b-6131-4752-b2f0-58e1fb5f40aa
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 2%

---

# 在AEM Forms as a Cloud Service上設定OpenAPI型AEM Forms通訊API

## 先決條件

* AEM Forms as a Cloud Service的最新執行個體。
* 已將所有必要的[產品設定檔新增至環境。](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis)

* 啟用產品設定檔的AEM API存取權，如下所示
  ![product_profile1](assets/product-profiles1.png)
  ![product_profile](assets/product-profiles.png)

## 建立Adobe Developer Console專案

使用您的Adobe ID登入[Adobe Developer Console](https://developer.adobe.com/console/)。
按一下適當的圖示以建立新專案
![新專案](assets/new-project.png)

為專案提供有意義的名稱，然後按一下「新增API」圖示
![新專案](assets/new-project2.png)

選取Experience Cloud
![新專案3](assets/new-project3.png)
選取AEM Forms Communications API並按下一步
![新專案4](assets/new-project4.png)

確定您已選取伺服器對伺服器驗證，然後按下一步
![新專案5](assets/new-project5.png)
選取設定檔，然後按一下「儲存已設定的API」按鈕以儲存您的設定
![新專案6](assets/new-project6.png)
按一下OAuth伺服器對伺服器
![新專案7](assets/new-project7.png)
複製使用者端ID、使用者端密碼和範圍
![新專案8](assets/new-project8.png)

## 設定AEM執行個體以啟用ADC專案通訊

如果您已有AEM Forms專案，[請依照這些指示](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis)啟用Adobe Developer Console專案的OAuth伺服器對伺服器認證ClientID與AEM執行個體通訊

如果您沒有AEM Forms專案，請依照本檔案建立[AEM Forms專案。](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/getting-started)，然後啟用Adobe Developer Console專案的OAuth伺服器對伺服器認證ClientID，以使用此檔案與AEM執行個體[通訊。](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis)


## 後續步驟

[產生存取權杖](./generate-access-token.md)
