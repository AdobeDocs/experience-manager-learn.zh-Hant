---
title: 建立使用者端資料庫
description: 建立使用者端程式庫以處理「儲存並退出」按鈕的點選事件
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6597
thumbnail: 6597.pg
topic: Development
role: Developer
level: Intermediate
exl-id: c90eea73-bd44-40af-aa98-d766aa572415
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 6%

---

# 建立使用者端程式庫

建立 [使用者端資料庫](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html) 其中包含呼叫方法的程式碼 `doAjaxSubmitWithFileAttachment` 的 `guideBridge` CSS類別所識別之按鈕的點選事件上的API **儲存按鈕**.  我們會傳遞最適化表單資料， `fileMap`，以及 `mobileNumber` 端點接聽： `**/bin/storeafdatawithattachments`

儲存表單資料後，系統會產生唯一的應用程式ID，並在對話方塊中呈現給使用者。 關閉對話方塊時，使用者將被帶入表單，表單可讓他們使用唯一應用程式ID擷取已儲存的最適化表單。

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
                  x.data.applicationID +
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
> 我們已使用 [Bootbox JavaScript程式庫](https://bootboxjs.com/examples.html) 顯示對話方塊

此範例中使用的使用者端資料庫可以是 [已從此處下載。](assets/store-af-with-attachments-client-lib.zip)

## 後續步驟

[驗證具有OTP服務的使用者](./verify-users-with-otp.md)
