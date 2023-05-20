---
title: 在自適應Forms中顯示內嵌影像
description: 在Adaptive Forms中內聯顯示上載的影像
feature: Adaptive Forms
topics: development
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 4a69513d-992c-435a-a520-feb9085820e7
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '225'
ht-degree: 0%

---

# 自適應Forms中的內嵌影像

常見的使用情形是在「自適應表單」中將上載的影像顯示為內嵌影像。 預設情況下，上載的影像顯示為連結，通過以自適應格式顯示影像可以增強此體驗。 本文將引導您瞭解顯示內嵌影像所涉及的步驟。

## 添加佔位符影像

第一步是將佔位符div預置到檔案附件元件。 在檔案附件元件下面的代碼中，檔案附件元件由照片上載的CSS類名標識。 JavaScript函式是與自適應表單關聯的客戶端庫的一部分。 在初始化檔案附件元件時調用此函式。

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

在用戶上載映像後，在檔案附件元件的提交事件中調用下面列出的函式。 函式將上載的檔案對象作為參數接收。

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

* 下載並安裝 [客戶端庫](assets/inline-image-client-library.zip) 在實例AEM上使AEM用包管理器。
* 下載並安裝 [樣式](assets/inline-image-af.zip) 使用包管AEM理器在您AEM的實例上。
* 將瀏覽器指向 [添加內聯映像](http://localhost:4502/content/dam/formsanddocuments/addinlineimage/jcr:content?wcmmode=disabled)
* 按一下「Attach your photo」按鈕添加影像
