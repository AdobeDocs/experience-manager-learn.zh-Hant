---
title: 建立自訂提交動作處理常式
description: 將最適化表單提交至自訂提交處理常式
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Developer Tools
jira: KT-8852
exl-id: 983e0394-7142-481f-bd5e-6c9acefbfdd0
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# 建立servlet以處理提交的資料

在IntelliJ上啟動您的aem-banking專案。
建立簡單的servlet，將提交的資料輸出到記錄檔。請確定程式碼位於核心專案中，如下列熒幕擷圖所示
![create-servlet](assets/create-servlet.png)

```java
package com.aem.bankingapplication.core.servlets;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import javax.servlet.Servlet;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.osgi.service.component.annotations.Component;
@Component(service = { Servlet.class}, property = {"sling.servlet.methods=post","sling.servlet.paths=/bin/formstutorial"})
public class HandleFormSubmissison extends SlingAllMethodsServlet {
    private static final Logger log = LoggerFactory.getLogger(HandleFormSubmissison.class);
    protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response) {
        log.debug("Inside my formstutorial servlet");
        log.debug("The form data I got was "+request.getParameter("jcr:data"));
    }
}
```

## 建立自訂提交處理常式

在中建立您的自訂提交動作 `apps/bankingapplication` 資料夾建立方式，與在 [舊版AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/custom-submit-aem-forms-article.html?lang=en). 出於本教學課程的目的，我在 `apps/bankingapplication` CRX存放庫中的節點。

post.request.jsp中的下列程式碼只會將POST轉送給掛接在/bin/formstutorial上的servlet。 這與先前步驟建立的servlet相同

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/formstutorial",null,null);
```

在IntelliJ的AEM專案中，以滑鼠右鍵按一下 `apps/bankingapplication` 資料夾並選取新增 |在新封裝對話方塊中的apps.bankingapplication之後封裝並輸入SubmitToAEMServlet。 以滑鼠右鍵按一下SubmitToAEMServlet節點，然後選取存放庫 |取得命令以將AEM專案與AEM伺服器存放庫同步。


## 設定最適化表單

您現在可以設定任何最適化表單，以提交至這個自訂提交處理常式，稱為 **提交至AEM Servlet**

## 後續步驟

[使用資源型別註冊servlet](./registering-servlet-using-resourcetype.md)
