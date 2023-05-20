---
title: 在自適應Forms中內聯顯示DAM影像
description: 在自適應Forms中內聯顯示DAM影像
feature: Adaptive Forms
topics: development
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-10-20T00:00:00Z
thumbnail: inline-dam.jpg
kt: kt-11307
exl-id: 339eb16e-8ad8-4b98-939c-b4b5fd04d67e
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 0%

---

# 自適應Forms顯示DAM影像

通用用例是以「自適應表單」內聯顯示駐留在crx儲存庫中的影像。

## 添加佔位符影像

第一步是將佔位符div預置到面板元件。 在面板元件下面的代碼中，由照片上載的CSS類名標識。 JavaScript函式是與自適應表單關聯的客戶端庫的一部分。 在初始化檔案附件元件時調用此函式。

```javascript
/**
* Add Placeholder Div
*/
function addPlaceholderDiv(){

     $(".photo-upload").prepend(" <div class='preview'' style='border:1px dotted;height:225px;width:175px;text-align:center'><br><br><div class='text'>The Image will appear here</div></div><br>");
}
```

### 顯示內嵌影像

用戶選擇影像後，隱藏欄位ImageName將填充選定的影像名稱。 然後，此影像名稱將傳遞給damURLToFile函式，該函式調用createFile函式將URL轉換為FileReader.readAsDataURL()的Blob。

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

* 下載並安裝 [客戶端庫和示例影像](assets/InlineDAMImage.zip) 在實例AEM上使AEM用包管理器。
* 下載並安裝 [樣式](assets/FieldInspectionForm.zip) 使用包管AEM理器在您AEM的實例上。
* 將瀏覽器指向 [檔案檢查表單](http://localhost:4502/content/dam/formsanddocuments/fieldinspection/fieldinspection/jcr:content?wcmmode=disabled)
* 選取一個夾具
* 您應看到窗體中顯示的影像
