---
title: 配置投資組合面板
seo-title: Configuring Investment Mix Panel
description: 這是建立第一個互動式通信文檔的多步驟教程的第11部分。在本部分，我們將添加餅圖以顯示當前和模型投資組合。
seo-description: This is part 11 of multistep tutorial for creating your first interactive communications document.In this part, we will add pie charts to display the current and model investment mix.
uuid: b0132912-cb6e-4dec-8309-5125d29ad291
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 77de4e76-53ee-467c-a71c-d1d3ea15283b
topic: Development
role: Developer
level: Beginner
exl-id: 774d7a6e-2b8f-4a70-98c5-e7712478ff75
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%

---

# 配置投資組合面板

在本部分，我們將添加餅圖來顯示當前和模型投資組合。

* 登入AEM Forms並導覽至「Adobe Experience Manager > Forms > Forms與檔案」。

* 開啟401KStatement資料夾。

* 在編輯模式下開啟401KStatement。

* 我們將添加2個餅圖，以表示賬戶持有人當前和模型投資組合。

## 流動資產組合 {#current-asset-mix}

* 點選右側的「CurrentAssetMix」面板，然後選取「+」圖示並插入文字元件。 將預設文字變更為「目前的資產組合」。

* 點選「CurrentAssetMix」面板，然後選取「+」圖示並插入圖表元件。 點選新插入的圖表元件，然後按一下「扳手」圖示，開啟圖表的設定屬性工作表。

* 設定屬性，如下圖所示。 請確定圖表類型為圓形圖。

* 請注意綁定到X和Y軸的資料模型對象。 您需要選取表單資料模型的根元素，然後向下切入以選取適當的元素。

* ![currentassetmix](assets/currentassetmixchart.png)

## 模型資產組合 {#model-asset-mix}

* 點選右側的「RecommendedAssetMix」面板，然後選取「+」圖示並插入文字元件。 將預設文字變更為「模型資產組合」。

* 點選「RecommendedAssetMix」面板，然後選取「+」圖示並插入圖表元件。 點選新插入的圖表元件，然後按一下「扳手」圖示，開啟圖表的設定屬性工作表。

* 設定屬性，如下圖所示。 請確定圖表類型為圓形圖。

* 請注意綁定到X和Y軸的資料模型對象。 您需要選取表單資料模型的根元素，然後向下切入以選取適當的元素。

* ![assettype](assets/modelassettypechart.png)

## 後續步驟

[準備傳送Web頻道檔案](./parttwelve.md)
