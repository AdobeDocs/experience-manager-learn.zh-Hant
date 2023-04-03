---
title: 使用存放庫瀏覽器除錯AEM
description: 存放庫瀏覽器是功能強大的工具，可顯示AEM基礎資料存放區，以輕鬆對AEMas a Cloud Service環境進行除錯。
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 10004
thumbnail: 341464.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 88af40fc-deff-4b92-84b1-88df2dbdd90b
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# 使用存放庫瀏覽器除錯AEMas a Cloud Service

存放庫瀏覽器是功能強大的工具，可顯示AEM基礎資料存放區，以輕鬆對AEMas a Cloud Service環境進行除錯。 存放庫瀏覽器支援在生產、預備和開發，以及製作、發佈和預覽服務上，以唯讀方式檢視AEM的資源和屬性。

>[!VIDEO](https://video.tv.adobe.com/v/341464?quality=12&learn=on)

儲存庫瀏覽器是 __僅__ 適用於AEMas a Cloud Service環境(使用 [CRXDE Lite](../aem-sdk-local-quickstart/other-tools.md#crxde-lite) 除錯本機AEM SDK)。

## 存取存放庫瀏覽器

若要在AEMas a Cloud Service上存取存放庫瀏覽器：

1. 確認您的使用者已 [必要的訪問](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#access-prerequisites)
1. 登入 [Cloud Manager](https://my.cloudmanager.adobe.com)
1. 選取包含AEMas a Cloud Service環境的程式進行偵錯
1. 開啟 [開發人員控制台](./developer-console.md) 對應至AEMas a Cloud Service環境以偵錯
1. 選取 __存放庫瀏覽器__ 標籤
1. 選擇要瀏覽的AEM服務層
   + 所有作者
   + 所有發佈者
   + 所有預覽
1. 選擇 __開啟儲存庫瀏覽器__

系統會以唯讀模式開啟選定服務層（製作、發佈或預覽）的「儲存庫瀏覽器」，顯示您的用戶有權訪問的資源和屬性。

## 發佈和預覽存取權

依預設，對發佈或預覽的存取權限有限，減少存放庫瀏覽器中的可用資源。 [若要在「發佈」（或「預覽」）上檢視所有資源，請將使用者新增至「發佈」（或「預覽」）管理員角色。](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#navigate-the-hierarchy)
