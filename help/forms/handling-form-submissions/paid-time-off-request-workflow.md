---
title: 簡易付費請求工作流程
description: 在AEM工作流程中隱藏和顯示最適化表單面板
uuid: 28ceb72b-24d9-488e-92af-7e85775dc682
feature: integrations
topics: workflow
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
discoiquuid: 1c4822e6-76ce-446b-98cb-408900d68b24
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '332'
ht-degree: 0%

---


# 簡易付費請求工作流程

在本文中，我們將檢視用於請求付費休息的簡單工作流程。 業務要求如下：

* 使用者A會填入最適化表單，要求暫停。
* 表單會路由至AEM管理員使用者（在實際生活中，表單會路由至提交者的管理員）
* 管理員開啟表單。 管理員不能編輯提交者填寫的任何資訊。
* 核准者區段應該會顯示給核准者（在此例中為AEM管理員使用者）。

為滿足上述要求，我們在表單中使用名為&#x200B;**initialstep**&#x200B;的隱藏欄位，其預設值設定為「是」。提交表單時，工作流中的第一步將初始步驟的值設定為「否」。 表單有業務規則可根據初始步驟值來隱藏和顯示適當的章節。

**設定表單以觸發AEM工作流程**

>[!VIDEO](https://video.tv.adobe.com/v/28406?quality=9&learn=on)

**工作流程逐步說明**

>[!VIDEO](https://video.tv.adobe.com/v/28407?quality=9&learn=on)

**提交者對「關閉時間請求」表單的視圖**

![initialstep](assets/initialstep.gif)

**表單的批准者檢視**

![approverview](assets/approversview.gif)

在批准者視圖中，批准者無法編輯提交的資料。 此外，還有一個僅適用於「批准者」的新部分。

若要在您的系統上測試此工作流程，請遵循下列步驟：
* [下載並部署DevelopingWiteServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [下載並部署SetValue自訂OSGI套件](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
* [將與此文章相關的資產匯入AEM](assets/helpxworkflow.zip)
* 開啟[關閉時間請求表單](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* 填寫詳細資訊並提交
* 開啟[inbox](http://localhost:4502/mnt/overlay/cq/inbox/content/inbox.html)。 您應該會看到指派的新任務。 開啟表格。 提交者的資料應為只讀，並且應顯示新的批准者部分。
* 探索[工作流程模型](http://localhost:4502/editor.html/conf/global/settings/workflow/models/helpxworkflow.html)
* 探索程式步驟。 此步驟將初始步驟的值設定為「否」。
