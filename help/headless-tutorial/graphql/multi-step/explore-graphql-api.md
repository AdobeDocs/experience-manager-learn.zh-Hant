---
title: 探索GraphQL API - AEM Headless快速入門 — GraphQL
description: 開始使用Adobe Experience Manager (AEM)和GraphQL。 使用內建GrapiQL IDE來探索AEM的GraphQL API。 瞭解AEM如何根據內容片段模型自動產生GraphQL結構描述。 嘗試使用GraphQL語法建構基本查詢。
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
jira: KT-6714
thumbnail: KT-6714.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 508b0211-fa21-4a73-b8b4-c6c34e3ba696
duration: 332
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1416'
ht-degree: 1%

---

# 探索GraphQL API {#explore-graphql-apis}

AEM的GraphQL API提供強大的查詢語言，以將內容片段的資料公開給下游應用程式。 內容片段模型會定義內容片段所使用的資料結構。 每當建立或更新內容片段模型時，都會翻譯結構描述並將其新增到組成GraphQL API的「圖表」。

在本章中，讓我們探索一些常見的GraphQL查詢，以使用名為[GraphiQL](https://github.com/graphql/graphiql)的IDE來收集內容。 GraphiQL IDE可讓您快速測試並調整傳回的查詢和資料。 也能讓您輕鬆存取檔案，讓您輕鬆學習和瞭解有哪些方法可用。

## 先決條件 {#prerequisites}

此教學課程包含多個部分，並假設已完成[編寫內容片段](./author-content-fragments.md)中概述的步驟。

## 目標 {#objectives}

* 瞭解如何使用GraphiQL工具來使用GraphQL語法建構查詢。
* 瞭解如何查詢內容片段清單和單一內容片段。
* 瞭解如何篩選及請求特定資料屬性。
* 瞭解如何聯結多個內容片段模型的查詢
* 瞭解如何保留GraphQL查詢。

## 啟用 GraphQL 端點 {#enable-graphql-endpoint}

必須將GraphQL端點設定為啟用內容片段的GraphQL API查詢。

1. 從AEM開始畫面瀏覽至&#x200B;**工具** > **一般** > **GraphQL**。

   ![瀏覽至GraphQL端點](assets/explore-graphql-api/navigate-to-graphql-endpoint.png)

1. 點選右上角的&#x200B;**建立**，在產生的對話方塊中輸入下列值：

   * 名稱*： **我的專案端點**。
   * 使用由提供的GraphQL結構描述…… *： **我的專案**

   ![建立GraphQL端點](assets/explore-graphql-api/create-graphql-endpoint.png)

   點選&#x200B;**建立**&#x200B;以儲存端點。

   根據專案設定建立的GraphQL端點只會針對屬於該專案的模型啟用查詢。 在此情況下，只能使用針對&#x200B;**人員**&#x200B;和&#x200B;**團隊**&#x200B;模型的查詢。

   >[!NOTE]
   >
   > 也可以建立全域端點以啟用跨多個設定的模型查詢。 使用此方式時請務必謹慎，因為這麼做可能會使環境面臨更多安全性漏洞，並增加AEM管理的整體複雜性。

1. 您現在應該會看到環境上啟用一個GraphQL端點。

   ![已啟用graphql端點](assets/explore-graphql-api/enabled-graphql-endpoints.png)

## 使用 GraphiQL IDE

[GraphiQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html)工具可讓開發人員針對目前AEM環境中的內容建立和測試查詢。 GraphiQL工具也可讓使用者&#x200B;**儲存或儲存**&#x200B;查詢，以供使用者端應用程式在生產設定中使用。

接下來，使用內建GraphiQL IDE來探索AEM的GraphQL API功能。

1. 從AEM開始畫面瀏覽至&#x200B;**工具** > **一般** > **GraphQL查詢編輯器**。

   ![瀏覽至GraphiQL IDE](assets/explore-graphql-api/navigate-graphql-query-editor.png)

   >[!NOTE]
   >
   > 在中，舊版AEM可能未內建GraphiQL IDE。 您可以依照這些[指示](#install-graphiql)手動安裝。

1. 在右上角，確定端點已設為&#x200B;**我的專案端點**。

   ![設定GraphQL端點](assets/explore-graphql-api/set-my-project-endpoint.png)

這會將所有查詢的範圍設定為在&#x200B;**我的專案**&#x200B;專案中建立的模型。

### 查詢內容片段清單 {#query-list-cf}

常見的需求是查詢多個內容片段。

1. 將下列查詢貼到主面板中（取代註解清單）：

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

1. 按頂端功能表中的&#x200B;**播放**&#x200B;按鈕以執行查詢。 您應該會看到上一章內容片段的結果：

   ![個人清單結果](assets/explore-graphql-api/all-teams-list.png)

1. 將游標置於`title`文字下方，並輸入&#x200B;**CTRL+Space**&#x200B;以觸發程式碼提示。 新增`shortname`和`description`至查詢。

   ![更新查詢，代碼為](assets/explore-graphql-api/update-query-codehinting.png)

1. 按&#x200B;**播放**&#x200B;按鈕再次執行查詢，您應該會看到結果包含`shortname`和`description`的其他屬性。

   ![簡短名稱和說明結果](assets/explore-graphql-api/updated-query-shortname-description.png)

   `shortname`是簡單屬性，`description`是多行文字欄位，GraphQL API可讓我們為結果選擇各種格式，例如`html`、`markdown`、`json`或`plaintext`。

### 查詢巢狀片段

接下來，嘗試查詢正在擷取巢狀片段，請記得&#x200B;**團隊**&#x200B;模型參考了&#x200B;**人員**&#x200B;模型。

1. 更新查詢以包含`teamMembers`屬性。 記得這是人員模型的&#x200B;**片段參考**&#x200B;欄位。 可傳回「人員」模型的屬性：

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

   JSON回應：

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

   查詢巢狀片段的功能是AEM GraphQL API的強大功能。 在這個簡單範例中，巢狀結構只有兩層深。 不過，也可以進一步巢狀內嵌片段。 例如，如果有&#x200B;**位址**&#x200B;模型與&#x200B;**人員**&#x200B;相關聯，則可以在單一查詢中傳回所有三個模型的資料。

### 篩選內容片段清單 {#filter-list-cf}

接下來，讓我們看看如何根據屬性值將結果篩選為內容片段子集。

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

   上述查詢會針對系統中的所有人員片段執行搜尋。 新增至查詢開頭的篩選器會對`name`欄位和變數字串`$name`執行比較。

1. 在&#x200B;**查詢變數**&#x200B;面板中輸入下列內容：

   ```json
   {"name": "John Doe"}
   ```

1. 執行查詢，預期只傳回&#x200B;**Person**&#x200B;內容片段值為`John Doe`。

   ![使用查詢變數來篩選](assets/explore-graphql-api/using-query-variables-filter.png)

   有許多其他選項可用來篩選及建立複雜查詢，請參閱[瞭解如何搭配AEM使用GraphQL — 範例內容和查詢](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/sample-queries.html)。

1. 增強上述查詢以擷取設定檔圖片

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

   `profilePicture`是內容參考，而且預期是影像，因此使用內建`ImageRef`物件。 這可讓我們請求有關所參考影像的其他資料，例如`width`和`height`。

### 查詢單一內容片段 {#query-single-cf}

也可以直接查詢單一內容片段。 AEM中的內容以階層方式儲存，片段的唯一識別碼取決於片段的路徑。

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

1. 為&#x200B;**查詢變數**&#x200B;輸入下列內容：

   ```json
   {"path": "/content/dam/my-project/en/alison-smith"}
   ```

1. 執行查詢並觀察是否傳回單一結果。

## 保留查詢 {#persist-queries}

一旦開發人員滿意從查詢傳回的查詢和結果資料，下一步就是儲存或保留查詢至AEM。 [持續查詢](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html)是將GraphQL API公開給使用者端應用程式的偏好機制。 當查詢持續存在後，即可使用GET請求來請求它，並在Dispatcher和CDN層進行快取。 持久查詢的效能要好得多。 除了效能優勢外，持續查詢還可確保不會意外向使用者端應用程式顯示額外資料。 有關[持續查詢的更多詳細資訊，請參閱此處](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html)。

接下來，保留兩個簡單查詢，它們會在下一個章節中使用。

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

1. 接著點選&#x200B;**另存新檔**，並輸入`all-teams`作為&#x200B;**查詢名稱**。

   查詢應該顯示在左側邊欄的&#x200B;**持續查詢**&#x200B;下。

   ![所有團隊持續查詢](assets/explore-graphql-api/all-teams-persisted-query.png)
1. 接著點選永久查詢旁邊的省略符號&#x200B;**...**，然後點選&#x200B;**複製URL**&#x200B;以將路徑複製到剪貼簿。

   ![複製持續查詢URL](assets/explore-graphql-api/copy-persistent-query-url.png)

1. 開啟新索引標籤，並在瀏覽器中貼上複製的路徑：

   ```plain
   https://$YOUR-AEMasCS-INSTANCEID$.adobeaemcloud.com/graphql/execute.json/my-project/all-teams
   ```

   它應該看起來與上述路徑類似。 您應該會看到傳回的查詢JSON結果。

   劃分上述URL：

   | 名稱 | 描述 |
   | ---------|---------- |
   | `/graphql/execute.json` | 持久查詢端點 |
   | `/my-project` | `/conf/my-project`的專案組態 |
   | `/all-teams` | 持久查詢的名稱 |

1. 返回GraphiQL IDE並使用加號按鈕&#x200B;**+**&#x200B;保留NEW查詢

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

1. 將查詢儲存為： `person-by-name`。
1. 您應該已儲存兩個持續查詢：

   ![最後一個持續查詢](assets/explore-graphql-api/final-persisted-queries.png)


## 發佈GraphQL端點與持續查詢

檢閱和驗證後，發佈`GraphQL Endpoint`和`Persisted Queries`

1. 從AEM開始畫面瀏覽至&#x200B;**工具** > **一般** > **GraphQL**。

1. 點選「**我的專案端點**」旁的核取方塊，然後點選「**發佈**」

   ![發佈GraphQL端點](assets/explore-graphql-api/publish-graphql-endpoint.png)

1. 從AEM開始畫面瀏覽至&#x200B;**工具** > **一般** > **GraphQL查詢編輯器**

1. 從「持續查詢」面板點選&#x200B;**所有團隊**&#x200B;查詢，然後點選&#x200B;**發佈**

   ![發佈持續查詢](assets/explore-graphql-api/publish-persisted-query.png)

1. 對`person-by-name`查詢重複上述步驟

## 解決方案檔案 {#solution-files}

下載在前三章中建立的內容、模型和持續查詢： [basic-tutorial-solution.content.zip](assets/explore-graphql-api/basic-tutorial-solution.content.zip)

## 其他資源

若要深入瞭解GraphQL查詢，請前往[學習如何搭配AEM使用GraphQL — 範例內容和查詢](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/sample-queries.html)。

## 恭喜！ {#congratulations}

恭喜，您已建立並執行數個GraphQL查詢！

## 後續步驟 {#next-steps}

在下一章[建置React應用程式](./graphql-and-react-app.md)中，您將探索外部應用程式如何查詢AEM的GraphQL端點，並使用這兩個持續性查詢。 此外，還向您介紹GraphQL查詢執行期間的一些基本錯誤處理方式。

## 安裝GraphiQL工具（選用） {#install-graphiql}

在中，某些版本的AEM (6.X.X) GraphiQL IDE工具需要手動安裝，請從這裡[&#128279;](../how-to/install-graphiql-aem-6-5.md)使用指示。

