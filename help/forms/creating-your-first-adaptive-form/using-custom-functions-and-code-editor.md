---
title: 使用函式和程式碼編輯器
seo-title: 使用函式和程式碼編輯器
description: 使用函式和程式碼編輯器來編寫業務規則
seo-description: 使用函式和程式碼編輯器來編寫業務規則
uuid: 578e91f8-0d93-4192-b7af-1579df2feaf8
feature: adaptive-forms
topics: authoring
audience: developer
doc-type: tutorial
activity: understand
version: 6.4,6.5
discoiquuid: f480ef3e-7e38-4a6b-a223-c102787aea7f
kt: 4270
thumbnail: 22282.jpg
translation-type: tm+mt
source-git-commit: c60a46027cc8d71fddd41aa31dbb569e4df94823
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 0%

---


# 使用自訂函式和程式碼編輯器 {#using-functions-and-code-editor}

在本部分，我們將使用自訂函式和程式碼編輯器來編寫業務規則。

在本教學課程的前面，您已 [經安裝了帶有自定義函式](assets/client-libs-and-logo.zip) 的ClientLib。

通常用戶端程式庫包含CSS和Javascript檔案。 此用戶端程式庫包含javascript檔案，可讓函式填入下拉式清單值。


## 填入下拉式清單的函式 {#function-to-populate-drop-down-list}

>[!VIDEO](https://video.tv.adobe.com/v/22282?quality=9&learn=on)

### 設定面板的摘要標題 {#set-the-summary-title-of-panels}

>[!VIDEO](https://video.tv.adobe.com/v/28387?quality=9&learn=on)

#### 驗證面板 {#validate-panels-using-rule-editor}

>[!VIDEO](https://video.tv.adobe.com/v/28409?quality=9&learn=on)

以下是用於驗證面板欄位的程式碼

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

您可以取消對第1行的注釋，以在瀏覽器視窗中除錯代碼。

第4行——獲取當前面板

第5行——驗證當前面板。

第9行——如果沒有錯誤，請移至下一個面板

預覽表單並測試新啟用的功能。
