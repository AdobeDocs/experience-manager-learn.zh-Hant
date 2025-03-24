---
title: 空白顯示工具列的下一個和上一個按鈕
description: 空白顯示工具列按鈕
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9291
exl-id: 1b55b6d2-3bab-4907-af89-c81a3b1a44cb
last-substantial-update: 2019-07-07T00:00:00Z
duration: 39
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 0%

---

# 空白顯示工具列按鈕

當您在AEM Forms的工具列中新增「下一個」和「上一個」按鈕時，依照預設，這些按鈕會彼此相鄰。 您可能想要將「下一步」按鈕推到工具列的最右側，同時保留左側的「上一步/上一步」按鈕

![工具列間距](assets/toolbar-spacing.png)


## 設定工具列樣式

使用樣式編輯器可輕鬆完成上述使用案例。 將「上一個/下一個」按鈕新增至工具列後，請確定您已從「編輯」選單中選取「樣式」圖層。 選取樣式模式後，選取工具列以開啟其樣式屬性表。 展開「維度和位置」區段，確認您看到所有屬性。 設定下列屬性
* 尺寸與位置
   * 寬度：100%
   * 位置：相對

儲存您的變更

## 設定下一個按鈕的樣式

選取「下一步」按鈕，並確定您開啟了下一個按鈕的樣式特性表（不是下一個按鈕文字）。 設定下列屬性
* 尺寸與位置
   * position： absolute Top 1px Right 1px
* 邊框
   * 邊框半徑：4px（上、右、下、左）

儲存您的變更
