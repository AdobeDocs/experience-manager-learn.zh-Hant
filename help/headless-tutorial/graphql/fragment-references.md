---
title: 使用片段參照建立進階資料模型——開始使用AEM Headless - GraphQL
description: 開始使用Adobe Experience Manager(AEM)和GraphQL。 瞭解如何使用「片段參考」功能來建立進階資料模型，以及建立兩個不同內容片段之間的關係。 瞭解如何修改GraphQL查詢以包含參考模型中的欄位。
sub-product: 資產
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6718
thumbnail: KT-6718.jpg
translation-type: tm+mt
source-git-commit: 2ea667d3bdb73fa4da87b877f14db77d896448a7
workflow-type: tm+mt
source-wordcount: '849'
ht-degree: 1%

---


# 使用片段參照建立進階資料模型

>[!CAUTION]
>
> AEM GraphQL API的內容片段傳送將於2021年初發行。
> 相關檔案可供預覽使用。

可以從其他內容片段中參考內容片段。 這可讓使用者建立具有片段間關係的複雜資料模型。

在本章中，您將使用&#x200B;**片段參考**&#x200B;欄位更新「冒險」模型，以包含對「投稿者」模型的參考。 您還將學習如何修改GraphQL查詢以包括引用模型中的欄位。

## 必備條件

這是多部分教學課程，並假設先前各部分中概述的步驟已完成。

## 目標

在本章中，我們將學習如何：

* 更新內容片段模型以使用片段參考欄位
* 建立GraphQL查詢，該查詢從引用的模型返回欄位

## 添加片段引用{#add-fragment-reference}

更新Adventure Content Fragment Model，以新增對Contributor模型的參考。

1. 開啟新瀏覽器並導覽至AEM。
1. 從&#x200B;**AEM Start**&#x200B;功能表導覽至&#x200B;**Tools** > **Assets** > **Content Fragment Models** > **WKND Site**。
1. 開啟&#x200B;**Adventure**&#x200B;內容片段模型

   ![開啟冒險內容片段模型](assets/fragment-references/adventure-content-fragment-edit.png)

1. 在「**資料類型**」下，將&#x200B;**片段引用**&#x200B;欄位拖放到主面板中。

   ![新增片段參考欄位](assets/fragment-references/add-fragment-reference-field.png)

1. 使用以下命令更新此欄位的&#x200B;**屬性**:

   * 呈現為 - `fragmentreference`
   * 欄位標籤- **冒險貢獻者**
   * 屬性名稱 - `adventureContributor`
   * 型號類型——選擇&#x200B;**Contributor**&#x200B;型號
   * 根路徑 - `/content/dam/wknd`

   ![片段參考屬性](assets/fragment-references/fragment-reference-properties.png)

   屬性名稱`adventureContributor`現在可用來參考「投稿者內容片段」。

1. 將更改保存到模型。

## 指派參與者參與冒險

現在「冒險內容片段」模型已經更新，我們可以編輯現有片段並參考「投稿者」。 請注意，編輯內容片段模型&#x200B;*會影響*&#x200B;任何從中建立的現有內容片段。

1. 導覽至&#x200B;**Assets** > **Files** > **WKND Site** > **English** Adventures **>**[ Bali Surf Camp](http://localhost:4502/assets.html/content/dam/wknd/en/adventures/bali-surf-camp)**。**

   ![Bali Surf Camp資料夾](assets/setup/bali-surf-camp-folder.png)

1. 按一下&#x200B;**Bali Surf Camp**&#x200B;內容片段，以開啟內容片段編輯器。
1. 更新&#x200B;**Adventure Contributor**&#x200B;欄位，並按一下資料夾圖示以選取「Contributor」。

   ![選擇Stacey Roswells做貢獻者](assets/fragment-references/stacey-roswell-contributor.png)

   *選擇參與者片段的路徑*

   ![投稿者的填入路徑](assets/fragment-references/populated-path.png)

   請注意，只能選擇使用&#x200B;**Contributor**&#x200B;模型建立的片段。

1. 儲存對片段所做的變更。

1. 重複上述步驟，為[Yosemite Backpacking](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking)和[Colorado Rock Climping](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/colorado-rock-climbing/colorado-rock-climbing)等探險指派貢獻者

## 使用GraphiQL查詢巢狀內容片段

接著，對「冒險」執行查詢，並新增參考「投稿者」模型的巢狀屬性。 我們將使用GraphiQL工具快速驗證查詢的語法。

1. 導覽至AEM中的GraphiQL工具：[http://localhost:4502/content/graphiql.html](http://localhost:4502/content/graphiql.html)

1. 輸入以下查詢：

   ```graphql
   {
     adventureByPath(_path:"/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp") {
        item {
          _path
          adventureTitle
          adventureContributor {
            fullName
            occupation
            pictureReference {
           ...on ImageRef {
             _path
           }
         }
       }
     }
    }
   }
   ```

   上述查詢是指單一Adventure的路徑。 `adventureContributor`屬性會參考Contributor模型，然後我們可以從巢狀內容片段請求屬性。

1. 執行查詢，您應得到如下結果：

   ```json
   {
     "data": {
       "adventureByPath": {
           "item": {
               "_path": "/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp",
               "adventureTitle": "Bali Surf Camp",
               "adventureContributor": {
                   "fullName": "Stacey Roswells",
                   "occupation": "Photographer",
                   "pictureReference": {
                       "_path": "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
                   }
               }
           }
        }
     }
   }
   ```

1. 嘗試其他查詢（例如`adventureList`），並在`adventureContributor`下新增參考內容片段的屬性。

## 更新React應用程式以顯示Contributor內容

接著，更新React Application使用的查詢，加入新的Contributor，並在Adventure詳細資訊檢視中顯示Contributor的相關資訊。

1. 在IDE中開啟WKND GraphQL React應用程式。

1. 開啟檔案`src/components/AdventureDetail.js`。

   ![Adventure Detail元件IDE](assets/fragment-references/adventure-detail-ide.png)

1. 查找函式`adventureDetailQuery(_path)`。 `adventureDetailQuery(..)`函式只會包住篩選GraphQL查詢，此查詢使用AEM的`<modelName>ByPath`語法來查詢由其JCR路徑識別的單一內容片段。

1. 更新查詢以包含有關引用的參與者的資訊：

   ```javascript
   function adventureDetailQuery(_path) {
       return `{
           adventureByPath (_path: "${_path}") {
           item {
               _path
               adventureTitle
               adventureActivity
               adventureType
               adventurePrice
               adventureTripLength
               adventureGroupSize
               adventureDifficulty
               adventurePrice
               adventurePrimaryImage {
                   ... on ImageRef {
                   _path
                   mimeType
                   width
                   height
                   }
               }
               adventureDescription {
                   html
               }
               adventureItinerary {
                   html
               }
               adventureContributor {
                   fullName
                   occupation
                   pictureReference {
                       ...on ImageRef {
                           _path
                       }
                   }
               }
             }
          }
        }
       `;
   }
   ```

   透過此更新，查詢中將包含有關`adventureContributor`、`fullName`、`occupation`和`pictureReference`的其他屬性。

1. 在`function Contributor(...)`的`AdventureDetail.js`檔案中檢查嵌入的`Contributor`元件。 如果屬性存在，此元件將呈現Contributor的名稱、職業和圖片。

   `Contributor`元件在`AdventureDetail(...)` `return`方法中引用：

   ```javascript
   function AdventureDetail(props) {
       ...
       return (
           ...
            <h2>Itinerary</h2>
           <hr />
           <div className="adventure-detail-itinerary"
                dangerouslySetInnerHTML={{__html: adventureData.adventureItinerary.html}}></div>
           {/* Contributor component is instaniated and 
               is passed the adventureContributor object from the GraphQL Query results */}
           <Contributer {...adventureData.adventureContributor} />
           ...
       )
   }
   ```

1. 將變更儲存至檔案。
1. 啟動React App（如果尚未執行）:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm start
   ```

1. 導覽至[http://localhost:3000](http://localhost:3000/)，然後按一下具有參考貢獻者的冒險。 您現在應該會看到&#x200B;**Intrique**&#x200B;下方列出的「參與者」資訊：

   ![應用程式中新增的投稿者](assets/fragment-references/contributor-added-detail.png)

## 其他資源

有關內容片段和GraphQL的詳細資訊，請參閱下列資源：

* [使用內容片段與GraphQL進行無頭內容傳送](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/content-fragments/content-fragments-graphql.html)
* [AEM GraphQL API，用於內容片段](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html)

## 恭喜！{#congratulations}

恭喜！ 您已更新現有的內容片段模型，以使用&#x200B;**片段參考**&#x200B;欄位來參考巢狀內容片段。 您還學習了如何修改GraphQL查詢以包括引用模型中的欄位。
