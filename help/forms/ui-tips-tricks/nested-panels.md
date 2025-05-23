---
title: 導覽巢狀面板
description: 導覽巢狀面板
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9335
exl-id: c60d019e-da26-4f67-8579-ef707e2348bb
last-substantial-update: 2019-07-07T00:00:00Z
duration: 264
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '247'
ht-degree: 0%

---

# 具有多個面板的導覽標籤

當您的表單有左側導覽標籤，並且其中一個標籤有多個面板時，您可能想要隱藏子面板的標題，並且仍然能夠在這些標籤的標籤和子面板之間導覽

## 建立最適化表單

建立具有下列結構的最適化表單。 根面板具有子面板，這些子面板在左側顯示為索引標籤。 其中部分「**標籤**」具有其他子面板。 例如，「家庭」標籤有兩個名為「配偶」和「子女」的子項面板。

也會在FormContainer下方新增一個工具列，其中包含「上一個」和「下一個」按鈕

![工具列間距](assets/multiple-panels.png)



此表單的預設行為是顯示左側的所有面板，然後按一下「下一步」按鈕從一個標籤導覽至另一個標籤。

若要變更此預設行為，我們需要執行以下操作

>[!VIDEO](https://video.tv.adobe.com/v/3438642?quality=12&learn=on&captions=chi_hant)


使用程式碼編輯器在&#x200B;**下一步**&#x200B;按鈕的點選事件中新增下列程式碼

```javascript
window.guideBridge.setFocus(null, 'nextItemDeep', true);
```

使用程式碼編輯器在&#x200B;**Prev**&#x200B;按鈕的點選事件中新增下列程式碼

```javascript
window.guideBridge.setFocus(null, 'prevItemDeep', true);
```

上述程式碼可協助您在每個標籤的標籤與子面板之間導覽。

## 隱藏子面板標題

使用樣式編輯器來隱藏索引標籤子面板的標題。

>[!VIDEO](https://video.tv.adobe.com/v/3439141?quality=12&learn=on&captions=chi_hant)

>[!NOTE]
>
>本文所述的功能在最後一個標籤中無法運作。 例如，如果「地址」標籤有子面板，則此功能無法運作。
