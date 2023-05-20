---
title: Workflow[AEMPart2]中的變數
description: 在工作流中使用XML、JSON、ArrayList和Document類型的變AEM量
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: e7d3e0be-5194-47c2-a668-ce78e727986e
source-git-commit: 4b47daf82e27f6bea4be30e3cdd132f497f4c609
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 0%

---

# 工作流中JSON類型的變AEM量

從AEM Forms6.5開始，現在可以在工作流中建立JSON類型的AEM變數。 通常，如果要將基於JSON架構的自適應Forms提交到工作流，或要儲存表單資料模型調用操作的結果，則將建立JSON類型的變數。 以下視頻將引導您完成在工作流中建立和使用JSON類型的變數所需AEM的步驟

**如果使用AEM Forms6.5.0**

建立JSON類型的變數以捕獲工作流模型中提交的資料時，請不要將JSON架構與變數關聯。 這是因為當您提交基於JSON架構的自適應表單時，提交的資料與JSON架構不相容。 JSON架構投訴資料包含在afData.afBoundData.data元素中。

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**如果使用AEM Forms6.5.1及更高版本**

可以在工作流模型中使用JSON類型的變數映射架構。 然後，可以使用架構瀏覽器將架構元素與工作流模型中的字串/數字變數映射

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

要使系統上的資產正常工作，請執行以下步驟：

* [使用包管理器將資產下載AEM並導入到](assets/jsonandstringvariable.zip)
* [瀏覽工作流模型](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) 瞭解工作流中使用的變數
* [配置電子郵件服務](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [開啟自適應窗體](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* 填寫詳細資訊並提交表單
