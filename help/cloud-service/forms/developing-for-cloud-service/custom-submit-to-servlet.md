---
title: 建立自訂提交動作處理常式
description: 將最適化表單提交至自訂提交處理常式
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Developer Tools
jira: KT-8852
exl-id: 983e0394-7142-481f-bd5e-6c9acefbfdd0
duration: 52
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '203'
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

在`apps/bankingapplication`資料夾中建立自訂提交動作，其方式與在[舊版AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/custom-submit-aem-forms-article.html?lang=zh-Hant)中建立的方式相同。 出於本教學課程的目的，我在CRX存放庫的`apps/bankingapplication`節點下建立了名為SubmitToAEMServlet的資料夾。

post.POST.jsp中的下列程式碼只會將要求轉送給掛載在/bin/formstutorial上的servlet。 這與先前步驟建立的servlet相同

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/formstutorial",null,null);
```

在IntelliJ的AEM專案中，以滑鼠右鍵按一下`apps/bankingapplication`資料夾並選取「新增」 | 在新封裝對話方塊中的apps.bankingapplication後面封裝並輸入SubmitToAEMServlet。 以滑鼠右鍵按一下SubmitToAEMServlet節點，然後選取存放庫 | 取得命令，將AEM專案與AEM伺服器存放庫同步。


## 設定最適化表單

您現在可以設定任何最適化表單，以提交至此自訂提交處理常式，稱為&#x200B;**提交至AEM Servlet**

## 後續步驟

[使用資源型別註冊servlet](./registering-servlet-using-resourcetype.md)
