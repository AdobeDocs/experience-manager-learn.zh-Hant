---
title: 編寫內容片段- AEM Headless快速入門- GraphQL
description: 開始使用Adobe Experience Manager(AEM)和GraphQL。 根據內容片段模型建立和編輯新的內容片段。 瞭解如何建立內容片段的變體。
sub-product: 資產
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6713
thumbnail: 22451.jpg
translation-type: tm+mt
source-git-commit: 8c5b425e6dcf23cbef042097f17db9e51bdf63c9
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 0%

---


# 編寫內容片段{#authoring-content-fragments}

在本章中，您將根據[新定義的參與者內容片段模型](./content-fragment-models.md)來建立和編輯新的內容片段。 您也將學習如何建立內容片段的變體。

## 必備條件 {#prerequisites}

這是多部分教學課程，假定[定義內容片段模型](./content-fragment-models.md)中概述的步驟已完成。

## 目標{#objectives}

* 基於內容片段模型製作內容片段
* 建立內容片段變數

## 內容片段製作概述{#overview}

>[!VIDEO](https://video.tv.adobe.com/v/22451/?quality=12&learn=on)

上述影片提供製作內容片段的高階概觀。

## 建立內容片段{#create-content-fragment}

在上一章[定義內容片段模型](./content-fragment-models.md)中，建立了&#x200B;**貢獻者**&#x200B;模型。 使用此模型製作新的內容片段。

1. 從&#x200B;**AEM Start**&#x200B;功能表導覽至&#x200B;**Assets** > **Files**。
1. 按一下資料夾以導航至&#x200B;**WKND站點** > **英文** > **參與者**。 此資料夾包含WKND品牌「投稿者」的頭照清單。

1. 按一下右上角的&#x200B;**建立**&#x200B;並選擇&#x200B;**內容片段**:

   ![按一下「建立新片段」](assets/author-content-fragments/create-content-fragment-menu.png)

1. 選擇&#x200B;**Contributor**&#x200B;型號，然後按一下&#x200B;**Next**。

   ![選擇參與者模型](assets/author-content-fragments/select-contributor-model.png)

   這與前一章中建立的&#x200B;**Contributor**&#x200B;模型相同。

1. 輸入&#x200B;**Stacey Roswells**&#x200B;作為標題，然後按一下&#x200B;**Create**。
1. 在&#x200B;**Success**&#x200B;對話方塊中按一下&#x200B;**Open**，以開啟新建立的片段。

   ![已建立新內容片段](assets/author-content-fragments/new-content-fragment.png)

   請注意，模型定義的欄位現在可用來製作此內容片段例項。

1. 對於&#x200B;**全名**，請輸入：**Stacey Roswells**。
1. 對於&#x200B;**傳記**，請輸入簡短的傳記。 需要靈感嗎？ 您可以重新使用此[文字檔](assets/author-content-fragments/stacey-roswells-bio.txt)。
1. 對於&#x200B;**圖片參考**，按一下&#x200B;**資料夾**&#x200B;表徵圖並瀏覽至&#x200B;**WKND站點** > **英文** > **參與者** > **stacey-roswells.jpg**... 這將評估路徑：`/content/dam/wknd/en/contributors/stacey-roswells.jpg`。
1. 對於&#x200B;**佔領**&#x200B;選擇&#x200B;**攝影師**。

   ![編寫的片段](assets/author-content-fragments/stacye-roswell-fragment-authored.png)

1. 按一下&#x200B;**保存**&#x200B;保存更改。

## 建立內容片段變數

所有內容片段都以&#x200B;**Master**&#x200B;變數開頭。 **Master**&#x200B;變體可視為片段的&#x200B;*default*&#x200B;內容，並在通過GraphQL API公開內容時自動使用。 也可以建立內容片段的變體。 此功能為設計實施提供了額外的靈活性。

變體可用於特定通道。 例如，可以建立&#x200B;**mobile**&#x200B;變體，該變體包含較少的文本或引用特定於通道的影像。 如何使用變體，完全取決於實施。 與任何功能一樣，在使用前應仔細規劃。

接下來，建立一個新變體，瞭解可用功能。

1. 重新開啟&#x200B;**Stacey Roswells**&#x200B;內容片段。
1. 在左側滑軌中，按一下「Create Variation **（建立變體**）」。
1. 在&#x200B;**新建變體**&#x200B;模式中，輸入「摘要」的標題&#x200B;**。**

   ![新建變體 — 摘要](assets/author-content-fragments/new-variation-summary.png)

1. 按一下&#x200B;**傳記**&#x200B;多行欄位，然後按一下&#x200B;**展開**&#x200B;按鈕以輸入多行欄位的全屏視圖。

   ![輸入全屏視圖](assets/author-content-fragments/enter-full-screen-view.png)

1. 按一下右上角菜單中的「匯總文本&#x200B;**」。**

1. 輸入&#x200B;******50**&#x200B;單詞的&#x200B;**目標，然後按一下「開始**」。

   ![摘要預覽](assets/author-content-fragments/summarize-text-preview.png)

   這將開啟摘要預覽。 AEM的機器語言處理器將嘗試根據目標詞數來總結文本。 您也可以選擇不同的句子刪除。

1. 如果您對摘要很滿意，請按一下「摘要」。 ****&#x200B;按一下多行文本欄位並切換&#x200B;**展開**&#x200B;按鈕以返回主視圖。

1. 按一下&#x200B;**保存**&#x200B;保存更改。

## 建立其他內容片段

重複[建立內容片段](#create-content-fragment)中概述的步驟，以建立附加的&#x200B;**參與者**。 在下一章中，將使用此示例來說明如何查詢多個片段。

1. 在&#x200B;**「參與者**」資料夾中，按一下右上角的&#x200B;**建立**&#x200B;並選擇&#x200B;**內容片段**:
1. 選擇&#x200B;**Contributor**&#x200B;模型，然後按一下&#x200B;**Next**。
1. 輸入&#x200B;**Jacob Wester**&#x200B;作為標題，然後按一下&#x200B;**建立**。
1. 按一下&#x200B;**成功**&#x200B;對話框中的&#x200B;**開啟**&#x200B;以開啟新建立的片段。
1. 對於&#x200B;**全名**，請輸入：**雅各布·韋斯特**。
1. 對於&#x200B;**傳記**，請輸入簡短傳記。 需要點靈感嗎？ 您可以重新使用此[文本檔案](assets/author-content-fragments/jacob-wester.txt)。
1. 對於&#x200B;**圖片參考**，按一下&#x200B;**資料夾**&#x200B;表徵圖並瀏覽至&#x200B;**WKND站點** **英語** **參與者** > **jacob_wester.jpg**。 這將根據路徑計算：`/content/dam/wknd/en/contributors/jacob_wester.jpg`。
1. 對於&#x200B;**佔領**，選擇&#x200B;**書寫器**。
1. 按一下&#x200B;**保存**&#x200B;保存更改。 除非您願意，否則無需建立變體！

   ![其他內容片段](assets/author-content-fragments/additional-content-fragment.png)

   您現在應該有兩個&#x200B;**參與者**&#x200B;片段。

## 恭喜！{#congratulations}

恭喜，您剛剛創作了多個內容片段並建立了變體。

## 後續步驟{#next-steps}

在下一章[瀏覽GraphQL API](explore-graphql-api.md)中，您將使用內置的GrapiQL工具瀏覽AEM的GraphQL API。 瞭解AEM如何基於內容片段模型自動生成GraphQL架構。 您將嘗試使用GraphQL語法構建基本查詢。