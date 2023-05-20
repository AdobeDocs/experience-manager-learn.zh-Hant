---
title: 獲取請求參數
description: 訪問表單資料模型的預填充服務的請求參數
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

# 獲取請求參數

## 獲取empID參數

下一步是從url訪問empID參數。 然後，將empID請求參數的值傳遞給 **_得_** 服務操作表單資料模型。
為了在本課程中學習，我們建立並提供了以下內容

* 已調用自適應表單模板 **_FDMemo_**
* 已調用頁面元件 **_fdmdemo_**
* 將自定義jsp與頁面元件一起包含
* 將自適應表單模板與頁面元件關聯

通過執行此操作，只有在呈現基於此自定義模板的自適應表單時，才會執行自定義jsp中的代碼

* [導入包](assets/template-page-component.zip) 使用 [軟體包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* [開啟fdmrequest.jsp](http://localhost:4502/crx/de/index.jsp#/apps/fdmdemo/component/page/fdmdemo/fdmrequest.jsp)
* 取消注釋注釋注釋行。
* 保存更改

```java
if(request.getParameter("empID")!=null)
    {
      //System.out.println("Adobe !!!There is a empID parameter in the request "+request.getParameter("empID"));
      //java.util.Map paraMap = new java.util.HashMap();
      //paraMap.put("empID",request.getParameter("empID"));
      //slingRequest.setAttribute("paramMap",paraMap);
    }
```

empID的值與paraMap中名為empID的鍵相關聯。 然後，此映射將傳遞給slingRequest

>[!NOTE]
>
>密鑰empID必須與新實體獲取服務的綁定值匹配

## 後續步驟

[基於表單資料模型建立自適應表單](./create-adaptive-form.md)
