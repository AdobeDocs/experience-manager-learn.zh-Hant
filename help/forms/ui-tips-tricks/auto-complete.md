---
title: AEM Forms中的自動完成功能
description: 讓使用者利用搜尋和篩選，在輸入時快速尋找和選取預先填入的值清單。
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-11374
last-substantial-update: 2022-11-01T00:00:00Z
exl-id: e9a696f9-ba63-462d-93a8-e9a7a1e94e72
duration: 47
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 0%

---

# 實作自動完成

使用jquery的自動完成功能，在AEM表單中實作自動完成功能。
本文包含的範例使用各種資料來源（靜態陣列、從REST API回應填入的動態陣列）在使用者開始輸入文字欄位時填入建議。

用來完成自動完成功能的程式碼與欄位的初始化事件相關聯。

## 提供地址建議

![國家/地區建議](assets/auto-complete2.png)



以下是用來提供街道地址建議的程式碼

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





## 使用emoji的建議

![國家/地區建議](assets/auto-complete3.png)

下列程式碼用於顯示建議清單中的emoji

```javascript
var values=["Wolf \u{1F98A}", "Lion \u{1F981}","Puppy \u{1F436}","Giraffe \u{1F992}","Frog \u{1F438}"];
$(".Animals input").autocomplete( {
minLength: 1, source: values, delay: 0
}

);
```

可從這裡下載[範例表單](assets/auto-complete-form.zip)。 請務必使用程式碼的程式碼編輯器提供您自己的使用者名稱/API金鑰，以成功執行REST呼叫。

>[!NOTE]
>
> 若要自動完成以運作，請確認您的表單使用下列使用者端程式庫&#x200B;**cq.jquery.ui**。 此使用者端資料庫隨附AEM。
