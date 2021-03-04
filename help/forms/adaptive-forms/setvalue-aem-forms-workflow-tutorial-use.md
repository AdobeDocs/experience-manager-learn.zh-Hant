---
title: 在AEM Forms工作流中使用setvalue
seo-title: 在AEM Forms工作流中使用setvalue
description: AEM FormsOSGI中自適應Forms提交資料中元素值的設定
seo-description: AEM FormsOSGI中自適應Forms提交資料中元素值的設定
uuid: fe431e48-f05b-4b23-94d2-95d34d863984
feature: 適用性表單
topics: developing
audience: implementer
doc-type: article
activity: setup
discoiquuid: dbd87302-f770-4e61-b5ad-3fc5831b4613
topic: 開發
role: 開發人員
level: 經驗豐富
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '552'
ht-degree: 1%

---


# 在AEM Forms工作流中使用setvalue

在Adaptive Forms中設定XML元素的值，在AEM FormsOSGI工作流中提交資料。

![SetValue](assets/setvalue.png)

LiveCycle用於具有設定值元件，該元件允許您設定XML元素的值。

根據此值，當表單填入XML時，您可以隱藏／停用表單的特定欄位或面板。

在AEM FormsOSGI-我們必須編寫自訂的OSGi套件，才能在XML中設定值。 本教學課程提供此套件。
我們在工作流中使用「流程AEM步驟」。 我們將「在XML中設定元素的值」OSGi套件與此程式步驟關聯。
我們需要將兩個引數傳遞到設定值包。 第一個引數是需要設定其值的XML元素的XPath。 第二個引數是需要設定的值。
例如，在上述螢幕擷取中，我們將intialstep元素的值設定為&quot;N&quot;。
根據此值，最適化Forms中的某些面板會隱藏或顯示。
在我們的範例中，我們有一個簡單的「離開時間請求」表單。 此表單的發起者會填入其姓名和截止日期。 提交時，此表格會前往「管理員」進行審核。 管理員開啟表單時，第一個面板上的欄位會停用。 這是因為我們已將XML中初始步驟元素的值設為&quot;N&quot;。

根據初始步驟欄位值，我們會顯示第二個面板，「管理員」可在其中核准或拒絕請求

請使用規則編輯器查看針對「請求的關閉時間」欄位所設定的規則。

若要在您的本機系統上部署資產，請遵循下列步驟：

* [部署使用服務使用者套件進行開發](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [部署範例套件](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。這是自訂OSGI套件，可讓您在提交的xml資料中設定元素的值

* [下載並解壓縮zip檔案的內容](assets/setvalueassets.zip)
* 將瀏覽器指向[軟體包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* 匯入並安裝setValueWorkflow.zip。 此為範例工作流程模型。
* 將您的瀏覽器指向[Forms和檔案](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 按一下「建立」 |檔案上傳
* 上傳TimeOfRequestForm.zip
* 開啟[TimeOffRequestform](http://localhost:4502/content/dam/formsanddocuments/timeoffapplication/jcr:content?wcmmode=disabled)
* 填寫3個必填欄位並提交
* 以「管理員」身分登入AEM（如果您尚未登入）
* 前往[「收件匣AEM」](http://localhost:4502/aem/inbox)
* 開啟「檢閱時間請求」表單
* 請注意，第一個面板中的欄位已停用。 這是因為表單由審核者開啟。 此外，請注意，現在會顯示要核准或拒絕請求的面板

>[!NOTE]
>
>You can enable debug logging by enabling logger for
>com.aemforms.setvalue.core.SetValueinXml
>將您的瀏覽器指向http://localhost:4502/system/console/slinglog

>[!NOTE]
>
>請確定「最適化表單」提交選項中的「資料檔案」路徑已設為「Data.xml」。 這是因為處理步驟會在裝載資料夾下尋找名為Data.xml的檔案
