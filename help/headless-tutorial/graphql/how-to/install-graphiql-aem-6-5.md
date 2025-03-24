---
title: 在AEM 6.5上安裝GraphiQL IDE
description: 瞭解如何在AEM 6.5上安裝和設定GraphiQL IDE
version: Experience Manager 6.5
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
jira: KT-11614
thumbnail: KT-10253.jpeg
exl-id: 04fcc24c-7433-4443-a109-f01840ef1a89
duration: 41
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---

# 在AEM 6.5上安裝GraphiQL IDE

在AEM 6.5中，必須手動安裝GraphiQL IDE工具。

1. 導覽至&#x200B;**[軟體發佈入口網站](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEM as a Cloud Service**。
1. 搜尋「GraphiQL」（請務必在&#x200B;**GraphiQL**&#x200B;中包含&#x200B;**i**）。
1. 下載最新的&#x200B;**GraphiQL Content Package v.x.x.x**。

   ![下載GraphiQL封裝](assets/graphiql/software-distribution.png)

   zip檔案是可直接安裝的AEM套件。

1. 從AEM開始功能表，導覽至&#x200B;**工具** > **部署** > **套件**。
1. 按一下&#x200B;**上傳封裝**，然後選擇在先前步驟中下載的封裝。 按一下&#x200B;**安裝**&#x200B;以安裝封裝。

   ![安裝GraphiQL封裝](assets/graphiql/install-graphiql-package.png)

1. 導覽至&#x200B;**CRXDE Lite** > **存放庫面板** >選取`/content/graphiql`節點（例如<http://localhost:4502/crx/de/index.jsp#/content/graphiql>）。
1. 在&#x200B;**屬性**&#x200B;索引標籤中，將`endpoint`屬性的值變更為`/content/_cq_graphql/wknd-shared/endpoint.json`。
   ![端點屬性值變更](assets/graphiql/endpoint-prop-value-change.png)

1. 瀏覽至&#x200B;**網頁主控台組態** UI >搜尋&#x200B;**CSRF篩選器**&#x200B;組態（例如<http://localhost:4502/system/console/configMgr/com.adobe.granite.csrf.impl.CSRFFilter)>）
1. 在`Excluded Paths`屬性名稱欄位更新中，WKND GraphQL端點路徑為`/content/cq:graphql/wknd-shared/endpoint`。

![排除路徑屬性值變更](assets/graphiql/exclude-paths-value-change.png)

1. 使用`//HOST:PORT/content/graphiql.html`存取GraphiQL編輯器，並確認您可以建構新的查詢或執行現有的查詢。 （例如<http://localhost:4502/content/graphiql.html>）

![GraphiQL編輯器](assets/graphiql/graphiql-editor.png)

>[!TIP]
>
>若要支援您專案特定的GraphQL結構描述和查詢執行，您必須對上述步驟中的`endpoint`和`Excluded Paths`值進行對應的變更。
