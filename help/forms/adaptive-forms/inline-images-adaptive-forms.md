---
title: 在最適化表單中顯示內嵌影像
seo-title: 在最適化表單中顯示內嵌影像
description: 在Adaptive Forms中內嵌顯示上傳的影像
seo-description: 在Adaptive Forms中內嵌顯示上傳的影像
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 0%

---


# 最適化表單中的內嵌影像

常見的使用案例是在「最適化表單」中將上傳的影像顯示為內嵌影像。 依預設，上傳的影像會顯示為連結，而透過在最適化表單中顯示影像，可增強此體驗。 本文將引導您逐步瞭解顯示內嵌影像的相關步驟。

## 新增預留位置影像

第一個步驟是在檔案附件元件的預留位置div前面附加預留位置。 在下方的程式碼中，檔案附件元件會以其像片上傳的CSS類別名稱來識別。 JavaScript函式是與最適化表單相關聯之用戶端程式庫的一部分。 此函式在檔案附件元件的初始化事件中被調用。

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

在用戶上傳映像後，在檔案附件元件的commit事件中將調用下面列出的函式。 函式將上載的檔案對象作為參數接收。

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

### 部署在您的伺服器上

* 使用AEM套件管理器，在您的AEM例項上下載並安裝[用戶端程式庫](assets/inline-image-client-library.zip)。
* 使用AEM套件管理器，在您的AEM例項上下載並安裝[範例表單](assets/inline-image-af.zip)。
* 將瀏覽器指向[新增內嵌影像](http://localhost:4502/content/dam/formsanddocuments/addinlineimage/jcr:content?wcmmode=disabled)
* 按一下「附加您的像片」按鈕以新增影像