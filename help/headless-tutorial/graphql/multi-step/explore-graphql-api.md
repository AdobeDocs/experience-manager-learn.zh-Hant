---
title: 瀏覽GraphQLAPI — 無頭入門AEM-GraphQL
description: 從Adobe Experience Manager和AEMGraphQL開始。 使AEM用內置的GrapiQL IDE瀏覽GraphQLAPI。 瞭解如AEM何基於內容片段模型自動生成GraphQL架構。 使用GraphQL語法構造基本查詢的實驗。
version: Cloud Service
mini-toc-levels: 1
kt: 6714
thumbnail: KT-6714.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 508b0211-fa21-4a73-b8b4-c6c34e3ba696
source-git-commit: ae27cbc50fc5c4c2e8215d7946887b99d480d668
workflow-type: tm+mt
source-wordcount: '1454'
ht-degree: 2%

---

# 瀏覽GraphQLAPI {#explore-graphql-apis}

GraphQLAPIAEM提供了功能強大的查詢語言，將內容片段的資料公開到下游應用程式。 內容片段模型定義內容片段使用的資料架構。 無論何時建立或更新內容片段模型，都會轉換該模式並將其添加到構成GraphQLAPI的「圖形」中。

在本章中，我們來探討一些常見的GraphQL查詢，以使用名為 [圖形QL](https://github.com/graphql/graphiql)。 GraphiQL IDE允許您快速test和細化返回的查詢和資料。 它還提供了對文檔的輕鬆訪問，使您能夠輕鬆瞭解和瞭解可用的方法。

## 必備條件 {#prerequisites}

這是一個多部分教程，並假定在 [創作內容片段](./author-content-fragments.md) 已完成。

## 目標 {#objectives}

* 學習使用GraphiQL工具使用GraphQL語法構造查詢。
* 瞭解如何查詢內容片段和單個內容片段的清單。
* 瞭解如何篩選和請求特定資料屬性。
* 瞭解如何加入多個內容片段模型的查詢
* 瞭解如何保留GraphQL查詢。

## 啟用 GraphQL 端點 {#enable-graphql-endpoint}

必須配置GraphQL終結點以啟用GraphQLAPI內容片段查詢。

1. 從「開始AEM」螢幕導航到 **工具** > **常規** > **GraphQL**。

   ![導航到GraphQL終結點](assets/explore-graphql-api/navigate-to-graphql-endpoint.png)

1. 點擊 **建立** 在右上角的結果對話框中，輸入以下值：

   * 名稱*: **我的項目終結點**。
   * 使用由……提供的GraphQL架構*: **我的項目**

   ![建立GraphQL終結點](assets/explore-graphql-api/create-graphql-endpoint.png)

   點擊 **建立** 的子菜單。

   基於項目配置建立的GraphQL端點僅對屬於該項目的模型啟用查詢。 在本例中，僅對 **人員** 和 **團隊** 可以使用模型。

   >[!NOTE]
   >
   > 還可以建立全局端點，以啟用對多個配置中的模型的查詢。 應謹慎使用此選項，因為它可能會使環境暴露於其他安全漏洞，並增加管理的總體復AEM雜性。

1. 現在，您應看到在您的環境中啟用了一個GraphQL終結點。

   ![已啟用grapql端點](assets/explore-graphql-api/enabled-graphql-endpoints.png)

## 使用 GraphiQL IDE

的 [圖形QL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html) 工具使開發人員能夠針對當前環境中的內容建立和testAEM查詢。 GraphiQL工具還使用戶能夠 **保留或保存** 要由生產設定中的客戶端應用程式使用的查詢。

接下來，使用內置AEMGraphiQL IDE瞭解GraphQLAPI的功能。

1. 從「開始AEM」螢幕導航到 **工具** > **常規** > **GraphQL查詢編輯器**。

   ![導航到GraphiQL IDE](assets/explore-graphql-api/navigate-graphql-query-editor.png)

   >[!NOTE]
   >
   > 在中， GraphiQL IDE的AEM舊版本可能未內置。 可以在這些之後手動安裝 [說明](#install-graphiql)。

1. 在右上角，確保端點設定為 **我的項目終結點**。

   ![設定GraphQL端點](assets/explore-graphql-api/set-my-project-endpoint.png)

這將將所有查詢範圍限定為在 **我的項目** 項目。

### 查詢內容片段清單 {#query-list-cf}

一個常見要求是查詢多個內容片段。

1. 在主面板中貼上以下查詢（替換注釋清單）:

   ```graphql
   query allTeams {
     teamList {
       items {
         _path
         title
       }
     }
   } 
   ```

1. 按 **播放** 按鈕。 您應看到上一章中內容片段的結果：

   ![人員清單結果](assets/explore-graphql-api/all-teams-list.png)

1. 將游標置於 `title` 文本和輸入 **CTRL+空格鍵** 觸發代碼提示。 添加 `shortname` 和 `description` 的子菜單。

   ![更新包含代碼的查詢](assets/explore-graphql-api/update-query-codehinting.png)

1. 通過按 **播放** 按鈕，您應該看到結果包括 `shortname` 和 `description`。

   ![短名稱和描述結果](assets/explore-graphql-api/updated-query-shortname-description.png)

   的 `shortname` 是一個簡單的屬性 `description` 是多行文本欄位，GraphQLAPI允許我們為結果選擇各種格式，例如 `html`。 `markdown`。 `json`或 `plaintext`。

### 查詢嵌套片段

接下來，查詢實驗是檢索嵌套片段，回想 **團隊** 模型參照 **人員** 模型。

1. 更新查詢以包括 `teamMembers` 屬性。 記住，這是 **片段引用** 欄位。 可以返回人員模型的屬性：

   ```graphql
   query allTeams {
       teamList {
           items {
               _path
               title
               shortName
               description {
                   plaintext
               }
               teamMembers {
                   fullName
                   occupation
               }
           }
       }
   }
   ```

   JSON響應：

   ```json
   {
       "data": {
           "teamList": {
           "items": [
               {
               "_path": "/content/dam/my-project/en/team-alpha",
               "title": "Team Alpha",
               "shortName": "team-alpha",
               "description": {
                   "plaintext": "This is a description of Team Alpha!"
               },
               "teamMembers": [
                   {
                   "fullName": "John Doe",
                   "occupation": [
                       "Artist",
                       "Influencer"
                   ]
                   },
                   {
                   "fullName": "Alison Smith",
                   "occupation": [
                       "Photographer"
                   ]
                   }
                 ]
           }
           ]
           }
       }
   }
   ```

   對嵌套片段進行查詢的能力是GraphQLAPI的強AEM大功能。 在此簡單示例中，嵌套只有兩層深。 然而，碎片可能會更遠。 例如，如果 **地址** 與 **人員** 可以在單個查詢中返回所有三個模型的資料。

### 篩選內容片段清單 {#filter-list-cf}

接下來，讓我們看看如何根據屬性值將結果篩選為內容片段的子集。

1. 在GraphiQL UI中輸入以下查詢：

   ```graphql
   query personByName($name:String!){
     personList(
       filter:{
         fullName:{
           _expressions:[{
             value:$name
             _operator:EQUALS
           }]
         }
       }
     ){
       items{
         _path
         fullName
         occupation
       }
     }
   }  
   ```

   上述查詢對系統中的所有Person片段執行搜索。 添加到查詢開頭的篩選器對 `name` 欄位和變數字串 `$name`。

1. 在 **查詢變數** 面板輸入以下內容：

   ```json
   {"name": "John Doe"}
   ```

1. 執行查詢，應僅 **人** 返回內容片段，其值為 `John Doe`。

   ![使用查詢變數篩選](assets/explore-graphql-api/using-query-variables-filter.png)

   有許多其他選項可用於篩選和建立複雜查詢，請參見 [學習將GraphQL與AEM樣例內容和查詢一起使用](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/sample-queries.html)。

1. 增強上述查詢以提取配置檔案圖片

   ```graphql
   query personByName($name:String!){
     personList(
       filter:{
         fullName:{
           _expressions:[{
             value:$name
             _operator:EQUALS
           }]
         }
       }
     ){
       items{  
         _path
         fullName
         occupation
         profilePicture{
           ... on ImageRef{
             _path
             _authorUrl
             _publishUrl
             height
             width
   
           }
         }
       }
     }
   } 
   ```

   的 `profilePicture` 是內容引用，它應是影像，因此內置 `ImageRef` 對象。 這允許我們請求有關正在引用的影像的附加資料，如 `width` 和 `height`。

### 查詢單個內容片段 {#query-single-cf}

也可以直接查詢單個內容片段。 中的內AEM容以分層方式儲存，並且片段的唯一標識符基於片段的路徑。

1. 在GraphiQL編輯器中輸入以下查詢：

   ```graphql
   query personByPath($path: String!) {
       personByPath(_path: $path) {
           item {
           fullName
           occupation
           }
       }
   }
   ```

1. 為 **查詢變數**:

   ```json
   {"path": "/content/dam/my-project/en/alison-smith"}
   ```

1. 執行查詢，並觀察是否返回單個結果。

## 保留查詢 {#persist-queries}

一旦開發人員對從查詢返回的查詢和結果資料感到滿意，下一步就是將查詢儲存或保留到AEM。 的 [永續查詢](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html) 是將GraphQLAPI暴露給客戶端應用程式的首選機制。 一旦查詢被永續，就可以使用GET請求來請求它，並在Dispatcher和CDN層快取。 永續查詢的效能要好得多。 除了效能優勢外，永續查詢還可確保額外資料不會意外暴露到客戶端應用程式。 有關 [可在此處找到永續查詢](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html)。

接下來，保留兩個簡單的查詢，它們將在下一章中使用。

1. 在GraphiQL IDE中輸入以下查詢：

   ```graphql
   query allTeams {
       teamList {
           items {
               _path
               title
               shortName
               description {
                   plaintext
               }
               teamMembers {
                   fullName
                   occupation
               }
           }
       }
   }
   ```

   驗證查詢是否有效。

1. 下一次點擊 **另存為** 輸入 `all-teams` 的 **查詢名稱**。

   查詢應顯示在 **永續查詢** 左欄。

   ![所有團隊永續查詢](assets/explore-graphql-api/all-teams-persisted-query.png)
1. 接下來點擊橢圓 **...** 在永久查詢旁邊，點擊 **複製URL** 將路徑複製到剪貼簿。

   ![複製永久查詢URL](assets/explore-graphql-api/copy-persistent-query-url.png)

1. 開啟新頁籤，然後在瀏覽器中貼上複製的路徑：

   ```plain
   https://$YOUR-AEMasCS-INSTANCEID$.adobeaemcloud.com/graphql/execute.json/my-project/all-teams
   ```

   它應該與上面的路徑類似。 您應看到查詢的JSON結果返回。

   拆分上述URL:

   | 名稱 | 說明 |
   | ---------|---------- |
   | `/graphql/execute.json` | 永久查詢終結點 |
   | `/my-project` | 項目配置 `/conf/my-project` |
   | `/all-teams` | 永續查詢的名稱 |

1. 返回到GraphiQL IDE並使用加號按鈕 **+** 保留NEW查詢

   ```graphql
   query personByName($name: String!) {
     personList(
       filter: {
         fullName:{
           _expressions: [{
             value: $name
             _operator:EQUALS
           }]
         }
       }){
       items {
         _path
         fullName
         occupation
         biographyText {
           json
         }
         profilePicture {
           ... on ImageRef {
             _path
             _authorUrl
             _publishUrl
             width
             height
           }
         }
       }
     }
   }
   ```

1. 將查詢另存為： `person-by-name`。
1. 您應保存兩個永續查詢：

   ![最終永續查詢](assets/explore-graphql-api/final-persisted-queries.png)


## 發佈GraphQL終結點和永續查詢

經審閱和驗證後，發佈 `GraphQL Endpoint` &amp; `Persisted Queries`

1. 從「開始AEM」螢幕導航到 **工具** > **常規** > **GraphQL**。

1. 按一下旁邊的複選框 **我的項目終結點** 點擊 **發佈**

   ![發佈GraphQL終結點](assets/explore-graphql-api/publish-graphql-endpoint.png)

1. 從「開始AEM」螢幕導航到 **工具** > **常規** > **GraphQL查詢編輯器**

1. 點擊 **全團隊** 從「永續查詢」面板中查詢並點擊 **發佈**

   ![發佈永續查詢](assets/explore-graphql-api/publish-persisted-query.png)

1. 重複上述步驟 `person-by-name` 查詢

## 解決方案檔案 {#solution-files}

下載在前三章中建立的內容、模型和永續查詢： [教程 — 解決方案 — content.zip](assets/explore-graphql-api/tutorial-solution-content.zip)

## 其他資源

瞭解有關GraphQL查詢的詳細資訊，請訪問 [學習將GraphQL與AEM樣例內容和查詢一起使用](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/sample-queries.html)。

## 恭喜！ {#congratulations}

恭喜，您建立並執行了多個GraphQL查詢！

## 後續步驟 {#next-steps}

在下一章， [生成反應應用](./graphql-and-react-app.md)，您將瞭解外部應用程式如何查詢AEMGraphQL端點並使用這兩個永續查詢。 在執行GraphQL查詢期間，還將介紹一些基本錯誤處理。

## 安裝GraphiQL工具（可選） {#install-graphiql}

在中，需要手AEM動安裝(6.X.X)的某些版本的GraphiQL IDE工具，請使用 [此處的說明](../how-to/install-graphiql-aem-6-5.md)。

