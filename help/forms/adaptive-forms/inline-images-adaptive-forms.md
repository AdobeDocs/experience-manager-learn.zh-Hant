---
title: 在適用性Forms中顯示內嵌影像
description: 在適用性Forms中內嵌顯示已上傳的影像
feature: Adaptive Forms
topics: development
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 4a69513d-992c-435a-a520-feb9085820e7
source-git-commit: 9529b1f6d1a863fc570822c8ecd6c4be01b36729
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# 適用性Forms中的內嵌影像

常見的使用案例是在適用性表單中將上傳的影像顯示為內嵌影像。 依預設，上傳的影像會顯示為連結，而此體驗可透過在適用性表單中顯示影像來增強。 本文將引導您完成顯示內嵌影像的相關步驟。

## 添加佔位符影像

第一步是在檔案附件元件的開頭附加預留位置div。 在下方的程式碼中，檔案附件元件是以其像片上傳的CSS類別名稱來識別。 JavaScript函式是與適用性表單相關聯之用戶端程式庫的一部分。 在初始化檔案附件元件的事件時調用此函式。

```javascript
/**
* Add Placeholder Image
* @return {string} 
 */
function addTempImage(){
  $(".photo-upload").prepend(" <div class='preview'' style='border:2px solid;height:225px;width:175px;text-align:center'><br><br><div class='text'>3.5mm * 4.5mm<br>2Mb max<br>Min 600dpi</div></div><br>");

}
```

### 顯示內嵌影像

在用戶上載映像後，將在檔案附件元件的提交事件中調用以下列出的函式。 函式會以引數的形式接收上傳的檔案物件。

```javascript
/**
* Consume Image
* @return {string} 
 */
function consumeImage (file) {
  var reader = new FileReader();
    console.log("Reading file");
  reader.addEventListener("load", function (e) {
    console.log("in the event listener");
    var image  = new Image();
    $(".photo-upload .preview .imageP").remove();
    $(".photo-upload .preview .text").remove();
    image.width = 170;image.height = 220;
    image.className = "imageP";
    image.addEventListener("load", function () {
      $(".photo-upload .preview")[0].prepend(this);
    });
    image.src = window.URL.createObjectURL(file);
  });
  reader.readAsDataURL(file); 
}
```

### 在伺服器上部署

* 下載並安裝 [用戶庫](assets/inline-image-client-library.zip) 在AEM例項上使用AEM套件管理器。
* 下載並安裝 [範例表單](assets/inline-image-af.zip) 在您的AEM例項上使用AEM套件管理器。
* 將瀏覽器指向 [新增內嵌影像](http://localhost:4502/content/dam/formsanddocuments/addinlineimage/jcr:content?wcmmode=disabled)
* 按一下「附加您的照片」按鈕以添加影像
