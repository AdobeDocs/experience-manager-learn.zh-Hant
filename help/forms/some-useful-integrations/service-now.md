---
title: 與整合 [!DNL ServiceNow]
description: 使用表單資料模型建立並顯示所有事件。
feature: Adaptive Forms
version: 6.4,6.5
kt: 9957
topic: Development
role: Developer
level: Intermediate
exl-id: 93a177b0-7852-44da-89cc-836d127be4e7
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '288'
ht-degree: 2%

---

# 將AEM Forms與 [!DNL ServiceNow]

在中建立和顯示事件 [!DNL ServiceNow] 使用AEM Forms中的表單資料模型。

## 必備條件

* [!DNL ServiceNow] 帳戶。
* 熟悉 [建立資料來源](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)
* 熟悉 [表單資料模型](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)

## 範例資產

隨本文提供的範例資產包括下列項目

* 雲端服務設定
* 交換檔案以建立事件並擷取所有事件
* 基於交換器檔案的表單資料模型
* 要建立和列出的適用性表單 [!DNL ServiceNow] 事件

## 在伺服器上部署資產

* 下載 [範例資產](assets/service-now.zip)
* 使用將資產匯入AEM [封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)
* 用於此整合的swagger檔案位於 ```/conf/9957/settings/cloudconfigs/fdm``` crx儲存庫中的資料夾
* 編輯 [CreateIncident雲端服務設定](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fcreateincident)以匹配您的ServiceNow實例。
* 編輯 [GetAllEccints雲端服務設定](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fgetallincidents) 以匹配您的ServiceNow實例。 您需要更改主機、用戶名和密碼，以匹配您的ServiceNow實例憑據。

## 訪問ServiceNow實例憑據

* 按一下您的使用者設定檔
   ![按一下使用者設定檔](assets/snow-1.png)

* 按一下管理實例密碼
* 執行個體詳細資訊如下所示
   ![執行個體詳細資訊](assets/snow-3.png)

## 測試整合

* [開啟最適化表單](http://localhost:4502/content/dam/formsanddocuments/create-incident-in-service-now/jcr:content?wcmmode=disabled)
* 在「說明和注釋」欄位中輸入一些值，然後按一下「建立事件」按鈕
* 新建立事件的事件ID應填入文字欄位中，下表應列出所有事件。
