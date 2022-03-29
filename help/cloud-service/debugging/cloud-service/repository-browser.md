---
title: 使用儲存AEM庫瀏覽器調試
description: 儲存庫瀏覽器是一種功能強大的工具，可AEM以查看基礎資料儲存，從而輕鬆調試AEMas a Cloud Service環境。
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
source-git-commit: 467b0c343a28eb573498a013b5490877e4497fe0
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---


# 使用儲存AEM庫瀏覽器調試as a Cloud Service

儲存庫瀏覽器是一種功能強大的工具，可AEM以查看基礎資料儲存，從而輕鬆調試AEMas a Cloud Service環境。 儲存庫瀏覽器支援生產、階段和開發以及作者、AEM發佈和預覽服務上的資源和屬性的只讀視圖。

>[!VIDEO](https://video.tv.adobe.com/v/341464/?quality=12&learn=on)

儲存庫瀏覽器是 __僅__ 在as a Cloud ServiceAEM環境中可用(使用 [CRXDE Lite](../aem-sdk-local-quickstart/other-tools.md#crxde-lite) 調試本AEM地SDK)。

## 訪問儲存庫瀏覽器

要在as a Cloud Service上訪問儲存庫瀏覽AEM器：

1. 確保用戶 [所需的訪問](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#access-prerequisites)
1. 登錄到 [雲管理器](https://my.cloudmanager.adobe.com)
1. 選擇包含要調試的AEMas a Cloud Service環境的程式
1. 開啟 [開發人員控制台](./developer-console.md) 與要調AEM試的as a Cloud Service環境對應
1. 選擇 __儲存庫瀏覽器__ 頁籤
1. 選擇要AEM瀏覽的服務層
   + 所有作者
   + 所有發佈者
   + 所有預覽
1. 選擇 __開啟儲存庫瀏覽器__

以只讀模式為所選服務層（「作者」、「發佈」或「預覽」）開啟「儲存庫瀏覽器」，顯示用戶有權訪問的資源和屬性。

## 發佈和預覽訪問

預設情況下，對「發佈」或「預覽」的訪問權限受到限制，從而減少了「儲存庫瀏覽器」中的可用資源。 [要查看發佈（或預覽）上的所有資源，請將用戶添加到發佈（或預覽）管理員角色。](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#navigate-the-hierarchy)

