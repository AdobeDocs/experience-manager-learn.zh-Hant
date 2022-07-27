---
source-git-commit: 10032375a23ba99f674fb59a6f6e48bf93908421
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 0%

---



# 無AEM頭部署

無AEM頭客戶端部署採用多種形式；托AEM管SPA、外部SPA或網站、移動應用甚至伺服器到伺服器進程。

根據客戶端及其部署方式，無AEM頭部署有不同的考慮。

## AEM服務架構

在探索部署考慮事項之前，必須了AEM解邏輯體系結構以及as a Cloud Service服務層AEM的分離和角色。 AEMas a Cloud Service包括兩個邏輯服務：

+ __AEM作者__ 是團隊建立、協作和發佈內容片段（和其他資產）的服務
+ __AEM發佈__ 是複製已發佈內容片段（和其他資產）以用於一般消耗的服務

以生AEM產容量運行的無頭客戶端通常與AEM Publish交互，AEM Publish包含經批准的已發佈內容。 與AEM Author交互的客戶端需要特別考慮，因為AEM Author預設是安全的，需要對所有請求進行授權，並且可能包含不完整或未批准的內容。

## 無頭客戶端部署

+ [單頁應用](./spa.md)
+ [Web元件/JS](./web-component.md)
+ [移動應用](./mobile.md)
+ [伺服器到伺服器](./server-to-server.md)

