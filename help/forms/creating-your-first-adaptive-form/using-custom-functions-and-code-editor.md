---
title: 使用函式和代碼編輯器
description: 使用函式和代碼編輯器編寫業務規則
feature: Adaptive Forms
version: 6.4,6.5
kt: 4270
thumbnail: 22282.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 7b2a4075-bfdf-49f3-b507-34d86193bf64
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 0%

---

# 使用自定義函式和代碼編輯器 {#using-functions-and-code-editor}

在本部分，我們將使用自定義函式和代碼編輯器來編寫業務規則。

您已經安裝了 [具有自定義函式的ClientLib](assets/client-libs-and-logo.zip) 在本教程的前面部分。

通常，客戶端庫由CSS和Javascript檔案組成。 此客戶端庫包含Javascript檔案，它顯示用於填充下拉清單值的函式。


## 填充下拉清單的函式 {#function-to-populate-drop-down-list}

>[!VIDEO](https://video.tv.adobe.com/v/22282?quality=12&learn=on)

### 設定面板的摘要標題 {#set-the-summary-title-of-panels}

>[!VIDEO](https://video.tv.adobe.com/v/28387?quality=12&learn=on)

#### 驗證面板 {#validate-panels-using-rule-editor}

>[!VIDEO](https://video.tv.adobe.com/v/28409?quality=12&learn=on)

以下是用於驗證面板欄位的代碼

```javascript
//debugger;
var errors =[];
var fields ="";
var currentPanel = guideBridge.getFocus({"focusOption": "navigablePanel"});
window.guideBridge.validate(errors,currentPanel);
console.log("The errors are "+ errors.length);
if(errors.length===0)
{
        window.guideBridge.setFocus(this.panel.somExpression, 'nextItem', true);
}
else
  {
    for(var i=0;i<errors.length;i++)
      {
        var fields = fields+guideBridge.resolveNode(errors[i].som).title+" , ";
      }
        window.confirm("Please fill out  "+fields.slice(0,-1)+ " fields");
  }
```

您可以取消注釋行1以在瀏覽器窗口中調試代碼。

行4 — 獲取當前面板

第5行 — 驗證當前面板。

第9行 — 如果沒有錯誤，則移至下一個面板

預覽表單並test新啟用的功能。
