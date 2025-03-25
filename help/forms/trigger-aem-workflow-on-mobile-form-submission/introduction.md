---
title: 在HTML5表單提交簡介時觸發AEM工作流程
description: 在行動表單提交時觸發AEM工作流程
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2024-09-17T00:00:00Z
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: ce51b25f-6069-4799-9a61-98c0a672e821
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 1%

---

# 在行動表單提交時觸發AEM工作流程

一個常見的使用案例是將XDP轉譯為HTML以進行資料擷取活動。 提交此表單時，可能需要觸發AEM工作流程。 在AEM工作流程中，您可以將資料與XDP範本合併，並顯示產生的PDF以供檢閱和核准。 表單會在發佈執行個體上轉譯，而工作流程會在AEM處理執行個體上觸發。

使用案例包含下列步驟

* 使用者填寫並提交HTML5表單(XDP的HTML5轉譯)。
* 提交是由發佈執行個體中的servlet所處理。
* 此servlet會將資料儲存在AEM處理執行個體存放庫中的預先定義資料夾。
* 工作流程啟動器會設定為每次在特定資料夾底下新增檔案時都會觸發AEM工作流程。

本教學課程將逐步解說完成上述使用案例所需的步驟。 此教學課程的相關範常式式碼和資產[可在此取得。](./deploy-assets.md)


## 後續步驟

[處理表單提交](./handle-form-submission.md)
