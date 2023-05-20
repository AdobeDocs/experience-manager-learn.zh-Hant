---
title: 建立客戶端庫
description: 建立客戶端庫以處理「保存並退出」按鈕的按一下事件
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6597
thumbnail: 6597.pg
topic: Development
role: Developer
level: Intermediate
exl-id: c90eea73-bd44-40af-aa98-d766aa572415
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 6%

---

# 建立客戶端庫

建立 [客戶端庫](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html) 將包括調用該方法的代碼 `doAjaxSubmitWithFileAttachment` 的 `guideBridge` CSS類標識的按鈕的按一下事件上的API **保存按鈕**。  我們通過自適應表格資料， `fileMap`的 `mobileNumber` 到終結點的偵聽 `**/bin/storeafdatawithattachments`

在保存表單資料後，生成唯一的應用程式ID並在對話框中向用戶顯示。 取消對話框後，用戶將轉為允許他們使用唯一應用程式ID檢索已保存的自適應表單的表單。

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
> 我們用了 [book javascript庫](http://bootboxjs.com/examples.html) 對話框

此示例中使用的客戶端庫可以 [從此處下載](assets/client-libraries.zip)

## 後續步驟

[使用OTP服務驗證用戶](./verify-users-with-otp.md)