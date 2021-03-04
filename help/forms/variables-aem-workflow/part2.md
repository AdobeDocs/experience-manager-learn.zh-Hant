---
title: WorkflowAEM中的變數[Part2]
seo-title: WorkflowAEM中的變數[Part2]
description: 在AEM工作流程中使用xml,json,arraylist,document類型的變數
seo-description: 在AEM工作流程中使用xml,json,arraylist,document類型的變數
feature: 工作流程
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: 開發
role: 開發人員
level: 初學者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '300'
ht-degree: 1%

---

# 工作流程中JSON類型的變AEM數

從AEM Forms6.5開始，我們現在可以在「工作流程」中建立JSON類型的變AEM數。 通常，如果您是根據JSON結構描述將最適化Forms送出至工作流程，或想要儲存表單資料模型叫用作業的結果，AEM您會建立JSON類型的變數。 以下視訊會逐步帶您瞭解在工作流程中建立和使用JSON類型變數所需AEM的步驟

**如果使用AEM Forms6.5.0**

當您建立JSON類型的變數以擷取工作流程模型中的已提交資料時，請勿將JSON結構描述與變數建立關聯。 這是因為當您送出以JSON結構描述為基礎的最適化表單時，提交的資料與JSON結構描述不相容。 JSON結構描述抱怨資料會封入afData.afBoundData.data元素中。

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**如果使用AEM Forms6.5.1及更新版本**

您可以在工作流程模型中使用類型為JSON的變數來對應結構描述。 然後，您可以使用架構瀏覽器將架構元素與工作流模型中的字串／數字變數映射

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

若要讓資產在您的系統上運作，請遵循下列步驟：

* [使用套件管理器將資產下AEM載並匯入](assets/jsonandstringvariable.zip)
* [探索工作流](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) 程模型，瞭解工作流程中使用的變數
* [設定電子郵件服務](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [開啟最適化表單](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* 填寫詳細資訊並提交表格
