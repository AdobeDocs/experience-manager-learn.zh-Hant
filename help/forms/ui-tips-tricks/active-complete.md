---
title: 使用圖示設定左側導覽標籤的樣式
description: 新增圖示以指示活動和已完成的索引標籤
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9359
exl-id: f7c1f991-0486-4355-8502-cd5b038537e3
last-substantial-update: 2019-07-07T00:00:00Z
duration: 111
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '328'
ht-degree: 14%

---

# 新增圖示以指示活動和已完成的索引標籤

當您有附有左側索引標籤導覽的最適化表單時，您可能想要顯示圖示來指示索引標籤的狀態。 例如，您想要顯示一個圖示來指示作用中的標籤，而圖示來指示完成的標籤，如下方熒幕擷取所示。

![工具列間距](assets/active-completed.png)

## 建立最適化表單

範例表單是使用以基本範本和畫布3.0主題為基礎的簡單調適型表單來建立。
此 [本文中使用的圖示](assets/icons.zip) 可從這裡下載。


## 設定預設狀態的樣式

在編輯模式中開啟表單確定您在樣式圖層中，並選取任何標籤（例如「一般」標籤）。
開啟標籤的樣式編輯器時，您處於預設狀態，如下方熒幕擷取畫面所示
![導覽標籤](assets/navigation-tab.png)

設定預設狀態的CSS屬性，如下所示 |類別 |屬性名稱 |屬性值 | |：—|：—|：—| |Dimension和位置 |寬度 | 50px | |文字 |字型粗細|粗體 | |文字 |色彩 |#FFF | |文字 |行高| 3 | |文字 |文字對齊 |左側 | |背景|色彩 |#056dae |

儲存您的變更

## 設定使用中狀態的樣式

請確定您處於作用中狀態，並設定下列CSS屬性的樣式

| 類別 | 屬性名稱 | 屬性值 |
|:---|:---|:---|
| Dimension和位置 | 寬度 | 50px |
| 文字 | 字粗 | 粗體 |
| 文字 | 彩色 | #FFF |
| 文字 | 行高 | 3 |
| 文字 | 文字對齊 | 左 |
| 背景 | 彩色 | #056dae |

設定背景影像的樣式，如下方熒幕擷取畫面所示

儲存您的變更。



![active-state](assets/active-state.png)

## 設定造訪狀態的樣式

請確定您處於已造訪狀態，並設定下列屬性的樣式

| 類別 | 屬性名稱 | 屬性值 |
|:---|:---|:---|
| Dimension和位置 | 寬度 | 50px |
| 文字 | 字粗 | 粗體 |
| 文字 | 彩色 | #FFF |
| 文字 | 行高 | 3 |
| 文字 | 文字對齊 | 左 |
| 背景 | 彩色 | #056dae |

設定背景影像的樣式，如下方熒幕擷取畫面所示


![造訪狀態](assets/visited-state.png)

儲存您的變更

預覽表單並測試圖示是否如預期運作。
