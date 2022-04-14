---
title: 與 [!DNL ServiceNow]
description: 使用表單資料模型建立和顯示所有事件。
feature: Adaptive Forms
version: 6.4,6.5
kt: 9957
topic: Development
role: Developer
level: Intermediate
exl-id: 93a177b0-7852-44da-89cc-836d127be4e7
source-git-commit: 81a15fb0182760aaac8cb58cccbfe28de7323492
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 2%

---

# 將AEM Forms與 [!DNL ServiceNow]

在中建立和顯示事件 [!DNL ServiceNow] 在AEM Forms使用表單資料模型。

## 必備條件

* [!DNL ServiceNow] 帳戶。
* 熟悉 [建立資料源](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)
* 熟悉 [窗體資料模型](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)

## 示例資產

本條所提供的樣本資產包括：
* 雲服務配置
* 切換檔案以建立事件並獲取所有事件
* 基於Swagger檔案的表單資料模型
* 要建立和列出的自適應窗體 [!DNL ServiceNow] 事件

## 在伺服器上部署資產

* 下載 [示例資產](assets/service-now.zip)
* 將資產導入AEM到 [軟體包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* 編輯 [CreateIncident雲服務配置](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fcreateincident)匹配ServiceNow實例。
* 編輯 [GetAllIncides雲服務配置](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fgetallincidents) 與ServiceNow實例匹配

## Test整合

* [開啟自適應窗體](http://localhost:4502/content/dam/formsanddocuments/create-incident-in-service-now/jcr:content?wcmmode=disabled)
* 在說明和注釋欄位中輸入一些值，然後按一下建立事件按鈕
* 新建事件的事件ID應填充在文本欄位中，下表應列出所有事件。
