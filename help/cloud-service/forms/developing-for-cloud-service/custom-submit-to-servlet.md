---
title: 建立自訂提交動作處理常式
description: 將最適化表單提交至自訂提交處理常式
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 8852
source-git-commit: d42fd02b06429be1b847958f23f273cf842d3e1b
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 0%

---

# 建立Servlet以處理提交的資料

在IntelliJ中啟動您的aem-banking專案。
建立簡單的Servlet以將提交的資料輸出到日誌檔案。請確保代碼位於核心項目中，如下面螢幕截圖所示
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

## 建立自訂提交

在應用程式/銀行應用程式資料夾中建立自訂提交的方式，與在 [舊版AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/custom-submit-aem-forms-article.html?lang=en)
post.POST.jsp中的以下代碼只需將請求轉發到/bin/formstutorial上裝載的servlet。 這與先前步驟中建立的Servlet相同

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/formstutorial",null,null);
```

## 設定最適化表單

您現在可以設定您的適用性表單，以提交至此自訂提交處理常式，稱為 **提交至AEM Servlet**



