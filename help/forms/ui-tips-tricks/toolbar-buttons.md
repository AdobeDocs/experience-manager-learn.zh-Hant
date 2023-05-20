---
title: 將工具欄的下一個和上一個按鈕空間
description: 將工具欄按鈕空間
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9291
exl-id: 1b55b6d2-3bab-4907-af89-c81a3b1a44cb
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# 使工具欄按鈕空間

將「下一步」(Next)和「上一步」(Prev)按鈕添加到AEM Forms的工具欄時，預設情況下，這些按鈕會彼此相鄰。 您可能希望在將「上一步/後退」按鈕保留在左側的同時，按工具欄中的「下一步」按鈕至右側

![工具欄間距](assets/toolbar-spacing.png)


## 樣式工具欄

使用樣式編輯器可輕鬆完成上述使用情形。 將「上一步/下一步」(Prev/Next)按鈕添加到工具欄後，請確保已從「編輯」(edit)菜單中選擇「樣式」(Style)圖層。 選擇樣式模式後，選擇工具欄以開啟其樣式屬性工作表。 展開「Dimension和位置」部分，確保看到所有屬性。 設定以下屬性
* Dimension和職位
   * 寬度：100%
   * 職位：相對

保存更改

## 設定「下一個」按鈕的樣式

選擇「下一個」按鈕，並確保開啟下一個按鈕的樣式屬性表（而不是下一個按鈕文本）。 設定以下屬性
* Dimension和職位
   * 職位：絕對前1px右1px
* 邊框
   * 邊框半徑：4px（上、右、下、左）

保存更改
