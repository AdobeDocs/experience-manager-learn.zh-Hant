---
title: 內容建模 — AEM無頭第一教程
description: 瞭解如何利用內容片段、建立片段模型以及使用中的GraphQL終AEM點。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
source-git-commit: 12b3888552d5a131628dabf380840f0586798ea5
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 9%

---


# 內容建模

歡迎使用Adobe Experience Manager()中有關內容片段和GraphQL端點的教AEM程章。 我們將介紹利用內容片段、建立片段模型以及使用中的GraphQL端點AEM。

內容片段提供了跨渠道管理內容的結構化方法，提供了靈活性和可重用性。 在中啟用內容片AEM段可以建立模組化內容，增強一致性和適應性。

首先，我們將指導您完成中的內容片段啟用AEM，包括實現無縫整合所需的配置和設定。

接下來，我們將介紹如何建立定義結構和屬性的片段模型。 瞭解如何設計符合您的內容要求的模型並有效管理它們。

然後，我們將演示從模型中建立內容片段，提供有關創作和發佈的逐步指導。

此外，我們將探討定義AEMGraphQL端點。 GraphQL高效地從中AEM檢索資料，我們將設定和配置端點以公開所需資料。 永續查詢將優化效能和快取。

在整個教程中，我們將提供說明、代碼示例和實用提示。 最後，您將具備啟用內容片段、建立片段模型、生成片段以及定義AEMGraphQL端點和永續查詢的技能。 開始吧！

## 上下文感知配置

1. 導航到 __工具>配置瀏覽器__ 建立無頭體驗的配置。

   ![建立資料夾](./assets/1/create-configuration.png)

   提供 __標題__ 和 __名稱__，然後 __GraphQL永續查詢__ 和 __內容片段模型__。


## 內容片段模型

1. 導航到 __「工具」>「內容片段模型」__ 並選擇具有步驟1中建立的配置名稱的資料夾。

   ![模型資料夾](./assets/1/model-folder.png)

1. 在資料夾內，選擇 __建立__ 並命名模型 __預告__。 將以下資料類型添加到 __預告__ 模型。

   | 資料類型 | 名稱 | 必要 | 選項 |
   |----------|------|----------|---------|
   | 內容參考 | 資產 | 是 | 如果需要，請添加預設影像。 例如：/content/dam/wknd-headless/assets/AdobeStock_307513975.mp4 |
   | 單行文字 | 標題 | 是 |
   | 單行文字 | 預標題 | 否 |
   | 多行文字 | 說明 | 否 | 確保預設類型為RTF |
   | 列舉 | 樣式 | 是 | 呈現為下拉清單。 選項為「英雄」 — >「英雄」和「特色」 — > |

   ![Teaser模型](./assets/1/teaser-model.png)

1. 在資料夾內，建立名為 __優惠__。 按一下「建立」(create)，並為模型指定「提供」(Offer)名稱，然後添加以下資料類型：

   | 資料類型 | 名稱 | 必要 | 選項 |
   |----------|------|----------|---------|
   | 內容參考 | 資產 | 是 | 添加預設影像。 例如：`/content/dam/wknd-headless/assets/AdobeStock_238607111.jpeg` |
   | 多行文字 | 說明 | 否 |  |
   | 多行文字 | 文章 | 否 |  |

   ![服務模式](./assets/1/offer-model.png)

1. 在資料夾內，建立名為 __影像清單__。 按一下「建立」(Create)，並為模型指定「影像清單」(Image List)名稱，然後添加以下資料類型：

   | 資料類型 | 名稱 | 必要 | 選項 |
   |----------|------|----------|---------|
   | 片段參考 | 清單項目 | 是 | 呈現為多個欄位。 允許的內容片段模型為「提供」。 |

   ![影像清單模型](./assets/1/imagelist-model.png)

## 內容片段

1. 現在導航到「資產」並為新站點建立資料夾。 按一下建立並命名資料夾。

   ![添加資料夾](./assets/1/create-folder.png)

1. 建立資料夾後，選擇該資料夾並開啟它 __屬性__。
1. 在 __雲配置__ 頁籤，選擇配置 [建立早](#enable-content-fragments-and-graphql)。

   ![資產資料夾AEM無頭雲配置](./assets/1/cloud-config.png)

   按一下新資料夾並建立預告。 按一下 __建立__ 和 __內容片段__ 的 __預告__ 模型。 命名模型 __英雄__ 按一下 __建立__。

   | 名稱 | 附註 |
   |----------|------|
   | 資產 | 保留為預設值或選擇其他資產（視頻或影像） |
   | 標題 | `Explore. Discover. Live.` |
   | 預標題 | `Join use for your next adventure.` |
   | 說明 | 留空 |
   | 樣式 | `Hero` |

   ![英雄片](./assets/1/teaser-model.png)

## GraphQL 端點

1. 導航到 __工具>GraphQL__

   ![圖形AEMQL](./assets/1/endpoint-nav.png)

1. 按一下 __建立__ 並為新端點指定名稱並選擇新建立的配置。

   ![無AEM頭GraphQL端點](./assets/1/endpoint.png)

## GraphQL 持續性查詢

1. 讓我們test新端點。 導航到 __工具>GraphQL查詢編輯器__ 並為窗口右上角的下拉清單選擇我們的端點。

1. 在查詢編輯器中，建立幾個不同的查詢。


   ```graphql
   {
       teaserList {
           items {
           title
           }
       }
   }
   ```

   您應該獲得包含已建立的單個片段的清單 [上](#create-content)。

   對於本練習，請建立無頭應用使用的AEM完整查詢。 建立按路徑返回單個預告的查詢。 在查詢編輯器中，輸入以下查詢：

   ```graphql
   query TeaserByPath($path: String!) {
   component: teaserByPath(_path: $path) {
       item {
       __typename
       _path
       _metadata {
           stringMetadata {
           name
           value
           }
       }
       title
       preTitle
       style
       asset {
           ... on MultimediaRef {
           __typename
           _authorUrl
           _publishUrl
           format
           }
           ... on ImageRef {
           __typename
           _authorUrl
           _publishUrl
           mimeType
           width
           height
           }
       }
       description {
           html
           plaintext
       }
       }
   }
   }
   ```

   在 __查詢變數__ 在底部輸入，輸入：

   ```json
   {
       "path": "/content/dam/pure-headless/hero"
   }
   ```

   >[!NOTE]
   >
   > 您可能需要調整查詢變數 `path` 基於資料夾和片段名稱。


   運行查詢以接收先前建立的內容片段的結果。

1. 按一下 __保存__  保留（保存）查詢並命名查詢 __偏激器__。 這允許我們按應用程式中的名稱引用查詢。

## 後續步驟

恭喜！您已成功配置AEMas a Cloud Service，以允許建立內容片段和GraphQL端點。 您還建立了內容片段模型和內容片段，並定義了GraphQL終結點和永續查詢。 現在，您已準備好進入下一教程章節，在該章節中您將學習如何建立AEMHeadless React應用程式，該應用程式佔用您在本章中建立的內容片段和GraphQL終結點。

[下一章：無AEM頭API和反應](./2-aem-headless-apis-and-react.md)