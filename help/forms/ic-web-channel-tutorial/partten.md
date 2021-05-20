---
title: 配置報廢展望面板
seo-title: 配置報廢展望面板
description: 這是建立第一個互動式通訊檔案的多步驟教學課程的10部分。 在本部分中，我們將通過添加文本和圖表元件來配置Retimution Outlook面板。
seo-description: 這是建立第一個互動式通訊檔案的多步驟教學課程的10部分。 在本部分中，我們將通過添加文本和圖表元件來配置Retimution Outlook面板。
uuid: 1d5119b5-e797-4bf0-9b10-995b3f051f92
feature: 互動式通訊
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 2ee2cea2-aefa-4d21-a258-248648f73a68
topic: 開發
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 1%

---


# 配置報廢Outlook面板{#configuring-retirement-outlook-panel}

* 這是建立第一個互動式通訊檔案的多步驟教學課程的10部分。 在本部分中，我們將通過添加文本和圖表元件來配置Retimution Outlook面板。

* 登入AEM Forms並導覽至「Adobe Experience Manager > Forms > Forms與檔案」。

* 開啟401KStatement資料夾。

* 在編輯模式下開啟401KStatement文檔。

**配置左面板目標區域**

* 點選右側的「左面板」目標區域，然後按一下「+」圖示以開啟「插入元件」對話方塊。

* 插入文本元件。

* 輕輕點選新增的文字元件，以開啟元件工具列

* 選取「鉛筆」圖示以編輯預設文字。

* 將預設文本替換為「**Your Retimeting Incement Outlook」**

**配置RightPanel目標區域**

* 點選右側的「RightPanel」目標區域，然後按一下「+」圖示以開啟「插入元件」對話方塊。

* 插入文本元件。

* 輕輕點選新增的文字元件，以開啟元件工具列。

* 選取「鉛筆」圖示以編輯預設文字。

* 將預設文本替換為「**估計每月退休收入」**

## 添加退休收入展望文檔片段{#add-retirement-income-outlook-document-fragment}

* 按一下「資產」圖示，然後套用篩選器以顯示「檔案片段」類型的資產。 將RetirementIncomeOutlook文檔片段拖放到「左面板」目標區域。

* 有關將文檔片段添加到內容區域，可以將[引用到此頁](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-web-channel-aem-forms/9.html)。

## 添加估計月收入圖{#adding-estimated-monthly-income-chart}

* 按一下右側的「RightPanel」目標區域。 按一下「+」圖示以插入圖表元件。 我們將用一個柱圖來顯示月收入估計值。 輕輕點選新插入的圖表元件。 選擇「扳手」表徵圖以開啟「配置屬性」工作表。使用以下螢幕截圖所示的屬性配置圖表。

**AEM Forms 6.4 — 設定預計月收入欄圖**

![form64](assets/estimatedmonthlyincomechart.png)

**AEM Forms 6.5 — 設定預計月收入欄圖**

![forms65](assets/estimatedmonthlyincomechart65.PNG)




