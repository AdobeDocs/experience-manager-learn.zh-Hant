---
title: 在網頁中嵌入自適應Forms/HTML5表單
description: 在非網頁中嵌入自適應Forms或HTML5表單所需AEM的配置步驟。
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 8390
exl-id: 068e38df-9c71-4f55-b6d6-e1486c29d0a9
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# 在網頁中嵌入自適應表單或HTML5表單

嵌入式自適應表單功能齊全，用戶無需離開頁面即可填寫和提交表單。 它幫助用戶保持在網頁上其他元素的上下文中並同時與表單交互。

以下視頻說明了在網頁中嵌入Adaptive或HTML5表單所需的步驟。
請參閱 [文檔](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html) 最佳先決條件、最佳做法等。
>[!VIDEO](https://video.tv.adobe.com/v/335893?quality=12&learn=on)

您可以下載視頻中使用的示例檔案 [從這裡](assets/embedding-af-web-page.zip)

下面是用於提取自適應表單並將表單嵌入由類名標識的容器中的代碼 **右**

```javascript
$(document).ready(function(){
  
    var formName = $( "#xfaform" ).val();
    $.get("http://localhost/content/dam/formsanddocuments/newslettersubscription/jcr:content?wcmmode=disabled", function(data, status){
    console.log(formName);
    $(".right").append(data);
      
    });
  
});
```
