---
title: 與 [!DNL ServiceNow]整合
description: 使用表單資料模型建立並顯示所有未預期事件。
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-9957
topic: Development
role: Developer
level: Intermediate
exl-id: 93a177b0-7852-44da-89cc-836d127be4e7
last-substantial-update: 2022-07-07T00:00:00Z
duration: 47
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 0%

---

# 將AEM Forms與[!DNL ServiceNow]整合

使用AEM Forms中的表單資料模型在[!DNL ServiceNow]中建立和顯示事件。

## 先決條件

* [!DNL ServiceNow]帳戶。
* 熟悉[建立資料來源](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)
* 熟悉[表單資料模型](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)

## Assets範例

本文提供的範例資產包括

* 雲端服務設定
* Swagger檔案以建立事件並擷取全部   事件
* 以Swagger檔案為基礎的表單資料模型
* 用於建立和列出[!DNL ServiceNow]個事件的最適化表單

## 在您的伺服器上部署資產

* 下載[範例資產](assets/service-now.zip)
* 使用[封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)將資產匯入AEM
* 用於此整合的swagger檔案位於crx存放庫中的```/conf/9957/settings/cloudconfigs/fdm```資料夾下
* 編輯[CreateIncident雲端服務設定](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fcreateincident)以符合您的ServiceNow執行個體。
* 編輯[GetAllIncidents雲端服務設定](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fgetallincidents)以符合您的ServiceNow執行個體。 您需要變更主機、使用者名稱和密碼，以符合您的ServiceNow執行個體認證。

## 存取ServiceNow執行個體認證

* 按一下您的使用者設定檔
  ![按一下使用者設定檔](assets/snow-1.png)

* 按一下管理執行處理密碼
* 執行個體詳細資訊如下所示
  ![執行個體詳細資料](assets/snow-3.png)

## 測試整合

* [開啟最適化表單](http://localhost:4502/content/dam/formsanddocuments/create-incident-in-service-now/jcr:content?wcmmode=disabled)
* 在說明與備註欄位中輸入一些值，然後按一下「建立事件」按鈕
* 新建立之事件的事件ID應填入文字欄位中，且下表應列出所有事件。
