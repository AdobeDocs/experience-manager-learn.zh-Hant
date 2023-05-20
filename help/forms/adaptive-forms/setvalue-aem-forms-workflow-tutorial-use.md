---
title: 在AEM Forms工作流中使用setvalue
description: 在AEM FormsOSGI中設定自適應Forms提交資料中元素的值
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: 3919efee-6998-48e8-85d7-91b6943d23f9
last-substantial-update: 2020-01-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 0%

---

# 在AEM Forms工作流中使用setvalue

設定Adaptive Forms中XML元素的值在AEM FormsOSGI工作流中提交了資料。

![設定值](assets/setvalue.png)

LiveCycle用於具有設定值元件，該元件允許您設定XML元素的值。

根據此值，在使用XML填充表單時，可以隱藏/禁用表單的某些欄位或面板。

在AEM FormsOSGI — 我們必須編寫自定義OSGi捆綁包以在XML中設定值。 本教程將提供該捆綁包。
我們在工作流中使AEM用Process Step。 我們將「在XML中設定元素值」OSGi捆綁包與此過程步驟關聯。
我們需要將兩個參數傳遞給設定值包。 第一個參數是需要設定其值的XML元素的XPath。 第二個參數是需要設定的值。
例如，在上面的螢幕抓圖中，我們將intialstep元素的值設定為「N」。
根據此值，自適應Forms中的某些面板將被隱藏或顯示。
在我們的示例中，我們有一個簡單的「暫停請求表」。 此表單的發起者填寫其姓名和結束日期。 提交時，此表單將轉到「admin」進行審閱。 管理員開啟表單時，第一個面板上的欄位將被禁用。 這是因為我們已將XML中初始步驟元素的值設定為「N」。

根據初始步驟欄位值，我們顯示第二個面板，其中「admin」可以批准或拒絕請求

請使用規則編輯器查看針對「請求的關機時間」欄位設定的規則。

要在本地系統上部署資產，請執行以下步驟：

* [部署Developingwithserviceuser捆綁包](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [部署示例包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。 這是自定義OSGI捆綁包，它允許您在提交的xml資料中設定元素的值

* [下載並解壓縮zip檔案的內容](assets/setvalueassets.zip)
* 將瀏覽器指向 [軟體包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* 導入並安裝setValueWorkflow.zip。 這包含示例工作流模型。
* 將瀏覽器指向 [Forms和文檔](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 按一下建立 |檔案上載
* 上載TimeOfRequestForm.zip
* 開啟 [TimeOffRequestform](http://localhost:4502/content/dam/formsanddocuments/timeoffapplication/jcr:content?wcmmode=disabled)
* 填寫3個必填欄位並提交
* 以「admin」身份登錄AEM到（如果尚未登錄）
* 轉到 [&quot;收AEM件箱&quot;](http://localhost:4502/aem/inbox)
* 開啟「審核時間請求」表單
* 請注意，第一個面板中的欄位被禁用。 這是因為該表單正由審閱者開啟。 另外，請注意批准或拒絕請求的面板現在可見

>[!NOTE]
>
>通過為啟用記錄器啟用調試日誌記錄
>com.aemforms.setvalue.core.SetValueinXml
>將瀏覽器指向http://localhost:4502/system/console/slinglog

>[!NOTE]
>
>確保自適應表單的提交選項中的資料檔案路徑設定為「Data.xml」。 這是因為進程步驟在負載資料夾下查找名為Data.xml的檔案
