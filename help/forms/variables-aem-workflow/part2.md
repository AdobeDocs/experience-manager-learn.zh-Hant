---
title: AEM工作流程中的變數[第2部分]
description: 在AEM工作流程中使用XML、JSON、ArrayList、檔案型別的變數
version: 6.5
topic: Development
feature: Adaptive Forms, Workflow
role: Developer
level: Beginner
exl-id: e7d3e0be-5194-47c2-a668-ce78e727986e
duration: 354
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 0%

---

# AEM Workflow中JSON型別的變數

從AEM Forms 6.5開始，我們現在可以在AEM Workflow中建立JSON型別的變數。 一般來說，如果您根據JSON結構描述提交調適型Forms至AEM Workflow，或想要儲存表單資料模型叫用作業的結果，您將建立JSON型別的變數。 以下影片將逐步說明在AEM工作流程中建立及使用JSON型別變數所需的步驟

**若使用AEM Forms 6.5.0**

建立JSON型別的變數來擷取工作流程模型中提交的資料時，請勿將JSON結構描述與變數建立關聯。 這是因為當您提交以JSON結構描述為基礎的最適化表單時，提交的資料不符合JSON結構描述。 JSON結構描述投訴資料包含在afData.afBoundData.data元素中。

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**如果使用AEM Forms 6.5.1及更高版本**

您可在工作流程模型中使用型別JSON的變數來對應結構描述。 然後，您可以使用結構瀏覽器將結構元素與工作流程模型中的字串/數字變數對應

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

若要讓資產在您的系統上正常運作，請遵循下列步驟：

* [使用封裝管理程式下載資產並將其匯入AEM](assets/jsonandstringvariable.zip)
* [探索工作流程模型](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) 以瞭解工作流程中使用的變數
* [設定電子郵件服務](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [開啟最適化表單](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* 填寫詳細資料並提交表單
