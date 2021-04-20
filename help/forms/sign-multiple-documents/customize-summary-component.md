---
title: 自訂摘要元件
description: 擴充摘要步驟元件，加入導覽至封裝中下一個表單的功能。
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6894
thumbnail: 6894.jpg
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '182'
ht-degree: 2%

---


# 自訂摘要步驟

摘要步驟元件可用來顯示表單提交的摘要，並提供下載已簽署表單的連結。 摘要步驟通常會放在表單的最後一個面板中。
為此使用案例，我們根據現成的「摘要」元件建立了新元件，並擴充了包含自訂clientlib的功能。

此元件由標籤「簽署多個表單」來識別

以下螢幕擷取畫面顯示建立的新元件，以顯示簽署儀式完成時的訊息

![摘要元件](assets/summary.PNG)

新元件是以現成可用的摘要元件為基礎。
![component-prop](assets/componentprop.PNG)

我們新增了一個按鈕，可導覽至下一個表單進行簽署
![template-code](assets/template-code.PNG)

summary.jsp具有以下代碼。 它引用了類別id **getnextform**&#x200B;所標識的客戶端庫

```java
<%--
  Guide Summary Component
--%>
<%@include file="/libs/fd/af/components/guidesglobal.jsp"%>
<%@include file="/libs/fd/afaddon/components/summary/summary.jsp"%>
<ui:includeClientLib categories="getnextform"/>
```

## 資產

自訂摘要元件可從此處下載[](assets/custom-summary-step.zip)


