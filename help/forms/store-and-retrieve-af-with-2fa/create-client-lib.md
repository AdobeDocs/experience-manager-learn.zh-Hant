---
title: 建立用戶端程式庫
description: 建立clientlibrary以處理「儲存並退出」按鈕的點按事件
feature: 適用性表單
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6597
thumbnail: 6597.pg
topic: 開發
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 2%

---

# 建立客戶端庫

建立[client lib](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html)，該庫將包含用於在由CSS類&#x200B;**savebutton**&#x200B;標識的按鈕的點擊事件上調用`guideBridge` API方法`doAjaxSubmitWithFileAttachment`的代碼。  我們會將最適化表單資料`fileMap`和`mobileNumber`傳遞至`**/bin/storeafdatawithattachments`的端點監聽

保存表單資料後，將生成唯一的應用程式ID，並在對話框中向用戶顯示。 關閉對話方塊時，會將使用者視為表單，讓他們使用唯一的應用程式ID擷取儲存的最適化表單。

```java
$(document).ready(function () {
  
  $(".savebutton").click(function () {
    var tel = guideBridge.resolveNode(
      "guide[0].guide1[0].guideRootPanel[0].contactInformation[0].basicContact[0].telephoneNumber[0]"
    );
    var telephoneNumber = tel.value;
    guideBridge.getFormDataString({
      success: function (data) {
        var map = guideBridge._getFileAttachmentMapForSubmit();
        guideBridge.doAjaxSubmitWithFileAttachment(
          "/bin/storeafdatawithattachments",
          {
            data: data.data,
            fileMap: map,
            mobileNumber: telephoneNumber,
          },
          {
            success: function (x) {
              bootbox.alert(
                "This is your reference number.<br>" +
                  x.data.path +
                  " <br>You will need this to retrieve your application",
                function () {
                  console.log(
                    "This was logged in the callback! After the ok button was pressed"
                  );
                  window.location.href =
                    "http://localhost:4502/content/dam/formsanddocuments/myaccountform/jcr:content?wcmmode=disabled";
                }
              );
              console.log(x.data.path);
            },
          },
          guideBridge._getFileAttachmentsList()
        );
      },
    });
  });
});
```

>[!NOTE]
> 我們已使用[bootbox javascript庫](http://bootboxjs.com/examples.html)來顯示對話框

此示例中使用的客戶端庫可以從此處[下載](assets/client-libraries.zip)
