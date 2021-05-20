---
title: 建立用戶端程式庫
description: 用戶端程式庫程式碼，以擷取下一個要簽署的表單
feature: 適用性表單
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6907
thumbnail: 6907.jpg
topic: 開發
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '84'
ht-degree: 4%

---

# 建立用戶端程式庫

建立自訂用戶端程式庫（簡稱clientlib）以擷取url參數，並在GET呼叫中傳遞這些參數。 對裝載在/bin/getnextformtosign上的servlet進行GET調用，該servlet返回要登錄包的下一個表單的URL。

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

## 資產

[可從此處下載clientlib](assets/get-next-form-client-lib.zip)
