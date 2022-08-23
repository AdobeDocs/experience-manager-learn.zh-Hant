---
title: 建立自定義提交操作處理程式
description: 向自定義提交處理程式提交自適應表單
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8852
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 0%

---

# 建立Servlet以處理提交的資料

在IntelliJ中啟動您的銀行項目。
建立一個簡單的servlet，將提交的資料輸出到日誌檔案。確保代碼位於核心項目中，如下圖所示
![建立servlet](assets/create-servlet.png)

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

## 建立自定義提交

在應用程式/銀行應用程式資料夾中建立自定義提交與在 [早期版本的AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/custom-submit-aem-forms-article.html?lang=en)
post.POST.jsp中的以下代碼只是將請求轉發到/bin/formstuvial上裝載的servlet。 這是在前一步驟中建立的Servlet

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/formstutorial",null,null);
```

## 配置自適應窗體

現在，您可以將自適應表單配置為提交到此自定義提交處理程式，該提交處理程式名為 **提交到AEMServlet**



