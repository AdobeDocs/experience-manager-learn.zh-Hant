---
title: 自動完成功能，在AEM Forms
description: 允許用戶利用搜索和篩選，在鍵入時快速查找並從預先填充的值清單中進行選擇。
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 11374
last-substantial-update: 2022-11-01T00:00:00Z
exl-id: e9a696f9-ba63-462d-93a8-e9a7a1e94e72
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 0%

---

# 實施自動完成

使用jquery的自動完AEM成功能在表單中實現自動完成功能。
本文中包括的示例使用各種資料源（靜態陣列、從REST API響應填充的動態陣列）來填充建議，當用戶開始在文本欄位中鍵入內容時。

用於完成自動完成功能的代碼與欄位的初始化事件相關聯。

## 提供地址建議

![國家建議](assets/auto-complete2.png)



以下是用於提供街道地址建議的代碼

```javascript
$(".streetAddress input").autocomplete({
    source: function(request, response) {
        $.ajax({
            url: "https://api.geoapify.com/v1/geocode/autocomplete?text=" + request.term + "&apiKey=Your API Key", //please get your own API key with geoapify.com
            responseType: "application/json",
            success: function(data) {
                console.log(data.features.length);
                response($.map(data.features, function(item) {
                    return {
                        label: [item.properties.formatted],
                        value: [item.properties.formatted]
                    };
                }));
            },
        });
    },
    minLength: 5,
    select: function(event, ui) {
        console.log(ui.item ?
            "Selected: " + ui.item.label :
            "Nothing selected, input was " + this.value);
    }

});
```





## emoji表情符的建議

![國家建議](assets/auto-complete3.png)

以下代碼用於在建議清單中顯示emoji的

```javascript
var values=["Wolf \u{1F98A}", "Lion \u{1F981}","Puppy \u{1F436}","Giraffe \u{1F992}","Frog \u{1F438}"];
$(".Animals input").autocomplete( {
minLength: 1, source: values, delay: 0
}

);
```

的 [可以下載示例表單](assets/auto-complete-form.zip) 從這裡。 請確保使用代碼編輯器為代碼提供您自己的用戶名/API密鑰，以成功進行REST調用。

>[!NOTE]
>
> 要自動完成工作，請確保您的表單使用以下客戶端庫 **cq.jquery.ui**。 此客戶端庫隨附AEM。
