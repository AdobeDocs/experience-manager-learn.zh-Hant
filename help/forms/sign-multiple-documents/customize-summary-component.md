---
title: 自定義摘要元件
description: 擴展摘要步驟元件以包括導航到包中下一個表單的功能。
feature: Adaptive Forms
version: 6.4,6.5
kt: 6894
thumbnail: 6894.jpg
topic: Development
role: Developer
level: Experienced
exl-id: fb68579d-241c-414d-92f4-13194f4d1923
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 1%

---

# 自定義摘要步驟

摘要步驟元件用於顯示表單提交的摘要，其中包含下載簽名表單的連結。 摘要步驟通常放置在窗體的最後一個面板中。
為了使用此使用情形，我們基於現成的「摘要」元件建立了一個新元件，並擴展了包含自定義客戶端庫的功能。

此元件由標籤「標籤多個表單」標識

下面的螢幕抓圖顯示了為在簽署儀式完成時顯示消息而建立的新元件

![摘要元件](assets/summary.PNG)

新元件基於現成的摘要元件。
![元件](assets/componentprop.PNG)

我們已添加一個按鈕，以導航到下一個表單進行簽名
![模板代碼](assets/template-code.PNG)

summary.jsp具有以下代碼。 它引用了由類別ID標識的客戶端庫 **getnexform**

```java
<%--
  Guide Summary Component
--%>
<%@include file="/libs/fd/af/components/guidesglobal.jsp"%>
<%@include file="/libs/fd/afaddon/components/summary/summary.jsp"%>
<ui:includeClientLib categories="getnextform"/>
```

## Assets

自定義摘要元件可以 [從此處下載](assets/custom-summary-step.zip)

## 後續步驟

[獲取下一個用於簽名的表單](./create-client-lib.md)