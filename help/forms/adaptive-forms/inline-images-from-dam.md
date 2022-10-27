---
title: 在適用性Forms中顯示內嵌影像
description: 在適用性Forms中內嵌顯示已上傳的影像
feature: Adaptive Forms
topics: development
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-10-20T00:00:00Z
thumbnail: inline-dam.jpg
kt: kt-11307
source-git-commit: e1c16ff347f5f398c7bc47233049427eeffa2aab
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# 在最適化Forms中顯示DAM影像

常見的使用案例是內嵌在最適化表單中顯示crx存放庫中的影像。

## 添加佔位符影像

第一步是在面板元件的開頭附加預留位置div。 在下方的程式碼中，面板元件是以其像片上傳的CSS類別名稱來識別。 JavaScript函式是與適用性表單相關聯之用戶端程式庫的一部分。 在初始化檔案附件元件的事件時調用此函式。

```javascript
/**
* Add Placeholder Div
*/
function addPlaceholderDiv(){

     $(".photo-upload").prepend(" <div class='preview'' style='border:1px dotted;height:225px;width:175px;text-align:center'><br><br><div class='text'>The Image will appear here</div></div><br>");
}
```

### 顯示內嵌影像

用戶選擇影像後，隱藏欄位ImageName將填充所選影像名稱。 然後，此影像名稱會傳遞至damURLToFile函式，該函式會叫用createFile函式，將URL轉換為FileReader.readAsDataURL()的Blob。

```javascript
/**
* DAM URL to File Object
* @return {string} 
 */
 function damURLToFile (imageName) {
   console.log("The image selected is "+imageName);
     createFile(imageName);
}
```

```javascript
async function createFile(imageName){
  let response = await fetch('/content/dam/formsanddocuments/fieldinspection/images/'+imageName);
  let data = await response.blob();
    console.log(data);
  let metadata = {
    type: 'image/jpeg'
  };
  let file = new File([data], "test.jpg", metadata);
     let reader = new FileReader();
    reader.readAsDataURL(file);
     reader.onload = function() {
    console.log("finished reading ...."+reader.result);
    
  };
    var image  = new Image();
    $(".photo-upload .preview .imageP").remove();
    $(".photo-upload .preview .text").remove();
    image.width = 484;image.height = 334;
    image.className = "imageP";
    image.addEventListener("load", function () {
      $(".photo-upload .preview")[0].prepend(this);
    });
    
    console.log(window.URL.createObjectURL(file));
    image.src = window.URL.createObjectURL(file);

  }
```

### 在伺服器上部署

* 下載並安裝 [用戶端程式庫和範例影像](assets/InlineDAMImage.zip) 在AEM例項上。
* 下載並安裝 [範例表單](assets/FieldInspectionForm.zip) 在您的AEM例項上使用AEM套件管理器。
* 將瀏覽器指向 [FileInspectionForm](http://localhost:4502/content/dam/formsanddocuments/fieldinspection/fieldinspection/jcr:content?wcmmode=disabled)
* 選取夾具之一
* 您應該會看到表單中顯示的影像