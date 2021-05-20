---
title: 製作內容片段 — AEM無周邊功能快速入門 — GraphQL
description: 開始使用Adobe Experience Manager(AEM)和GraphQL。 根據內容片段模型建立和編輯新內容片段。 了解如何建立內容片段的變體。
sub-product: 資產
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6713
thumbnail: 22451.jpg
feature: 內容片段、GraphQL API
topic: 無頭式、內容管理
role: Developer
level: Beginner
source-git-commit: 81626b8d853f3f43d9c51130acf02561f91536ac
workflow-type: tm+mt
source-wordcount: '792'
ht-degree: 0%

---


# 製作內容片段{#authoring-content-fragments}

在本章中，您將根據[新定義的貢獻者內容片段模型](./content-fragment-models.md)建立和編輯新的內容片段。 您也會學習如何建立內容片段的變體。

## 必備條件 {#prerequisites}

這是多部分教學課程，假設[定義內容片段模型](./content-fragment-models.md)中概述的步驟已完成。

## 目標 {#objectives}

* 根據內容片段模型製作內容片段
* 建立內容片段變數

## 內容片段製作概述 {#overview}

>[!VIDEO](https://video.tv.adobe.com/v/22451/?quality=12&learn=on)

上述影片提供製作內容片段的概觀。

## 建立內容片段{#create-content-fragment}

在前一章的[定義內容片段模型](./content-fragment-models.md)中，建立了&#x200B;**貢獻者**&#x200B;模型。 使用此模型製作新的內容片段。

1. 從&#x200B;**AEM Start**&#x200B;功能表導覽至&#x200B;**Assets** > **Files**。
1. 按一下資料夾以導覽至&#x200B;**WKND Site** > **English** > **Contributors**。 此資料夾包含WKND品牌貢獻者的頭部擷取畫面清單。

1. 按一下右上角的&#x200B;**建立**&#x200B;並選取&#x200B;**內容片段**:

   ![按一下建立新片段](assets/author-content-fragments/create-content-fragment-menu.png)

1. 選取&#x200B;**貢獻者**&#x200B;模型，然後按一下&#x200B;**下一步**。

   ![選取貢獻者模型](assets/author-content-fragments/select-contributor-model.png)

   這與上一章中建立的&#x200B;**貢獻者**&#x200B;模型相同。

1. 為標題輸入&#x200B;**Stacey Roswells**，然後按一下&#x200B;**Create**。
1. 在&#x200B;**Success**&#x200B;對話方塊中，按一下&#x200B;**開啟**&#x200B;以開啟新建立的片段。

   ![已建立新內容片段](assets/author-content-fragments/new-content-fragment.png)

   請注意，模型定義的欄位現在可供製作此內容片段例項。

1. 對於&#x200B;**全名**，請輸入：**Stacey Roswells**。
1. **傳記**&#x200B;輸入簡短傳記。 需要點靈感嗎？ 歡迎重新使用此[文本檔案](assets/author-content-fragments/stacey-roswells-bio.txt)。
1. 對於&#x200B;**圖片引用**，按一下&#x200B;**資料夾**&#x200B;表徵圖並瀏覽到&#x200B;**WKND站點** > **英文** > **貢獻者** > **stacey-roswells.jpg**。 這會評估路徑：`/content/dam/wknd/en/contributors/stacey-roswells.jpg`。
1. 對於&#x200B;**佔領**，選擇&#x200B;**攝影師**。

   ![製作片段](assets/author-content-fragments/stacye-roswell-fragment-authored.png)

1. 按一下&#x200B;**儲存**&#x200B;以儲存變更。

## 建立內容片段變數

所有內容片段的開頭皆為&#x200B;**主版**&#x200B;變數。 **主**&#x200B;變數可視為片段的&#x200B;*預設*&#x200B;內容，並在透過GraphQL API公開內容時自動使用。 您也可以建立內容片段的變體。 此功能為設計實作提供額外的彈性。

變數可用來鎖定特定通道。 例如，可以建立&#x200B;**mobile**&#x200B;變數，其中包含較少的文字量或參考通道特定影像。 變數的使用方式取決於實作。 與任何功能一樣，使用前應先謹慎規劃。

接下來，建立新的變數，以了解可用的功能。

1. 重新開啟&#x200B;**Stacey Roswells**&#x200B;內容片段。
1. 在左側邊欄中，按一下「建立變異」**。**
1. 在&#x200B;**New Variation**&#x200B;強制回應中，輸入&#x200B;**Summary**&#x200B;的標題。

   ![新變化 — 摘要](assets/author-content-fragments/new-variation-summary.png)

1. 按一下&#x200B;**Berium**&#x200B;多行欄位，然後按一下&#x200B;**Expand**&#x200B;按鈕，輸入多行欄位的全螢幕視圖。

   ![輸入全螢幕視圖](assets/author-content-fragments/enter-full-screen-view.png)

1. 按一下右上角功能表中的&#x200B;**摘要文字** 。

1. 輸入&#x200B;******50**&#x200B;字的目標，然後按一下&#x200B;**開始**。

   ![摘要預覽](assets/author-content-fragments/summarize-text-preview.png)

   這將開啟總結預覽。 AEM電腦語言處理器將嘗試根據目標字數來匯總文本。 您也可以選取要移除的不同句子。

1. 對總結滿意時，按一下&#x200B;**匯總**。 按一下多行文本欄位，並切換&#x200B;**Expand**&#x200B;按鈕以返回主視圖。

1. 按一下&#x200B;**儲存**&#x200B;以儲存變更。

## 建立其他內容片段

重複[建立內容片段](#create-content-fragment)中概述的步驟，以建立額外的&#x200B;**貢獻者**。 這將用於下一章，作為如何查詢多個片段的範例。

1. 在&#x200B;**貢獻者**&#x200B;資料夾中，按一下右上方的&#x200B;**建立**&#x200B;並選取&#x200B;**內容片段**:
1. 選取&#x200B;**貢獻者**&#x200B;模型，然後按一下&#x200B;**下一步**。
1. 為標題輸入&#x200B;**Jacob Wester**，然後按一下&#x200B;**Create**。
1. 在&#x200B;**Success**&#x200B;對話方塊中，按一下&#x200B;**開啟**&#x200B;以開啟新建立的片段。
1. 對於&#x200B;**全名**，請輸入：**雅各布·韋斯特**。
1. **傳記**&#x200B;輸入簡短傳記。 需要點靈感嗎？ 歡迎重新使用此[文本檔案](assets/author-content-fragments/jacob-wester.txt)。
1. 對於&#x200B;**圖片引用**，按一下&#x200B;**資料夾**&#x200B;表徵圖並瀏覽到&#x200B;**WKND站點** > **英文** > **貢獻者** > **jacob_wester.**。 這會評估路徑：`/content/dam/wknd/en/contributors/jacob_wester.jpg`。
1. 對於&#x200B;**佔領**，選擇&#x200B;**書寫器**。
1. 按一下&#x200B;**儲存**&#x200B;以儲存變更。 除非您想要，否則不需要建立變異！

   ![其他內容片段](assets/author-content-fragments/additional-content-fragment.png)

   您現在應該有兩個&#x200B;**貢獻者**&#x200B;片段。

## 恭喜！ {#congratulations}

恭喜，您剛撰寫了多個內容片段，並建立了變體。

## 後續步驟{#next-steps}

在下一章[探索GraphQL API](explore-graphql-api.md)中，您將使用內建的GrapiQL工具探索AEM GraphQL API。 了解AEM如何根據內容片段模型自動產生GraphQL架構。 您將嘗試使用GraphQL語法來建構基本查詢。