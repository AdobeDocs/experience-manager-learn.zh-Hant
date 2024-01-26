---
title: 實施最適化表單的儲存並繼續功能
description: 瞭解如何從Azure儲存體帳戶儲存及擷取最適化表單資料。
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-13717
exl-id: b40b0ef4-efa9-400e-82d8-aa0c7feb7be4
duration: 36
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 2%

---

# 簡介

在本教學課程中，我們將實作允許表單填寫器儲存並繼續表單填寫流程的簡單使用案例。 使用案例的流程如下

* 使用者開始填寫最適化表單。
* 使用者儲存表單，並想要在稍後繼續填寫表單。
* 使用者收到電子郵件通知，其中包含已儲存表單的連結。

## 先決條件

* 體驗AEM Forms CS，尤其是建立表單資料模型的體驗。
* 使用Cloud Manager部署程式碼的體驗。
* 存取AEM Forms CS的雲端就緒例項。

若要在AEM Forms CS中實作上述使用案例，您將需要下列專案

* [AEM Forms CS雲端就緒例項](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/intellij-and-aem-sync.html?lang=en#set-up-aem-author-instance)
* [Azure入口網站帳戶](https://portal.azure.com/)
* [SendGrid帳戶](https://sendgrid.com/)

### 後續步驟

[建立頁面元件](./page-component.md)
