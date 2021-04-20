---
title: 從MySQL資料庫儲存和檢索表單資料
description: 多部分教學課程，引導您逐步瞭解儲存和擷取表單資料的相關步驟
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 2%

---

# 建立用戶端程式庫

用AEM戶端程式庫管理您所有的用戶端JavaScript程式碼。 在本文中，我已建立簡單的JavaScript，以使用指南橋接API擷取最適化表單資料。 一旦提取了自適應表單資料，就會對servlet進行POST調用，以插入或更新資料庫中的自適應表單資料。 函式getALLUrlParams會傳回URL中的參數。 如果URL中存在guid參數，則我們需要執行更新操作（如果不是插入操作）。其餘功能都在與。savebutton類的click事件關聯的代碼中處理。

>[!NOTE]
>
>客戶端庫是本教學課程資產的一部分

```javascript
function getAllUrlParams(url) {

    // get query string from url (optional) or window
    var queryString = url ? url.split('?')[1] : window.location.search.slice(1);

    // we'll store the parameters here
    var obj = {};

    // if query string exists
    if (queryString) {

        // stuff after # is not part of query string, so get rid of it
        queryString = queryString.split('#')[0];

        // split our query string into its component parts
        var arr = queryString.split('&');

        for (var i = 0; i < arr.length; i++) {
            // separate the keys and the values
            var a = arr[i].split('=');

            // set parameter name and value (use 'true' if empty)
            var paramName = a[0];
            var paramValue = typeof(a[1]) === 'undefined' ? true : a[1];

            // (optional) keep case consistent
            paramName = paramName.toLowerCase();
            if (typeof paramValue === 'string') paramValue = paramValue.toLowerCase();

            // if the paramName ends with square brackets, e.g. colors[] or colors[2]
            if (paramName.match(/\[(\d+)?\]$/)) {

                // create key if it doesn't exist
                var key = paramName.replace(/\[(\d+)?\]/, '');
                if (!obj[key]) obj[key] = [];

                // if it's an indexed array e.g. colors[2]
                if (paramName.match(/\[\d+\]$/)) {
                    // get the index value and add the entry at the appropriate position
                    var index = /\[(\d+)\]/.exec(paramName)[1];
                    obj[key][index] = paramValue;
                } else {
                    // otherwise add the value to the end of the array
                    obj[key].push(paramValue);
                }
            } else {
                // we're dealing with a string
                if (!obj[paramName]) {
                    // if it doesn't exist, create property
                    obj[paramName] = paramValue;
                } else if (obj[paramName] && typeof obj[paramName] === 'string') {
                    // if property does exist and it's a string, convert it to an array
                    obj[paramName] = [obj[paramName]];
                    obj[paramName].push(paramValue);
                } else {
                    // otherwise add the property
                    obj[paramName].push(paramValue);
                }
            }
        }
    }

    return obj;
}

$(document).ready(function() {
    console.log(window.location.hostname + "   " + window.location.protocol);
    var linktext = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
    var linktext1 = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktext1[0]");
    linktext.visible = false;
    linktext1.visible = false;
    $(".savebutton").click(function() {
        var params = getAllUrlParams(window.location.href);
        console.log(getAllUrlParams(window.location.href));
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                console.log("The data is " + guideResultObject.data);
                let xhr = new XMLHttpRequest();
                xhr.open('POST', '/bin/storeafdata');
                let formData = new FormData();
                if (typeof(params.guid) != "undefined") {
                    formData.append("operation", "update");
                    formData.append("guid", params.guid);

                }
                if (typeof(params.guid) == "undefined") {
                    formData.append("operation", "insert");


                }


                formData.append("formdata", guideResultObject.data);
                xhr.send(formData);
                xhr.onload = function(e) {
                    console.log("The data is ready");
                    if (this.status == 200) {
                        var jsonResponse = JSON.parse(this.response);
                        console.log(jsonResponse.guid);
                        var linktext = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
                        var linktext1 = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktext1[0]");
                        linktext1.visible = true;
                        linktext.value =  "/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled&guid=" + jsonResponse.guid;
                        linktext.visible = true;
                        guideBridge.setFocus("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
                    }

                }
            }
        });


    });


});
```
