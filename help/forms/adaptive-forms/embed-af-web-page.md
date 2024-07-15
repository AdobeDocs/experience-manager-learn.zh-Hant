---
title: 在網頁中內嵌Adaptive Forms/HTML5表單
description: 將最適化Forms或HTML5表單嵌入非AEM網頁所需的設定步驟。
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-8390
exl-id: 068e38df-9c71-4f55-b6d6-e1486c29d0a9
last-substantial-update: 2020-06-09T00:00:00Z
duration: 398
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 29%

---

# 將最適化表單或HTML5表單內嵌在網頁中

嵌入式調適型表單功能齊全，使用者無需離開頁面即可填寫並提交表單。此功能可幫助使用者在網頁維持相關的其他元素，並同時與表單進行互動。

以下影片說明在網頁中內嵌Adaptive或HTML5表單所需的步驟。
請參閱[檔案](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html)，瞭解最佳必要條件、最佳實務等。
>[!VIDEO](https://video.tv.adobe.com/v/335893?quality=12&learn=on)

您可以從這裡](assets/embedding-af-web-page.zip)下載視訊[中使用的範例檔案

下列是用來擷取最適化表單並將表單內嵌在類別名稱&#x200B;**right**&#x200B;所識別的容器中的程式碼

```javascript
$(document).ready(function(){
  
    var formName = $( "#xfaform" ).val();
    $.get("http://localhost/content/dam/formsanddocuments/newslettersubscription/jcr:content?wcmmode=disabled", function(data, status){
    console.log(formName);
    $(".right").append(data);
      
    });
  
});
```
