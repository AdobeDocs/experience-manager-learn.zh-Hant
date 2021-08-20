---
title: 自訂摘要元件
description: 擴充摘要步驟元件，加入導覽至套件中下一個表單的功能。
feature: 適用性表單
version: 6.4,6.5
kt: 6894
thumbnail: 6894.jpg
topic: 開發
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 2%

---


# 自訂摘要步驟

摘要步驟元件用於顯示表單提交摘要，其中包含下載已簽署表單的連結。 摘要步驟通常會放在表單的最後一個面板中。
為此使用案例，我們根據現成可用的「摘要」元件建立了新元件，並擴充了包含自訂clientlib的功能。

此元件由標籤「簽名多個表單」標識

下列螢幕擷取畫面顯示建立的新元件，以在簽署儀式結束時顯示訊息

![摘要元件](assets/summary.PNG)

新元件以現成可用的摘要元件為基礎。
![component-prop](assets/componentprop.PNG)

我們已新增按鈕，可導覽至下一個表單以進行簽署
![template-code](assets/template-code.PNG)

summary.jsp的代碼如下。 它參考類別id **getnextform**&#x200B;所識別的用戶端程式庫

```java
<%--
  Guide Summary Component
--%>
<%@include file="/libs/fd/af/components/guidesglobal.jsp"%>
<%@include file="/libs/fd/afaddon/components/summary/summary.jsp"%>
<ui:includeClientLib categories="getnextform"/>
```

## 資產

自訂摘要元件可從此處[下載](assets/custom-summary-step.zip)


