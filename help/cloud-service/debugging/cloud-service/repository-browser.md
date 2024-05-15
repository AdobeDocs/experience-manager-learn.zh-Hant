---
title: 使用存放庫瀏覽器除錯AEM
description: 存放庫瀏覽器是功能強大的工具，可提供AEM基礎資料存放區的可見度，允許輕鬆偵錯AEMas a Cloud Service環境。
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-10004
thumbnail: 341464.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 88af40fc-deff-4b92-84b1-88df2dbdd90b
duration: 305
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 0%

---

# 使用存放庫瀏覽器偵錯AEMas a Cloud Service

存放庫瀏覽器是功能強大的工具，可提供AEM基礎資料存放區的可見度，允許輕鬆偵錯AEMas a Cloud Service環境。 存放庫瀏覽器支援生產、暫存和開發以及作者、發佈和預覽服務上AEM的資源和屬性的唯讀檢視。

>[!VIDEO](https://video.tv.adobe.com/v/341464?quality=12&learn=on)

存放庫瀏覽器是 __僅限__ 可在AEMas a Cloud Service環境中取得(使用 [CRXDE Lite](../aem-sdk-local-quickstart/other-tools.md#crxde-lite) 以偵錯本機AEM SDK)。

## 存取存放庫瀏覽器

若要在AEMas a Cloud Service上存取「存放庫瀏覽器」：

1. 確保您的使用者已 [必要的存取權](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#access-prerequisites)
1. 登入 [Cloud Manager](https://my.cloudmanager.adobe.com)
1. 選取包含AEMas a Cloud Service環境要偵錯的方案
1. 開啟 [開發人員主控台](./developer-console.md) 與要偵錯的AEMas a Cloud Service環境相對應
1. 選取 __存放庫瀏覽器__ 標籤
1. 選取要瀏覽的AEM服務層
   + 所有作者
   + 所有發行者
   + 所有預覽
1. 選取 __開啟存放庫瀏覽器__

「存放庫瀏覽器」會以唯讀模式開啟所選服務層（作者、發佈或預覽），顯示您的使用者可存取的資源與屬性。

## 發佈和預覽存取權

依預設，存取發佈或預覽是有限的，減少了存放庫瀏覽器中的可用資源。 [若要檢視發佈（或預覽）上的所有資源，請將使用者新增至發佈（或預覽）管理員角色。](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#navigate-the-hierarchy)
