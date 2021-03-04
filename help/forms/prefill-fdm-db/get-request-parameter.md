---
title: 取得請求參數
description: 存取表單資料模型的預填服務的請求參數
feature: 適用性表單
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5815
thumbnail: kt-5815.jpg
topic: 開發
role: 開發人員
level: 初學者
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 4%

---

# 取得請求參數

## 獲取empID參數

下一步是從url存取empID參數。 然後，將empID請求參數的值傳遞給表單資料模型的&#x200B;**_get_**服務操作。
為了本課程的目的，我們建立並提供了以下內容

* 調用&#x200B;**_FDMDemo_**&#x200B;的自適應表單模板
* 名為&#x200B;**_fdmdemo_**&#x200B;的頁面元件
* 將我們的自訂jsp與頁面元件一起包含
* 將最適化表單範本與頁面元件關聯

執行此動作時，只有在轉換基於此自訂範本的最適化表單時，才會執行自訂jsp中的程式碼

* [使用包管](assets/template-page-component.zip) 理器導入 [包](http://localhost:4502/crx/packmgr/index.jsp)
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
