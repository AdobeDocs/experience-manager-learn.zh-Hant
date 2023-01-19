---
title: 在AEM 6.5.X上安裝GraphiQL IDE
description: 了解如何在AEM 6.5.X版上安裝和配置GraphiQL IDE
version: 6.5
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 11614
thumbnail: KT-10253.jpeg
source-git-commit: ae27cbc50fc5c4c2e8215d7946887b99d480d668
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 3%

---


# 在AEM 6.5.X上安裝GraphiQL IDE

在AEM 6.5中，需要手動安裝GraphiQL IDE工具，請遵循以下步驟安裝和配置。

1. 導覽至 **[Software Distribution入口網站](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEMas a Cloud Service**.
1. 搜尋「GraphiQL」(請務必包含 **i** in **GraphiQL**.
1. 下載最新 **GraphiQL內容包v.x.x.x**

   ![下載GraphiQL包](assets/graphiql/software-distribution.png)

   zip檔案是可直接安裝的AEM套件。

1. 從AEM開始功能表導覽至 **工具** > **部署** > **套件**.
1. 按一下 **上傳套件** 並選擇在上一步下載的包。 按一下 **安裝** 安裝套件。

   ![安裝GraphiQL包](assets/graphiql/install-graphiql-package.png)

1. 導覽至 **CRXDE Lite** > **存放庫面板** >選取 `/content/graphiql` 節點(例如， <http://localhost:4502/crx/de/index.jsp#/content/graphiql>)。
1. 在 **屬性** 頁簽更改值 `endpoint` 屬性 `/content/_cq_graphql/wknd-shared/endpoint.json`.
   ![端點屬性值更改](assets/graphiql/endpoint-prop-value-change.png)

1. 導覽至 **Web控制台配置** UI >搜尋 **CSRF濾鏡** 組態(例如<http://localhost:4502/system/console/configMgr/com.adobe.granite.csrf.impl.CSRFFilter)>
1. 在 `Excluded Paths` 屬性名稱欄位更新， WKND GraphQL端點路徑 `/content/cq:graphql/wknd-shared/endpoint`.
   ![排除路徑屬性值變更](assets/graphiql/exclude-paths-value-change.png)

1. 使用 `//HOST:PORT/content/graphiql.html`，然後確認您可以建構新查詢或執行現有查詢。 (例如 <http://localhost:4502/content/graphiql.html>)

![GraphiQL編輯器](assets/graphiql/graphiql-editor.png)

>[!TIP]
>
>若要支援專案專用的GraphQL結構和查詢執行，您必須為 `endpoint` 和 `Excluded Paths` 值。
