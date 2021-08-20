---
title: 取得要求參數
description: 在表單資料模型的預填服務中存取請求參數
feature: 適用性表單
version: 6.4,6.5
kt: 5815
thumbnail: kt-5815.jpg
topic: 開發
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '182'
ht-degree: 4%

---

# 取得要求參數

## 獲取empID參數

下一步是從URL中存取empID參數。 然後，將empID請求參數的值傳遞到表單資料模型的&#x200B;**_get_**服務操作。
為了本課程的目的，我們建立並提供了以下內容

* 名為&#x200B;**_FDMDemo_**&#x200B;的最適化表單範本
* 名為&#x200B;**_fdmdemo_**&#x200B;的頁面元件
* 已將自定義jsp與頁面元件一起包含
* 將最適化表單範本與頁面元件相關聯

通過執行此操作，我們在自訂jsp中的程式碼只有在根據此自訂範本呈現適用性表單時才會執行

* [使用套件管](assets/template-page-component.zip) 理器匯 [入套件](http://localhost:4502/crx/packmgr/index.jsp)
* [開啟fdmrequest.jsp](http://localhost:4502/crx/de/index.jsp#/apps/fdmdemo/component/page/fdmdemo/fdmrequest.jsp)
* 取消註解備注的行。
* 儲存您的變更

```java
if(request.getParameter("empID")!=null)
    {
      //System.out.println("Adobe !!!There is a empID parameter in the request "+request.getParameter("empID"));
      //java.util.Map paraMap = new java.util.HashMap();
      //paraMap.put("empID",request.getParameter("empID"));
      //slingRequest.setAttribute("paramMap",paraMap);
    }
```

empID的值與paraMap中名為empID的鍵相關聯。 此對應會傳遞至slingRequest

>[!NOTE]
>
>鍵empID必須與新實體獲取服務的綁定值匹配
