---
title: 將適用性Forms/HTML5表單內嵌在網頁中
description: 將適用性Forms或HTML5表單內嵌至非AEM網頁所需的設定步驟。
feature: 適用性表單
feature-set: Adaptive Forms
topics: development
audience: developer
doc-type: Tutorial
activity: implement
version: 6.4,6.5
topic: 管理
role: Admin
level: Beginner
kt: 8390
source-git-commit: 2fc4f748fd3b8f820d1451d08c5fe01d11892029
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 2%

---


# 將適用性表單或HTML5表單內嵌至網頁

內嵌的最適化表單功能齊全，使用者無需離開頁面即可填寫及提交表單。 可協助使用者停留在網頁上其他元素的內容中，並同時與表單互動。

以下影片說明在網頁中內嵌適用性或HTML5表單所需的步驟。
請參閱[檔案](https://experienceleague.adobe.com/docs/experience-manager-64/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html?lang=en)以了解最佳必要條件、最佳實務等。
>[!VIDEO](https://video.tv.adobe.com/v/335893?quality=9&learn=on)

您可以從此處](assets/embedding-af-web-page.zip)下載視訊[中使用的範例檔案

以下是用來擷取適用性表單並將表單內嵌於類別名稱&#x200B;**right**&#x200B;所識別容器中的程式碼

```javascript
$(document).ready(function(){
  
	var formName = $( "#xfaform" ).val();
    $.get("http://localhost/content/dam/formsanddocuments/newslettersubscription/jcr:content?wcmmode=disabled", function(data, status){
	console.log(formName);
	$(".right").append(data);
      
    });
  
});
```














