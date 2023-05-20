---
title: 在6.5上安裝AEMGraphiQL IDE
description: 瞭解如何在6.5上安裝和配置AEMGraphiQL IDE
version: 6.5
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 11614
thumbnail: KT-10253.jpeg
exl-id: 04fcc24c-7433-4443-a109-f01840ef1a89
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 3%

---

# 在6.5上安裝AEMGraphiQL IDE

在AEM6.5中，必須手動安裝GraphiQL IDE工具。

1. 導航到 **[軟體分發門戶](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEMas a Cloud Service**。
1. 搜索「GraphiQL」(請務必包括 **我** 在 **圖形QL**)。
1. 下載最新 **GraphiQL內容包v.x.x.x**。

   ![下載GraphiQL包](assets/graphiql/software-distribution.png)

   zip檔案是可直AEM接安裝的軟體包。

1. 從「開始」AEM菜單，導航到 **工具** > **部署** > **包**。
1. 按一下 **上載包** 選擇上一步下載的包。 按一下 **安裝** 安裝軟體包。

   ![安裝GraphiQL軟體包](assets/graphiql/install-graphiql-package.png)

1. 導航到 **CRXDE Lite** > **儲存庫面板** >選擇 `/content/graphiql` 節點(例如， <http://localhost:4502/crx/de/index.jsp#/content/graphiql>)。
1. 在 **屬性** 頁籤更改值 `endpoint` 屬性 `/content/_cq_graphql/wknd-shared/endpoint.json`。
   ![端點屬性值更改](assets/graphiql/endpoint-prop-value-change.png)

1. 導航到 **Web控制台配置** UI >搜索 **CSRF濾波器** 配置(例如，<http://localhost:4502/system/console/configMgr/com.adobe.granite.csrf.impl.CSRFFilter)>
1. 在 `Excluded Paths` 屬性名稱欄位更新，WKNDGraphQL終結點路徑到 `/content/cq:graphql/wknd-shared/endpoint`。

![排除路徑屬性值更改](assets/graphiql/exclude-paths-value-change.png)

1. 使用訪問GraphiQL編輯器 `//HOST:PORT/content/graphiql.html`，並驗證您可以構造新查詢或執行現有查詢。 (例如 <http://localhost:4502/content/graphiql.html>)

![GraphiQL編輯器](assets/graphiql/graphiql-editor.png)

>[!TIP]
>
>要支援特定於項目的GraphQL架構和查詢執行，必須對 `endpoint` 和 `Excluded Paths` 值。
