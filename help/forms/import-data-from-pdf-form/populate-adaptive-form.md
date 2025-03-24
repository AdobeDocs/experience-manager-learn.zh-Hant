---
title: 使用setData方法填入最適化表單
description: 傳送已上傳的pdf檔案以進行資料擷取，並使用擷取的資料填入最適化表單
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-14196
exl-id: f380d589-6520-4955-a6ac-2d0fcd5aaf3f
duration: 32
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 1%

---

# 進行Ajax呼叫

當使用者已上傳PDF檔案時，我們需要對servlet進行POST呼叫，並在POST請求中傳遞已上傳的PDF檔案。 POST要求會傳回crx存放庫中匯出資料的路徑

```javascript
$("#fileElem").on('change', function (e) {
           console.log("submitting files");
           var filesUploaded = e.target.files;
           var ajaxData = new FormData($("#myform").get(0));
           for (var i = 0; i < filesUploaded.length; i++) {
               ajaxData.append(filesUploaded[i].name, filesUploaded[i]);
           }

           handleFiles(ajaxData);

       });

function handleFiles(formData) {
    console.log("File uploaded");

    $.ajax({
        type: 'POST',
        data: formData,
        url: '/bin/ExtractDataFromPDF',
        contentType: false,
        processData: false,
        cache: false,
        success: function (filePath) {
            console.log(filePath);
            guideBridge.setData({
                dataRef: filePath,
                error: function (guideResultObject) {
                    console.log("Error");
                }
            })
            

        }
    });
}
```

掛載在&#x200B;**_/bin/ExtractDataFromPDF_**上的servlet會從PDF檔案中擷取資料，並傳回所擷取資料儲存所在的crx節點的路徑。
接著會使用[GuideBridge setData](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html#setData__anchor)方法來設定最適化表單的資料。

## 後續步驟

[部署範例資產](./test-the-solution.md)
