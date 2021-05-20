---
title: AEM Workflow[Part2]中的變數
seo-title: AEM Workflow[Part2]中的變數
description: 在aem工作流程中使用xml,json,arraylist,document類型的變數
seo-description: 在aem工作流程中使用xml,json,arraylist,document類型的變數
feature: 工作流程
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: 開發
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 0%

---

# AEM工作流程中JSON類型的變數

從AEM Forms 6.5開始，我們現在可以在AEM工作流程中建立JSON類型的變數。 如果您要根據JSON結構向AEM工作流程提交適用性Forms，或想要儲存表單資料模型叫用作業的結果，通常會建立JSON類型的變數。 下列影片會逐步帶您了解在AEM工作流程中建立和使用JSON類型變數所需的步驟

**若使用AEM Forms 6.5.0**

建立JSON類型的變數以擷取工作流程模型中提交的資料時，請勿將JSON結構描述與變數建立關聯。 這是因為當您提交以JSON結構描述為基礎的適用性表單時，提交的資料與JSON結構描述不相容。 JSON結構描述投訴資料會括在afData.afBoundData.data元素中。

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**若使用AEM Forms 6.5.1及更新版本**

您可以在工作流程模型中使用JSON類型的變數來對應結構。 然後，您可以使用架構瀏覽器將架構元素與工作流模型中的字串/數字變數對應

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

若要讓資產在您的系統上運作，請依照下列步驟操作：

* [使用套件管理器將資產下載並匯入AEM](assets/jsonandstringvariable.zip)
* [探索工作流](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) 模式，了解工作流程中使用的變數
* [設定電子郵件服務](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [開啟最適化表單](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* 填寫詳細資訊並提交表格
