---
title: 一些有用的UI提示和技巧
description: 演示一些有用的用戶介面提示的文檔
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9270
source-git-commit: c462d48d26c9a7aa0e4cfc4f24005b41e8e82cb8
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 2%

---

# 切換密碼欄位可見性

常見的使用情形是，允許填表符切換為口令欄位中輸入的文本的可見性。
要完成此使用案例，我使用了 [字型超棒庫](https://fontawesome.com/)。 為此演示建立的客戶端庫中包含所需的CSS和eye.svg。


## 示例代碼

自適應表單的欄位類型為PasswordBox **ssn欄位**。

載入表單時執行以下代碼

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

以下CSS用於定位 **眼** 表徵圖

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

* 下載 [客戶端庫](assets/simple-ui-tips.zip)
* 下載 [樣式](assets/simple-ui-tricks-form.zip)
* 使用 [包管理器UI](http://localhost:4502/crx/packmgr/index.jsp)
* 使用 [Forms與文檔](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [預覽窗體](http://localhost:4502/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)


