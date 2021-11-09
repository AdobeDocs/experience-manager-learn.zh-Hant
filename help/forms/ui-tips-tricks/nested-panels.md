---
title: 導覽巢狀面板
description: 導覽巢狀面板
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9335
source-git-commit: 84a0c78f89f78e161b460574b5927fc4aba2fe3a
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 0%

---

# 多個面板的導覽標籤

當您的表單有左側導覽標籤，且其中一個標籤有多個面板時，您可能會想要隱藏子面板的標題，但仍可在這些標籤和子面板之間導覽

[此功能以此形式上線](https://forms.enablementadobe.com/content/forms/af/testnav1.html)




## 建立最適化表單

使用下列結構建立最適化表單。 根面板的子面板在左側顯示為標籤。 其中一些「**標籤**」有其他子面板。 例如，「家庭」頁簽有兩個子面板，稱為「配偶」和「子項」。
FormContainer下還添加了一個工具欄，其中包含「前置」和「下一步」按鈕

![工具欄間距](assets/multiple-panels.png)



此表單的預設行為是顯示左側的所有面板，然後按一下下一個按鈕，從一個索引標籤導覽至另一個標籤。

若要變更此預設行為，我們必須執行下列動作

>[!VIDEO](https://video.tv.adobe.com/v/338369?quality=9&learn=on)


將下列程式碼新增至 **下一個** 按鈕（使用代碼編輯器）

```javascript
window.guideBridge.setFocus(null, 'nextItemDeep', true);
```

將下列程式碼新增至 **Prev** 按鈕（使用代碼編輯器）

```javascript
window.guideBridge.setFocus(null, 'prevItemDeep', true);
```

上述程式碼可協助您在各個標籤的標籤與子面板之間導覽。

## 隱藏子面板標題

使用樣式編輯器隱藏頁簽子面板的標題。

>[!VIDEO](https://video.tv.adobe.com/v/338370?quality=9&learn=on)

>[!NOTE]
> 本文所述的功能在最後一個索引標籤中無法運作。 例如，如果「位址」標籤有子面板，則此功能無法運作。