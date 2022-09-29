---
title: 簡單的付費請求工作流程
description: 在AEM工作流程中隱藏和顯示最適化表單面板
uuid: 28ceb72b-24d9-488e-92af-7e85775dc682
feature: Adaptive Forms
topics: workflow
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
discoiquuid: 1c4822e6-76ce-446b-98cb-408900d68b24
topic: Development
role: Developer
level: Experienced
exl-id: 9342bd2f-2ba9-42ee-9224-055649ac3c90
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 0%

---

# 簡單的付費請求工作流程

在本文中，我們會審視用於要求付費時間休假的簡單工作流程。 業務要求如下：

* 使用者A需要借由填入最適化表單來暫停。
* 表單會轉寄給AEM管理員使用者（在實際中會轉寄給提交者的管理員）
* 管理員會開啟表單。 管理員不應編輯提交者填寫的任何資訊。
* 核准者區段應該會顯示給核准者(在此例中是AEM管理員使用者)。

為達到上述要求，我們使用名為的隱藏欄位 **初始化步驟** 在窗體中，其預設值設定為「是」。提交窗體時，工作流的第一步將初始步驟的值設定為「否」。 表單具有業務規則，可根據初始步驟值來隱藏和顯示適當的部分。

**設定表單以觸發AEM工作流程**

>[!VIDEO](https://video.tv.adobe.com/v/28406?quality=9&learn=on)

**工作流程逐步說明**

>[!VIDEO](https://video.tv.adobe.com/v/28407?quality=9&learn=on)

**「提交者」的「關閉時間請求」表單視圖**

![初始化步驟](assets/initialstep.gif)

**表單的核准者檢視**

![approverview](assets/approversview.gif)

在核准者檢視中，核准者無法編輯已提交的資料。 還有一個僅用於「批准者」的新部分。

若要在您的系統上測試此工作流程，請遵循下列步驟：
* [下載並部署DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [下載並部署SetValue自訂OSGI套件組合](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
* [將與本文相關的資產匯入AEM](assets/helpxworkflow.zip)
* 開啟 [請求時間表](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* 填寫詳細資訊並提交
* 開啟 [收件匣](http://localhost:4502/mnt/overlay/cq/inbox/content/inbox.html). 您應該會看到已指派新任務。 開啟表單。 提交者的資料應為唯讀狀態，且應顯示新的核准者區段。
* 探索 [工作流模型](http://localhost:4502/editor.html/conf/global/settings/workflow/models/helpxworkflow.html)
* 探索程式步驟。 這是將初始步驟的值設定為否的步驟。
