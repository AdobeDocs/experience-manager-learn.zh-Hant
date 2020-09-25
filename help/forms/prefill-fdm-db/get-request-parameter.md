---
title: 取得請求參數
description: 存取表單資料模型的預填服務的請求參數
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5815
thumbnail: kt-5815.jpg
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 0%

---

# 取得請求參數

## 獲取empID參數

下一步是從url存取empID參數。 然後，將empID請求參數的值傳遞給表 **_單資料模_** 型的get服務操作。
為了本課程的目的，我們建立並提供了以下內容

* 稱為FDMDemo的最適化表 **_單範本_**
* 名為 **_fdmdemo的頁面元件_**
* 將我們的自訂jsp與頁面元件一起包含
* 將最適化表單範本與頁面元件關聯

執行此動作時，只有在轉換基於此自訂範本的最適化表單時，才會執行自訂jsp中的程式碼

* [使用包管理器](assets/template-page-component.zip) 導入 [包](http://localhost:4502/crx/packmgr/index.jsp)
* [開啟fdmrequest.jsp](http://localhost:4502/crx/de/index.jsp#/apps/fdmdemo/component/page/fdmdemo/fdmrequest.jsp)
* 取消注釋已注釋的行。
* 儲存變更

```java
if(request.getParameter("empID")!=null)
    {
      //System.out.println("Adobe !!!There is a empID parameter in the request "+request.getParameter("empID"));
      //java.util.Map paraMap = new java.util.HashMap();
      //paraMap.put("empID",request.getParameter("empID"));
      //slingRequest.setAttribute("paramMap",paraMap);
    }
```

empID的值與paraMap中名為empID的鍵相關聯。 此地圖接著會傳遞至slingRequest

>[!NOTE]
>
>密鑰empID必須與新實體獲取服務的綁定值匹配
