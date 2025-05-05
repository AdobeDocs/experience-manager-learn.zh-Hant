---
title: 建立使用者端資料庫
description: 建立使用者端程式庫以處理「儲存並退出」按鈕的點選事件
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6597
thumbnail: 6597.pg
topic: Development
role: Developer
level: Intermediate
exl-id: c90eea73-bd44-40af-aa98-d766aa572415
duration: 42
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 1%

---

# 建立使用者端程式庫

建立[使用者端程式庫](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=zh-Hant)，它將包含在CSS類別&#x200B;**儲存按鈕**&#x200B;所識別的按鈕的點選事件上呼叫`guideBridge` API的方法`doAjaxSubmitWithFileAttachment`的程式碼。  我們將最適化表單資料`fileMap`和`mobileNumber`傳遞至`**/bin/storeafdatawithattachments`接聽的端點

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
> 我們已使用[啟動方塊JavaScript資料庫](https://bootboxjs.com/examples.html)來顯示對話方塊

此範例中使用的使用者端資料庫可以從這裡[下載。](assets/store-af-with-attachments-client-lib.zip)

## 後續步驟

[驗證具有OTP服務的使用者](./verify-users-with-otp.md)
