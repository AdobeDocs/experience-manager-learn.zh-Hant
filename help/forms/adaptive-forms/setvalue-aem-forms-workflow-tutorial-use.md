---
title: 在AEM Forms工作流程中使用setvalue
description: 在AEM Forms OSGI中設定適用性Forms中元素的值已提交資料
feature: 適用性表單
topic: 開發
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '531'
ht-degree: 1%

---


# 在AEM Forms工作流程中使用setvalue

在AEM Forms OSGI工作流程中，設定適用性Forms中XML元素的值已提交資料。

![SetValue](assets/setvalue.png)

LiveCycle過去有設定值元件，可讓您設定XML元素的值。

根據此值，當表單填入XML時，您可以隱藏/停用表單的特定欄位或面板。

在AEM Forms OSGI — 我們必須撰寫自訂OSGi套件組合以在XML中設定值。 此套件組合是本教學課程內容的一部分。
我們在AEM工作流程中使用「處理步驟」。 我們將「在XML中設定元素的值」OSGi套件組合與此處理步驟關聯。
我們需要將兩個引數傳遞至設定值套件組合。 第一個引數是需要設定其值的XML元素的XPath。 第二個引數是需要設定的值。
例如，在上方的螢幕擷取中，我們會將初始步驟元素的值設為「N」。
根據此值，適用性Forms中的某些面板會隱藏或顯示。
在我們的範例中，我們提供簡單的「請求休息時間」表單。 此表單的發起人會填入其姓名和日期。 提交時，此表單會前往「管理員」進行審核。 管理員開啟表單時，第一個面板上的欄位會停用。 這是因為我們已將XML中初始步驟元素的值設為「N」。

我們會根據初始步驟欄位值顯示第二個面板， 「管理員」可在其中核准或拒絕請求

請使用規則編輯器，查看針對「請求的關閉時間」欄位所設定的規則。

若要在您的本機系統上部署資產，請遵循下列步驟：

* [部署Developmentwithserviceuser套件組合](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [部署範例套件組合](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。這是自訂OSGI套件組合，可讓您在提交的xml資料中設定元素的值

* [下載並解壓縮zip檔案的內容](assets/setvalueassets.zip)
* 將瀏覽器指向[包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* 匯入並安裝setValueWorkflow.zip。 這裡有範例工作流程模型。
* 將瀏覽器指向[Forms和Documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 按一下建立 |檔案上傳
* 上傳TimeOfRequestForm.zip
* 開啟[TimeOffRequestform](http://localhost:4502/content/dam/formsanddocuments/timeoffapplication/jcr:content?wcmmode=disabled)
* 填寫3個必填欄位並提交
* 以「管理員」身分登入AEM（如果尚未登入）
* 前往[&quot;AEM收件匣&quot;](http://localhost:4502/aem/inbox)
* 開啟「審核時間請求」表單
* 請注意，第一個面板中的欄位已停用。 這是因為表單由審核者開啟。 此外，請注意要核准或拒絕請求的面板現在已顯示

>[!NOTE]
>
>您可以為啟用記錄器，以啟用除錯記錄
>com.aemforms.setvalue.core.SetValueinXml
>將瀏覽器指向http://localhost:4502/system/console/slinglog

>[!NOTE]
>
>確認適用性表單提交選項中的資料檔案路徑已設為「Data.xml」。 這是因為處理步驟會在裝載資料夾下尋找名為Data.xml的檔案
