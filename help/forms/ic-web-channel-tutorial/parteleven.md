---
title: 設定投資組合面板
seo-title: 設定投資組合面板
description: 這是建立您第一個互動式通訊檔案的多步驟教學課程第11部分。在本部分，我們將新增圓形圖，以顯示目前和模型的投資組合。
seo-description: 這是建立您第一個互動式通訊檔案的多步驟教學課程第11部分。在本部分，我們將新增圓形圖，以顯示目前和模型的投資組合。
uuid: b0132912-cb6e-4dec-8309-5125d29ad291
feature: 互動式通訊
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 77de4e76-53ee-467c-a71c-d1d3ea15283b
topic: 開發
role: 開發人員
level: 初學者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 1%

---


# 設定投資組合面板

在此部分，我們將新增圓形圖來顯示目前和模型的投資組合。

* 登入AEM Forms並導覽至「Adobe Experience Manager>Forms>Forms與檔案」。

* 開啟401KStatement資料夾。

* 在編輯模式中開啟401KStatement。

* 我們將新增2張圓形圖，以代表目前及模型的帳戶持有人投資組合。

## 流動資產組合{#current-asset-mix}

* 點選右側的「CurrentAssetMix」面板，並選取「+」圖示並插入文字元件。 將預設文字變更為「目前的資產組合」。

* 點選「CurrentAssetMix」面板，並選取「+」圖示並插入圖表元件。 點選新插入的圖表元件，然後按一下「扳手」圖示以開啟圖表的設定屬性工作表。

* 如下圖所示設定屬性。 請確定您的圖表類型為圓形圖。

* 請注意系結至X和Y軸的資料模型物件。 您需要選取表單資料模型的根元素，然後下鑽以選取適當的元素。

* ![currentassetmix](assets/currentassetmixchart.png)

## 模型資產組合{#model-asset-mix}

* 點選右側的「RecommendedAssetMix」面板，並選取「+」圖示並插入文字元件。 將預設文字變更為「模型資產混合」。

* 點選「RecommendedAssetMix」面板，並選取「+」圖示並插入圖表元件。 點選新插入的圖表元件，然後按一下「扳手」圖示以開啟圖表的設定屬性工作表。

* 如下圖所示設定屬性。 請確定您的圖表類型為圓形圖。

* 請注意系結至X和Y軸的資料模型物件。 您需要選取表單資料模型的根元素，然後下鑽以選取適當的元素。

* ![assettype](assets/modelassettypechart.png)

