---
title: 在最適化Forms中內嵌顯示DAM影像
description: 在最適化Forms中內嵌顯示DAM影像
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-10-20T00:00:00Z
thumbnail: inline-dam.jpg
kt: kt-11307
exl-id: 339eb16e-8ad8-4b98-939c-b4b5fd04d67e
duration: 60
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---

# 在最適化Forms中顯示DAM影像

常見的使用案例是以Adaptive Form內聯顯示駐留在crx存放庫中的影像。

## 新增預留位置影像

第一個步驟是在面板元件前面加上預留位置div。 在下面的程式碼中，面板元件的CSS類別名稱photo-upload可用來識別。 JavaScript函式是與最適化表單相關聯的使用者端資料庫的一部分。 此函式是在檔案附件元件的初始化事件中呼叫。

```javascript
/**
* Add Placeholder Div
*/
function addPlaceholderDiv(){

     $(".photo-upload").prepend(" <div class='preview'' style='border:1px dotted;height:225px;width:175px;text-align:center'><br><br><div class='text'>The Image will appear here</div></div><br>");
}
```

### 顯示內嵌影像

使用者選取影像後，隱藏欄位ImageName會填入選取的影像名稱。 然後，此影像名稱會傳遞至damURLToFile函式，該函式會叫用createFile函式，將URL轉換為FileReader.readAsDataURL()的Blob。

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

### 在您的伺服器上部署

* 使用AEM封裝管理員下載[使用者端程式庫和範例影像](assets/InlineDAMImage.zip)，並安裝在您的AEM執行個體上。
* 使用AEM封裝管理員下載[範例表單](assets/FieldInspectionForm.zip)並安裝在您的AEM執行個體上。
* 將瀏覽器指向[FielInspectionForm](http://localhost:4502/content/dam/formsanddocuments/fieldinspection/fieldinspection/jcr:content?wcmmode=disabled)
* 選取其中一個夾具
* 您應該會看到表單中顯示的影像
