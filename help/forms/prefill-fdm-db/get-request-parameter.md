---
title: 取得要求參數
description: 在表單資料模型的預填服務中存取請求參數
feature: Adaptive Forms
version: 6.4,6.5
kt: 5815
thumbnail: kt-5815.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a640539d-c67f-4224-ad81-dd0b62e18c79
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 3%

---

# 取得要求參數

## 獲取empID參數

下一步是從URL中存取empID參數。 然後，empID請求參數的值會傳遞至 **_get_** 表單資料模型的服務操作。
為了本課程的目的，我們建立並提供了以下內容

* 已呼叫適用性表單範本 **_FDMDemo_**
* 已呼叫頁面元件 **_fdmdemo_**
* 已將自定義jsp與頁面元件一起包含
* 將最適化表單範本與頁面元件相關聯

通過執行此操作，我們在自訂jsp中的程式碼只有在根據此自訂範本呈現適用性表單時才會執行

* [匯入套件](assets/template-page-component.zip) 使用 [封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)
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

## 後續步驟

[根據表單資料模型建立最適化表單](./create-adaptive-form.md)
