---
title: 在AEM Forms工作流程中設定Json資料元素的值
description: 在AEM工作流程中，將適用性表單轉送給不同的使用者時，需要根據檢閱表單的人員，隱藏或停用特定欄位或面板。 為了滿足這些使用案例，我們通常會設定隱藏欄位的值。 可以根據此隱藏欄位的值業務規則來隱藏/停用適當的面板或欄位。
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

# 在AEM Forms工作流程中設定JSON資料元素的值 {#setting-value-of-json-data-element-in-aem-forms-workflow}

在AEM工作流程中，將適用性表單轉送給不同的使用者時，需要根據檢閱表單的人員，隱藏或停用特定欄位或面板。 為了滿足這些使用案例，我們通常會設定隱藏欄位的值。 可以根據此隱藏欄位的值業務規則來隱藏/停用適當的面板或欄位。

![在json資料中設定元素的值](assets/capture-3.gif)

在AEM Forms OSGi中 — 我們必須建立自訂OSGi套件組合以設定JSON資料元素的值。 此套件組合是本教學課程內容的一部分。

我們在AEM工作流程中使用「處理步驟」。 我們將「在Json中設定元素的值」OSGi套件組合與此程式步驟建立關聯。

我們需要將兩個引數傳遞至設定值套件組合。 第一個引數是需要設定其值的元素的路徑。 第二個引數是需要設定的值。

例如，在上方的螢幕擷取中，我們將intialStep元素的值設為「N」

afData.afUnboundData.data.initialStep,N

在我們的範例中，我們提供簡單的「請求休息時間」表單。 此表單的發起人會填入其姓名和日期。 提交後，此表單會轉給「管理員」審核。 管理員開啟表單時，第一個面板上的欄位會停用。 因為我們已將JSON資料中初始步驟元素的值設為N。

根據初始步驟欄位值，我們會顯示「管理員」可在其中批准或拒絕請求的核准者面板。

請查看針對「初始步驟」設定的規則。 我們會根據initialStep欄位的值，使用「表單資料模型」來擷取使用者詳細資訊，並填入適當欄位並隱藏/停用適當的面板。

若要在本機系統上部署資產：

* [下載並部署DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [下載並部署setvalue套件組合](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). 這是自訂OSGI套件組合，可讓您在提交的json資料中設定元素的值。

* [下載並解壓縮zip檔案的內容](assets/set-value-jsondata.zip)
   * 將瀏覽器指向 [封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)
      * 導入和安裝SetValueOfElementInJSONDataWorkflow.zip。此包包含與表單關聯的示例工作流模型和表單資料模型。

* 將瀏覽器指向 [Forms與檔案](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 按一下建立 |檔案上傳
* 上傳TimeOffRequestForm.zip檔案
   **此表單是使用AEM Forms 6.4建置的。請確定您使用AEM Forms 6.4或更新版本**
* 開啟 [表單](http://localhost:4502/content/dam/formsanddocuments/timeoffrequest/jcr:content?wcmmode=disabled)
* 填寫「開始日期」和「結束日期」並提交表單。
* 前往 [&quot;收件箱&quot;](http://localhost:4502/aem/inbox)
* 開啟與任務關聯的表單。
* 請注意，第一個面板中的欄位已停用。
* 請注意，系統現在會顯示核准或拒絕請求的面板。

>[!NOTE]
>
>由於我們使用使用者設定檔預先填入適用性表單，請確定管理員 [使用者設定檔資訊 ](http://localhost:4502/security/users.html). 請至少確定您已設定FirstName、LastName和Email欄位值。
>您可以為com.aemforms.setvalue.core.SetValueInJson啟用記錄器，以啟用除錯記錄 [從這裡](http://localhost:4502/system/console/slinglog)

>[!NOTE]
>
>用於設定JSON資料中資料元素值的OSGi套件目前支援一次設定一個元素值的功能。 如果要設定多個元素值，則需要多次使用處理步驟。
>
>確認適用性表單提交選項中的資料檔案路徑已設為「Data.xml」。 這是因為處理步驟中的程式碼會在裝載資料夾下尋找名為Data.xml的檔案。
