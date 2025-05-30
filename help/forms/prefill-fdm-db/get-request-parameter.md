---
title: 取得請求引數
description: 使用表單資料模型的預填服務存取請求引數
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-5815
thumbnail: kt-5815.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a640539d-c67f-4224-ad81-dd0b62e18c79
duration: 40
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '176'
ht-degree: 1%

---

# 取得請求引數

## 取得empID引數

下一步是從url存取empID引數。 然後，empID要求引數的值會傳遞至表單資料模型的&#x200B;**_get_**&#x200B;服務作業。
出於本課程的目的，我們已建立並提供下列內容

* 已呼叫&#x200B;**_FDMDemo_**&#x200B;的最適化表單範本
* 名為&#x200B;**_fdmdemo_**&#x200B;的頁面元件
* 包含我們的自訂jsp與頁面元件
* 將自適應表單範本與頁面元件建立關聯

如此一來，我們的自訂jsp中的程式碼只有在根據此自訂範本的自適應表單轉譯時才會執行

* [&#128279;](assets/template-page-component.zip)使用[封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)匯入封裝
* [開啟fdmrequest.jsp](http://localhost:4502/crx/de/index.jsp#/apps/fdmdemo/component/page/fdmdemo/fdmrequest.jsp)
* 取消註解註解的行。
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

empID的值與paraMap中稱為empID的索引鍵相關聯。 然後此對應會傳遞至slingRequest

>[!NOTE]
>
>金鑰empID必須與newhere entities get服務的繫結值相符

## 後續步驟

[根據表單資料模型建立最適化表單](./create-adaptive-form.md)
