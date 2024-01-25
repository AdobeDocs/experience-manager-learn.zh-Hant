---
title: 簡易帶薪休假請求工作流程
description: 在AEM Workflow中隱藏和顯示最適化表單面板
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
discoiquuid: 1c4822e6-76ce-446b-98cb-408900d68b24
topic: Development
role: Developer
level: Experienced
exl-id: 9342bd2f-2ba9-42ee-9224-055649ac3c90
last-substantial-update: 2020-07-07T00:00:00Z
duration: 601
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '305'
ht-degree: 0%

---

# 簡易帶薪休假請求工作流程

在本文中，我們將討論申請付費休假的簡單工作流程。 業務需求如下：

* 使用者A透過填寫最適化表單來要求休假。
* 表單會路由傳送給AEM管理員使用者（在現實情況中，它會路由傳送給提交者的管理員）
* 管理員會開啟表單。 管理員應該無法編輯提交者填入的任何資訊。
* 核准者區段應該對核准者可見(在此情況下是AEM管理員使用者)。

為了滿足上述要求，我們使用名為的隱藏欄位 **初始步驟** 在表單中，其預設值設定為「是」。提交表單時，工作流程中的第一個步驟會將initialstep的值設定為「否」。 表單具有商業規則，可根據初始步驟值隱藏和顯示適當的區段。

**設定表單以觸發AEM Workflow**

>[!VIDEO](https://video.tv.adobe.com/v/28406?quality=12&learn=on)

**工作流程逐步說明**

>[!VIDEO](https://video.tv.adobe.com/v/28407?quality=12&learn=on)

**提交者的休假要求表單檢視**

![初始步驟](assets/initialstep.gif)

**表單的核准者檢視**

![核准者檢視](assets/approversview.gif)

在核准者檢視中，核准者無法編輯提交的資料。 此外，也有新區段僅供核准者使用。

若要在系統上測試此工作流程，請遵循下列步驟：
* [下載並部署DevelopingWidthServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [下載並部署SetValue自訂OSGI套件組合](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
* [將與本文相關的資產匯入AEM](assets/helpxworkflow.zip)
* 開啟 [休假請求表單](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* 填寫詳細資料並提交
* 開啟 [收件匣](http://localhost:4502/mnt/overlay/cq/inbox/content/inbox.html). 您應該會看到新任務已指派。 開啟表單。 提交者的資料應為唯讀，且應會顯示新的核准者區段。
* 探索 [工作流程模型](http://localhost:4502/editor.html/conf/global/settings/workflow/models/helpxworkflow.html)
* 探索流程步驟。 此步驟會將initialstep的值設為「否」。
