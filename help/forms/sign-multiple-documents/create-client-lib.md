---
title: 建立客戶端庫
description: 客戶端庫代碼，以獲取要簽名的下一個表單
feature: Adaptive Forms
version: 6.4,6.5
kt: 6907
thumbnail: 6907.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 3c148b30-2c7d-428d-9a3c-f3067ca3a239
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 3%

---

# 建立客戶端庫

建立自定義客戶端庫（簡稱客戶端庫），以提取url參數在GET調用中傳遞這些參數。 對/bin/getnextformsign上裝載的servlet進行GET調用，該servlet返回要登錄包的下一個表單的url。

以下是客戶端libjavascript函式中使用的代碼


```java
function getUrlVars()
{
    var vars = {};
    var parts = window.location.href.replace(/[?&]+([^=&]+)=([^&]*)/gi, function(m, key, value)
    {
        vars[key] = value;
    });
    return vars;
}

function navigateToNextForm()
{
    
    console.log("The id is " + guidelib.runtime.adobeSign.submitData.agreementId);
    var guid = getUrlVars()["guid"];
    var customerID = getUrlVars()["customerID"];
    console.log("The customer Id is " + customerID);
    $.ajax(
    {
        type: 'GET',
        url: '/bin/getnextformtosign?guid=' + guid + '&customerID=' + customerID,
        contentType: false,
        processData: false,
        cache: false,
        success: function(response)
        {
            console.log(response);
            var jsonResponse = JSON.parse(JSON.stringify(response));
            console.log(jsonResponse.nextFormToSign);
            var nextFormToSign = jsonResponse.nextFormToSign;
            if (nextFormToSign != "AllDone")
            {
                window.open(nextFormToSign, '_self');
            }
            else
            {
                window.open("http://localhost:4502/content/forms/af/formsandsigndemo/alldone.html", '_self');
            }

}
    });
}
$(document).ready(function()
{
    $(document).on("click", ".nextform", navigateToNextForm);
});
```

## Assets

[客戶端庫可從此處下載](assets/get-next-form-client-lib.zip)

## 後續步驟

[為此用例建立自定義表單模板](./create-af-template.md)