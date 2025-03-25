---
title: 建立使用者端資源庫
description: 用於擷取下一個要簽署的表單的使用者端資料庫程式碼
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6907
thumbnail: 6907.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 3c148b30-2c7d-428d-9a3c-f3067ca3a239
duration: 34
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 3%

---

# 建立使用者端資源庫

建立自訂使用者端資料庫（簡稱clientlib）以擷取url引數，並在GET呼叫中傳遞這些引數。 GET會呼叫掛接在/bin/getnextformtosign上的servlet，此servlet會傳回下一個表單的url以登入套件。

以下是clientlib javascript函式中使用的程式碼


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

[可以從這裡下載clientlib](assets/get-next-form-client-lib.zip)

## 後續步驟

[建立此使用案例的自訂表單範本](./create-af-template.md)