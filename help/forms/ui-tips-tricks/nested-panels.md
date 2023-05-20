---
title: 導航嵌套面板
description: 導航嵌套面板
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9335
exl-id: c60d019e-da26-4f67-8579-ef707e2348bb
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '247'
ht-degree: 0%

---

# 具有多個面板的導航頁籤

當窗體具有左導航頁籤且其中一個頁籤具有多個面板時，您可能希望隱藏子面板的標題，並且仍然能夠在這些頁籤的頁籤和子面板之間導航

## 建立自適應窗體

建立具有以下結構的自適應窗體。 根面板具有子面板，這些子面板在左側顯示為頁籤。 其中一些&quot;**頁籤**」有其他子面板。 例如，「家庭」頁籤有兩個名為「配偶」和「子代」的子面板。

還在FormContainer下添加一個工具欄，其中包含「上一個」和「下一個」按鈕

![工具欄間距](assets/multiple-panels.png)



此表單的預設行為是在左側顯示所有面板，然後按一下下一個按鈕從一個頁籤導航到另一個頁籤。

要更改此預設行為，我們需要執行以下操作

>[!VIDEO](https://video.tv.adobe.com/v/338369?quality=12&learn=on)


將以下代碼添加到 **下一個** 按鈕

```javascript
window.guideBridge.setFocus(null, 'nextItemDeep', true);
```

將以下代碼添加到 **上** 按鈕

```javascript
window.guideBridge.setFocus(null, 'prevItemDeep', true);
```

上述代碼將幫助您在每個頁籤的頁籤和子面板之間導航。

## 隱藏子面板標題

使用樣式編輯器隱藏制表符子面板的標題。

>[!VIDEO](https://video.tv.adobe.com/v/338370?quality=12&learn=on)

>[!NOTE]
>
>本文中描述的功能在最後一個頁籤中不起作用。 例如，如果「地址」頁籤具有子面板，則此功能將不起作用。
