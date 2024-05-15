---
title: 一些實用的UI提示和技巧
description: 檔案以示範一些實用的使用者介面提示
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9270
exl-id: 13b9cd28-2797-4da9-a300-218e208cd21b
last-substantial-update: 2019-07-07T00:00:00Z
duration: 38
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 0%

---

# 切換密碼欄位可見性

常見的使用案例是允許表單填寫者切換為在密碼欄位中輸入文字的可見度。
為了完成此使用案例，我使用了以下檔案中的眼睛圖示： [Font Awesome Library](https://fontawesome.com/). 所需的CSS和eye.svg會包含在為此示範建立的使用者端資料庫中。



## 程式碼範例

最適化表單有一個欄位為PasswordBox型別，稱為 **ssnField**.

載入表單時會執行以下程式碼

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

下列CSS是用來放置 **眼睛** 圖示在密碼欄位內

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

## 部署切換密碼範例

* 下載 [使用者端資料庫](assets/simple-ui-tips.zip)
* 下載 [範例表單](assets/simple-ui-tricks-form.zip)
* 使用匯入使用者端資料庫 [封裝管理員UI](http://localhost:4502/crx/packmgr/index.jsp)
* 使用匯入範例表單 [Forms和檔案](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [預覽表單](http://localhost:4502/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)
