---
title: 使用片段參考功能進階資料模型 — AEM無周邊功能快速入門 — GraphQL
description: 開始使用Adobe Experience Manager(AEM)和GraphQL。 了解如何將片段參考功能用於進階資料模型，以及如何建立兩個不同內容片段之間的關係。 了解如何修改GraphQL查詢以包含參考模型的欄位。
sub-product: 資產
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6718
thumbnail: KT-6718.jpg
feature: 內容片段、GraphQL API
topic: 無頭式、內容管理
role: Developer
level: Beginner
source-git-commit: 81626b8d853f3f43d9c51130acf02561f91536ac
workflow-type: tm+mt
source-wordcount: '855'
ht-degree: 1%

---


# 使用片段參考的進階資料模型

可以從其他內容片段內參考內容片段。 這可讓使用者建立具有片段之間關係的複雜資料模型。

在本章中，您將更新冒險模型，使用&#x200B;**片段參考**&#x200B;欄位包含對貢獻者模型的參考。 您也將學習如何修改GraphQL查詢以包含參考模型的欄位。

## 必備條件

本教學課程分為多部分，假設已完成前幾部分所述的步驟。

## 目標

在本章中，我們將學習如何：

* 更新內容片段模型以使用片段參考欄位
* 建立GraphQL查詢，該查詢會從引用的模型返回欄位

## 新增片段參考{#add-fragment-reference}

更新冒險內容片段模型以新增參考至貢獻者模型。

1. 開啟新瀏覽器並導覽至AEM。
1. 從&#x200B;**AEM開始**&#x200B;功能表導覽至&#x200B;**工具** > **資產** > **內容片段模型** > **WKND網站**。
1. 開啟&#x200B;**Adventure**&#x200B;內容片段模型

   ![開啟冒險內容片段模型](assets/fragment-references/adventure-content-fragment-edit.png)

1. 在&#x200B;**資料類型**&#x200B;下，將&#x200B;**片段參考**&#x200B;欄位拖放至主面板。

   ![新增片段參考欄位](assets/fragment-references/add-fragment-reference-field.png)

1. 使用以下內容更新此欄位的&#x200B;**屬性**:

   * 呈現為 - `fragmentreference`
   * 欄位標籤 — **冒險貢獻者**
   * 屬性名稱 - `adventureContributor`
   * 模型類型 — 選取&#x200B;**貢獻者**&#x200B;模型
   * 根路徑 - `/content/dam/wknd`

   ![片段參考屬性](assets/fragment-references/fragment-reference-properties.png)

   屬性名稱`adventureContributor`現在可以用來參考貢獻者內容片段。

1. 保存對模型的更改。

## 為冒險指派貢獻者

現在「冒險內容片段」模型已更新，我們可以編輯現有片段並參考貢獻者。 請注意，編輯內容片段模型&#x200B;*會影響*&#x200B;從中建立的任何現有內容片段。

1. 導覽至&#x200B;**Assets** > **Files** > **WKND Site** > **English** > **Adventures** > **[Bali Surf Camp](http://localhost:4502/assets.html/content/dam/wknd/en/adventures/bali-surf-camp)**。

   ![巴釐島衝浪營資料夾](assets/setup/bali-surf-camp-folder.png)

1. 按一下進入&#x200B;**Bali Surf Camp**&#x200B;內容片段，以開啟內容片段編輯器。
1. 更新&#x200B;**Adventure Contributor**&#x200B;欄位，並按一下資料夾圖示以選取Contributor 。

   ![選擇Stacey Roswells作為貢獻者](assets/fragment-references/stacey-roswell-contributor.png)

   *選取貢獻者片段的路徑*

   ![貢獻者的填入路徑](assets/fragment-references/populated-path.png)

   請注意，只能選取使用&#x200B;**貢獻者**&#x200B;模型建立的片段。

1. 儲存對片段的變更。

1. 重複上述步驟，為[Yosemite Backpacking](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking)和[Colorado Rock Climping](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/colorado-rock-climbing/colorado-rock-climbing)等歷險指派貢獻者

## 使用GraphiQL查詢巢狀內容片段

接下來，對探險項執行查詢，並新增所參考貢獻者模型的巢狀屬性。 我們將使用GraphiQL工具快速驗證查詢的語法。

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

   上述查詢是針對路徑上的單一Adventure。 `adventureContributor`屬性會參考貢獻者模型，然後我們就可以從巢狀內容片段中請求屬性。

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

1. 嘗試其他查詢，例如`adventureList`，並在`adventureContributor`下新增參考內容片段的屬性。

## 更新React應用程式以顯示貢獻者內容

接下來，更新React應用程式使用的查詢，加入新的貢獻者，並在冒險詳細資訊檢視中顯示貢獻者的相關資訊。

1. 在IDE中開啟WKND GraphQL React應用程式。

1. 開啟檔案`src/components/AdventureDetail.js`。

   ![冒險詳細資訊元件IDE](assets/fragment-references/adventure-detail-ide.png)

1. 查找函式`adventureDetailQuery(_path)`。 `adventureDetailQuery(..)`函式只會包住篩選GraphQL查詢，該查詢使用AEM `<modelName>ByPath`語法來查詢以其JCR路徑識別的單一內容片段。

1. 更新查詢以包含所參考貢獻者的相關資訊：

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

1. Inspect`AdventureDetail.js`檔案中內嵌`Contributor`元件（位於`function Contributor(...)`）。 如果屬性存在，此元件將呈現貢獻者的名稱、職業和圖片。

   `AdventureDetail(...)` `return`方法中引用了`Contributor`元件：

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

1. 儲存檔案的變更。
1. 啟動React應用程式（如果尚未執行）:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm start
   ```

1. 導覽至[http://localhost:3000](http://localhost:3000/)，然後按一下參照了貢獻者的冒險。 您現在應該會看到&#x200B;**Interial**&#x200B;下方列出的貢獻者資訊：

   ![應用程式中新增的貢獻者](assets/fragment-references/contributor-added-detail.png)

## 恭喜！{#congratulations}

恭喜！ 您已更新現有內容片段模型，以使用&#x200B;**片段參考**&#x200B;欄位參考巢狀內容片段。 您還學習了如何修改GraphQL查詢以包括引用模型中的欄位。

## 後續步驟{#next-steps}

在下一章中，[使用AEM Publish環境進行生產部署](./production-deployment.md) ，了解AEM製作和發佈服務，以及無頭應用程式的建議部署模式。 您將更新現有應用程式，以使用環境變數根據目標環境動態更改GraphQL終結點。 您也將學習如何正確設定AEM以進行跨原始資源共用(CORS)。
