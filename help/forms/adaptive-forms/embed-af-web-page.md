---
title: 將適用性Forms/HTML5表單內嵌在網頁中
description: 將適用性Forms或HTML5表單內嵌至非AEM網頁所需的設定步驟。
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 8390
exl-id: 068e38df-9c71-4f55-b6d6-e1486c29d0a9
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 0%

---

# 將最適化表單或HTML5表單嵌入網頁

內嵌的最適化表單功能齊全，使用者無需離開頁面即可填寫及提交表單。 可協助使用者停留在網頁上其他元素的內容中，並同時與表單互動。

以下影片說明在網頁中內嵌適用性或HTML5表單所需的步驟。
請參閱 [檔案](https://experienceleague.adobe.com/docs/experience-manager-64/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html?lang=en) 最佳必要條件、最佳實務等。
>[!VIDEO](https://video.tv.adobe.com/v/335893?quality=12&learn=on)

您可以下載影片中使用的範例檔案 [從這裡](assets/embedding-af-web-page.zip)

以下是用來擷取適用性表單並將表單嵌入類別名稱所識別容器的程式碼 **右**

```javascript
$(document).ready(function(){
  
    var formName = $( "#xfaform" ).val();
    $.get("http://localhost/content/dam/formsanddocuments/newslettersubscription/jcr:content?wcmmode=disabled", function(data, status){
    console.log(formName);
    $(".right").append(data);
      
    });
  
});
```
