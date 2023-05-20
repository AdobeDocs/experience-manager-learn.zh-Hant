---
title: 在AEM Forms工作流中設定Json資料元素的值
description: 當自適應表單被路由到工作流中的不同用戶AEM時，有要求根據審閱表單的人員隱藏或禁用某些欄位或面板。 為了滿足這些使用情形，我們通常設定隱藏欄位的值。 可基於此隱藏欄位的值業務規則創作以隱藏/禁用相應的面板或欄位。
feature: Adaptive Forms
version: 6.4
topic: Development
role: Developer
level: Experienced
exl-id: fbe6d341-7941-46f5-bcd8-58b99396d351
last-substantial-update: 2021-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '685'
ht-degree: 1%

---

# 在AEM Forms工作流中設定JSON資料元素的值 {#setting-value-of-json-data-element-in-aem-forms-workflow}

當自適應表單被路由到工作流中的不同用戶AEM時，有要求根據審閱表單的人員隱藏或禁用某些欄位或面板。 為了滿足這些使用情形，我們通常設定隱藏欄位的值。 可基於此隱藏欄位的值業務規則創作以隱藏/禁用相應的面板或欄位。

![設定json資料中元素的值](assets/capture-3.gif)

在AEM FormsOSGi中 — 我們必須建立自定義OSGi捆綁包以設定JSON資料元素的值。 本教程將提供該捆綁包。

我們在工作流中使AEM用Process Step。 我們將「Set Value of Element in Json」 OSGi捆綁包與此進程步驟關聯。

我們需要將兩個參數傳遞給設定值包。 第一個參數是需要設定其值的元素的路徑。 第二個參數是需要設定的值。

例如，在上面的螢幕截圖中，我們將intialStep元素的值設定為&quot;N&quot;

afData.afUnboundData.data.initialStep,N

在我們的示例中，我們有一個簡單的「暫停請求表」。 此表單的發起者填寫其姓名和結束日期。 提交後，此表單將轉到「經理」進行審閱。 當管理器開啟表單時，第一個面板上的欄位將被禁用。 這是因為我們已將JSON資料中初始步驟元素的值設定為N。

根據初始步驟欄位值，我們將顯示「經理」可以批准或拒絕請求的批准者面板。

請查看針對「初始步驟」設定的規則。 根據initialStep欄位的值，我們使用表單資料模型獲取用戶詳細資訊，並填充相應欄位並隱藏/禁用相應面板。

要在本地系統上部署資產，請執行以下操作：

* [下載並部署DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [下載並部署setvalue包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。 這是自定義OSGI捆綁包，允許您設定提交的json資料中的元素值。

* [下載並解壓縮zip檔案的內容](assets/set-value-jsondata.zip)
   * 將瀏覽器指向 [軟體包管理器](http://localhost:4502/crx/packmgr/index.jsp)
      * 導入並安裝SetValueOfElementInJSONDataWorkflow.zip。此包具有與表單關聯的示例工作流模型和表單資料模型。

* 將瀏覽器指向 [Forms和文檔](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 按一下建立 |檔案上載
* 上載TimeOffRequestForm.zip檔案
   **這個表格是用AEM Forms6.4建的。請確保您在AEM Forms6.4或更高版本**
* 開啟 [表格](http://localhost:4502/content/dam/formsanddocuments/timeoffrequest/jcr:content?wcmmode=disabled)
* 填寫「起始日期」和「終止日期」並提交表單。
* 轉到 [&quot;收件箱&quot;](http://localhost:4502/aem/inbox)
* 開啟與任務關聯的表單。
* 請注意，第一個面板中的欄位被禁用。
* 請注意，批准或拒絕請求的面板現在可見。

>[!NOTE]
>
>由於我們正在使用用戶配置檔案預填充自適應表單，請確保管理員 [用戶配置檔案資訊 ](http://localhost:4502/security/users.html)。 至少確保已設定FirstName、LastName和E-mail欄位值。
>可以通過為com.aemforms.setvalue.core.SetValueInJson啟用記錄器來啟用調試日誌記錄 [從這裡](http://localhost:4502/system/console/slinglog)

>[!NOTE]
>
>用於設定JSON資料中資料元素值的OSGi捆綁包當前支援一次設定一個元素值的能力。 如果要設定多個元素值，則需要多次使用流程步驟。
>
>確保自適應表單的提交選項中的資料檔案路徑設定為「Data.xml」。 這是因為進程步驟中的代碼在負載資料夾下查找名為Data.xml的檔案。
