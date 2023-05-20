---
title: 使用表徵圖對左側導航頁籤進行樣式
description: 添加表徵圖以指示活動和已完成的頁籤
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9359
exl-id: f7c1f991-0486-4355-8502-cd5b038537e3
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 15%

---

# 添加表徵圖以指示活動和已完成的頁籤

當具有帶左頁籤導航的自適應表單時，您可能希望顯示表徵圖以指示頁籤的狀態。 例如，您希望顯示一個表徵圖來指示活動頁籤，而表徵圖來指示已完成的頁籤，如下面的螢幕快照所示。

![工具欄間距](assets/active-completed.png)

## 建立最適化表單

使用基本模板和Canvas 3.0主題的簡單自適應表單建立示例表單。
的 [本文中使用的表徵圖](assets/icons.zip) 可從此下載。


## 設定預設狀態的樣式

在編輯模式下開啟表單確保您位於樣式層中並選擇任何頁籤（例如「常規」頁籤）。
開啟頁籤的樣式編輯器時，您處於預設狀態，如下面螢幕抓圖所示
![導航頁籤](assets/navigation-tab.png)

為預設狀態設定CSS屬性，如下所示 |類別 |屬性名稱 |屬性值 | |:—|:—| |Dimension和位置 |寬度 | 50px | |文本 |字型粗細|粗體 | |文本 |顏色 | #FFF | |文本 |行高| 3 | |文本 |文本對齊 |左 | |背景|顏色 | #056dae |

保存更改

## 設定活動狀態的樣式

確保處於「活動」狀態，並設定以下CSS屬性的樣式

| 類別 | 屬性名稱 | 屬性值 |
|:---|:---|:---|
| Dimension和職位 | 寬度 | 50px |
| 文字 | 字粗 | 粗體 |
| 文字 | 彩色 | #FFF |
| 文字 | 行高 | 3 |
| 文字 | 文字對齊 | 左 |
| 背景 | 彩色 | #056dae |

如下面螢幕抓圖所示，設定背景影像的樣式

保存更改。



![活動狀態](assets/active-state.png)

## 設定訪問狀態的樣式

確保您處於已訪問狀態，並設定以下屬性的樣式

| 類別 | 屬性名稱 | 屬性值 |
|:---|:---|:---|
| Dimension和職位 | 寬度 | 50px |
| 文字 | 字粗 | 粗體 |
| 文字 | 彩色 | #FFF |
| 文字 | 行高 | 3 |
| 文字 | 文字對齊 | 左 |
| 背景 | 彩色 | #056dae |

如下面螢幕抓圖所示，設定背景影像的樣式


![訪問狀態](assets/visited-state.png)

保存更改

預覽表單並test表徵圖正按預期方式工作。
