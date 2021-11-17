---
title: 一些實用的UI提示與秘訣
description: 說明一些有用的使用者介面提示的檔案
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9270
source-git-commit: 9529b1f6d1a863fc570822c8ecd6c4be01b36729
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 2%

---

# 切換密碼欄位可見性

常見的使用案例是允許填入表單切換至密碼欄位中輸入文字的可見性。
為了完成此使用案例，我已使用 [Font Awesome Library](https://fontawesome.com/). 所需的CSS和eye.svg包含在為此示範建立的用戶端程式庫中。



## 程式碼範例

適用性表單的欄位類型為PasswordBox，稱為 **ssnField**.

下列程式碼會在表單載入時執行

```javascript
$(document).ready(function() {
    $(".ssnField").append("<p id='fisheye' style='color: Dodgerblue';><i class='fa fa-eye'></i></p>");

    $(document).on('click', 'p', function() {
        var type = $(".ssnField").find("input").attr("type");

        if (type == "text") {
            $(".ssnField").find("input").attr("type", "password");
        }

        if (type == "password") {
            $(".ssnField").find("input").attr("type", "text");
        }

    });

});
```

下列CSS用於定位 **眼** 「密碼」欄位內的表徵圖

```javascript
.svg-inline--fa {
    /* display: inline-block; */
    font-size: inherit;
    height: 1em;
    overflow: visible;
    vertical-align: -.125em;
    position: absolute;
    top: 45px;
    right: 40px;
}
```

## 部署切換密碼示例

* 下載 [用戶庫](assets/simple-ui-tips.zip)
* 下載 [範例表單](assets/simple-ui-tricks-form.zip)
* 使用匯入用戶端程式庫 [套件管理器UI](http://localhost:4502/crx/packmgr/index.jsp)
* 使用匯入範例表單 [Forms與檔案](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [預覽表單](http://localhost:4502/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)


