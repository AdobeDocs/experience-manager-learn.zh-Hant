---
title: 在AEM Forms工作流程中設定Json資料元素的值
seo-title: 在AEM Forms工作流程中設定Json資料元素的值
description: 當「最適化表單」在AEM Workflow中路由給不同的使用者時，將會要求根據審閱表單的人員來隱藏或停用某些欄位或面板。 為了滿足這些使用案例，我們通常會設定隱藏欄位的值。 根據此隱藏欄位的值商業規則，可編寫以隱藏／停用適當的面板或欄位。
seo-description: 當「最適化表單」在AEM Workflow中路由給不同的使用者時，將會要求根據審閱表單的人員來隱藏或停用某些欄位或面板。 為了滿足這些使用案例，我們通常會設定隱藏欄位的值。 根據此隱藏欄位的值商業規則，可編寫以隱藏／停用適當的面板或欄位。
uuid: a4ea6aef-a799-49e5-9682-3fa3b7a442fb
feature: adaptive-forms,workflow
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.4
discoiquuid: 548fb2ec-cfcf-4fe2-a02a-14f267618d68
translation-type: tm+mt
source-git-commit: f07680e73316efb859a675f4b2212d8c3e03f6a0
workflow-type: tm+mt
source-wordcount: '762'
ht-degree: 0%

---


# 在AEM Forms工作流程中設定JSON資料元素的值 {#setting-value-of-json-data-element-in-aem-forms-workflow}

當「最適化表單」在AEM Workflow中路由給不同的使用者時，將會要求根據審閱表單的人員來隱藏或停用某些欄位或面板。 為了滿足這些使用案例，我們通常會設定隱藏欄位的值。 根據此隱藏欄位的值商業規則，可編寫以隱藏／停用適當的面板或欄位。

![設定json資料中的元素值](assets/capture-3.gif)

在AEM Forms OSGI-我們必須編寫自訂OSGi搭售，以設定JSON資料元素的值。 本教學課程提供此套件。

我們在AEM工作流程中使用「流程步驟」。 我們將「在Json中設定元素值」OSGi套裝與此程式步驟產生關聯。

我們需要將兩個引數傳遞到設定值包。 第一個引數是元素的路徑，其值需要設定。 第二個引數是需要設定的值。

例如，在上述螢幕擷取中，我們將intialStep元素的值設為&quot;N&quot;

afData.afUnboundData.data.initialStep,N

在我們的範例中，我們有一個簡單的「離開時間請求」表單。 此表單的發起者會填入其姓名和截止日期。 提交時，此表單會前往「管理員」進行審核。 當管理員開啟表格時，第一個面板上的欄位會停用。 這是因為我們已將JSON資料中初始步驟元素的值設為N。

根據初始步驟欄位值，我們會顯示「經理」可核准或拒絕請求的核准者面板。

請檢視針對「初始步驟」設定的規則。 我們會根據initialStep欄位的值，使用「表單資料模型」擷取使用者詳細資料，並填入適當欄位，並隱藏／停用適當的面板。

若要在您的本機系統上部署資產：

* [下載並部署DevelopingWiteServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

*下[載並部署setvalue套件](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。 這是自訂OSGI搭售，可讓您在提交的json資料中設定元素的值。

* [下載並解壓縮zip檔案的內容](assets/set-value-jsondata.zip)
   * 將您的瀏覽器指向套 [件管理員](http://localhost:4502/crx/packmgr/index.jsp)
      * 匯入並安裝SetValueOfElementInJSONDataWorkflow.zip。此套件包含與表單相關聯的範例工作流程模型和表單資料模型。

* 將您的瀏覽器指向表 [單和檔案](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 按一下「建立」 |檔案上傳
* 上傳TimeOffRequestForm.zip檔案
   **此表單是使用AEM Forms 6.4建立。請確定您使用的是AEM Forms 6.4或更新版本**
* 開啟表 [單](http://localhost:4502/content/dam/formsanddocuments/timeoffrequest/jcr:content?wcmmode=disabled)
* 填寫開始和結束日期並提交表單。
* 前往「收 [件匣」](http://localhost:4502/aem/inbox)
* 開啟與任務關聯的表單。
* 請注意，第一個面板中的欄位已停用。
* 請注意，現在會顯示要核准或拒絕請求的面板。



>[!NOTE]
>
>由於我們使用使用者描述檔預先填入最適化表單，請確定管理員使 [用者描述檔資訊 ](http://localhost:4502/security/users.html)。 請至少確定您已設定「名字」、「姓氏」和「電子郵件」欄位值。
>您可以在此處啟用com.aemforms.setvalue.core.SetValueInJson的記錄程式，以啟用除錯 [記錄功能](http://localhost:4502/system/console/slinglog)

>[!NOTE]
>
>OSGi套件目前支援一次設定一個元素值的功能，以設定JSON資料中的資料元素值。 如果要設定多個元素值，則需要多次使用流程步驟。
>
>請確定「最適化表單」提交選項中的「資料檔案」路徑已設為「Data.xml」。 這是因為處理步驟中的代碼在裝載資料夾下查找名為Data.xml的檔案。
