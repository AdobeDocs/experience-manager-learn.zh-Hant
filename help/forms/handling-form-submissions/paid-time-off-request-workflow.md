---
title: 簡單付費請求工作流
description: 在工作流中隱藏和顯示自適應表單面AEM板
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
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 0%

---

# 簡單付費請求工作流

在本文中，我們看到一個用於請求帶薪休假的簡單工作流。 業務要求如下：

* 用戶A通過填寫自適應表單請求超時。
* 表單被路由到AEM管理員用戶（在現實生活中它被路由到提交者的管理員）
* 管理員開啟窗體。 管理員不能編輯提交者填寫的任何資訊。
* 批准者部分應對批准者可見(在本例中是管理AEM員用戶)。

為了達到上述要求，我們使用一個叫做 **初始步長** 在表單中，其預設值設定為「是」。提交表單時，工作流中的第一步將初始步驟的值設定為「否」。 表單具有業務規則以根據初始步驟值隱藏和顯示相應的節。

**將表單配置為觸發工AEM作流**

>[!VIDEO](https://video.tv.adobe.com/v/28406?quality=12&learn=on)

**工作流**

>[!VIDEO](https://video.tv.adobe.com/v/28407?quality=12&learn=on)

**提交者對「暫停請求」表單的視圖**

![初始步長](assets/initialstep.gif)

**表單的批准者視圖**

![批准視圖](assets/approversview.gif)

在批准者視圖中，批准者無法編輯提交的資料。 還有一個新節僅用於批准者。

要在您的系統上test此工作流，請執行以下步驟：
* [下載並部署DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [下載並部署SetValue自定義OSGI捆綁包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
* [將與本條相關的資產導入AEM](assets/helpxworkflow.zip)
* 開啟 [請求時間表](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* 填寫詳細資訊並提交
* 開啟 [收件箱](http://localhost:4502/mnt/overlay/cq/inbox/content/inbox.html)。 您應看到已分配的新任務。 開啟窗體。 提交者的資料應為只讀，並且新批准者部分應可見。
* 瀏覽 [工作流模型](http://localhost:4502/editor.html/conf/global/settings/workflow/models/helpxworkflow.html)
* 瀏覽流程步驟。 這是將初始步驟的值設定為「否」的步驟。
