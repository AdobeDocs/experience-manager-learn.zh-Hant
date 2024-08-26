---
title: 在AEM FormsCloud Service中使用垂直索引標籤
description: 使用垂直索引標籤建立最適化表單
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
thumbnail: 331891.jpg
badgeVersions: label="AEM Formsas a Cloud Service" before-title="false"
jira: KT-16023
source-git-commit: f3f5c4c4349c8d02c88e1cf91dbf18f58db1e67e
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 0%

---

# 在標籤之間導覽

您可以按一下個別標籤，或使用表單上上一個和下一個按鈕，在標籤之間導覽。
若要使用按鈕導覽，請在表單中新增兩個按鈕，並將其命名為「上一步」和「下一步」。 將下列自訂函式與按鈕的點選事件建立關聯，以在標籤之間導覽。

以下是要在標籤之間導覽的自訂函式。



```javascript
/**
 * Navigate in panel with focusOption
 * @name navigateInPanelWithFocusOption
 * @param {object} panelField
 * @param {string} focusOption - values can be 'nextItem'/'previousItem'
 * @param {scope} globals
 */
function navigateInPanelWithFocusOption(panelField, focusOption, globals)
{
    globals.functions.setFocus(panelField, focusOption);
}
```

以下是「下一個」和「上一個」按鈕的規則編輯器

**下一個按鈕**

![下一個按鈕](assets/next-button.png)

**上一個按鈕**

![上一個按鈕](assets/prev-button.png)

