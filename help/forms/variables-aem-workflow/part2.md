---
title: AEM Workflow[Part2]中的變數
seo-title: AEM Workflow[Part2]中的變數
description: 在AEM工作流程中使用xml,json,arraylist,document類型的變數
seo-description: 在AEM工作流程中使用xml,json,arraylist,document類型的變數
feature: workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---

# AEM工作流程中JSON類型的變數

從AEM Forms 6.5開始，我們現在可以在AEM Workflow中建立JSON類型的變數。 通常，如果您要根據JSON結構描述提交最適化表單至AEM工作流程，或想要儲存表單資料模型叫用作業的結果，您會建立JSON類型的變數。 以下影片會逐步帶您瞭解在AEM工作流程中建立和使用JSON類型變數所需的步驟
>[!NOTE]

**如果使用AEM Forms 6.5.0**

當您建立JSON類型的變數以擷取工作流程模型中的已提交資料時，請勿將JSON結構描述與變數建立關聯。 這是因為當您送出以JSON結構描述為基礎的最適化表單時，提交的資料與JSON結構描述不相容。 JSON結構描述抱怨資料會封入afData.afBoundData.data元素中。

**如果使用AEM Forms 6.5.1和更新版本**

您可以在工作流程模型中使用類型為JSON的變數來對應結構描述。 然後，您可以使用架構瀏覽器將架構元素與工作流模型中的字串／數字變數映射

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)

**只有AEM Forms 6.5.1之後，才能下鑽架構元素並將架構元素對應至工作流程變數。**

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

若要讓資產在您的系統上運作，請遵循下列步驟：

* [使用套件管理員將資產下載並匯入AEM](assets/jsonandstringvariable.zip)
* [探索工作流程模型](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) ，以瞭解工作流程中使用的變數
* [設定電子郵件服務](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [開啟最適化表單](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* 填寫詳細資訊並提交表格
