---
title: 使用存放庫瀏覽器除錯AEM
description: 存放庫瀏覽器是功能強大的工具，可讓您檢視AEM的基本資料存放區，以便更輕鬆地偵錯AEM as a Cloud Service環境。
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-10004
thumbnail: 341464.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 88af40fc-deff-4b92-84b1-88df2dbdd90b
duration: 305
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 0%

---

# 使用存放庫瀏覽器除錯AEM as a Cloud Service

存放庫瀏覽器是功能強大的工具，可讓您檢視AEM的基本資料存放區，以便更輕鬆地偵錯AEM as a Cloud Service環境。 存放庫瀏覽器支援生產、暫存和開發以及作者、發佈和預覽服務中AEM的資源和屬性的唯讀檢視。

>[!VIDEO](https://video.tv.adobe.com/v/3447065?quality=12&learn=on&captions=chi_hant)

存放庫瀏覽器僅&#x200B;__可在AEM as a Cloud Service環境中使用__ (使用[CRXDE Lite](../aem-sdk-local-quickstart/other-tools.md#crxde-lite)對本機AEM SDK進行偵錯)。

## 存取存放庫瀏覽器

若要存取AEM as a Cloud Service上的存放庫瀏覽器：

1. 確定您的使用者具有[必要的存取權](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html?lang=zh-Hant#access-prerequisites)
1. 登入[Cloud Manager](https://my.cloudmanager.adobe.com)
1. 選取包含AEM as a Cloud Service環境要除錯的方案
1. 開啟與AEM as a Cloud Service環境相對應的[Developer Console](./developer-console.md)以進行偵錯
1. 選取&#x200B;__存放庫瀏覽器__&#x200B;索引標籤
1. 選取要瀏覽的AEM服務層
   + 所有作者
   + 所有發行者
   + 所有預覽
1. 選取&#x200B;__開啟存放庫瀏覽器__

「存放庫瀏覽器」會以唯讀模式開啟所選服務層（作者、發佈或預覽），顯示您的使用者可存取的資源與屬性。

## 發佈和預覽存取權

依預設，存取發佈或預覽是有限的，減少了存放庫瀏覽器中的可用資源。 [若要檢視發佈（或預覽）上的所有資源，請將使用者新增至發佈（或預覽）管理員角色。](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html?lang=zh-Hant#navigate-the-hierarchy)
