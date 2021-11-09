---
title: 將工具欄的下一個按鈕和上一個按鈕空間
description: 將工具欄按鈕空間
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9291
source-git-commit: 84a0c78f89f78e161b460574b5927fc4aba2fe3a
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# 將工具列按鈕空間

將「下一步」和「上一步」按鈕新增至AEM Forms中的工具列時，預設會將按鈕放置在彼此旁邊。 您可能希望在工具列中將「下一步」按鈕按到極右，同時將「上一步/返回」按鈕保留在左側

![工具欄間距](assets/toolbar-spacing.png)


## 設定工具列的樣式

使用樣式編輯器可輕鬆完成上述使用案例。 將「上/下一步」(Prev/Next)按鈕添加到工具欄後，請確保已從編輯菜單中選擇「樣式」(Style)圖層。 在選取樣式模式後，選取工具列以開啟其樣式屬性工作表。 展開「Dimension和位置」區段，並確認您看到所有屬性。 設定下列屬性
* Dimension和位置
   * 寬度：100%
   * 位置：相對

儲存您的變更

## 設定「下一步」按鈕的樣式

選擇「下一步」按鈕，並確保開啟下一個按鈕的樣式屬性表（而不是下一個按鈕文本）。 設定下列屬性
* Dimension和位置
   * 位置：絕對上1px右1px
* 邊框
   * 邊框半徑：4px（上、右、下、左）

儲存您的變更
