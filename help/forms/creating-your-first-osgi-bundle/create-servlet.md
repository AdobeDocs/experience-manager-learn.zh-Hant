---
title: 在AEM Forms建立第一個Servlet
description: 構建第一個sling servlet以將資料與表單模板合併。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 72728ed7-80a2-48b5-ae7f-d744db8a524d
source-git-commit: f4e86059d29acf402de5242f033a25f913febf36
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 0%

---

# Sling Servlet

Servlet是一種類，用於擴展通過請求 — 響應寫程式模型訪問的應用程式所承載的伺服器的能力。 對於這些應用程式，Servlet技術定義HTTP特定的Servlet類。
所有Servlet都必須實現Servlet介面，該介面定義生命週期方法。


中的ServletAEM可以註冊為OSGi服務：您可以擴展SlingSafeMethodsServlet用於只讀實現或SlingAllMethodsServlet，以便實現所有REST風格的操作。

## Servlet代碼

```java
package com.mysite.core.servlets;
import javax.servlet.Servlet;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.FormsService;

@Component(service={Servlet.class}, property={"sling.servlet.methods=post", "sling.servlet.paths=/bin/mergedataWithAcroform"})
public class MyFirstAEMFormsServlet extends SlingAllMethodsServlet
{
	
	private static final long serialVersionUID = 1L;
	@Reference
	FormsService formsService;
	 protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response)
	  { 
		 String file_path = request.getParameter("save_location");
		 
		 java.io.InputStream pdf_document_is = null;
		 java.io.InputStream xml_is = null;
		 javax.servlet.http.Part pdf_document_part = null;
		 javax.servlet.http.Part xml_data_part = null;
		 	 try
		 	 {
		 		pdf_document_part = request.getPart("pdf_file");
				 xml_data_part = request.getPart("xml_data_file");
				 pdf_document_is = pdf_document_part.getInputStream();
				 xml_is = xml_data_part.getInputStream();
				 Document data_merged_document = formsService.importData(new Document(pdf_document_is), new Document(xml_is));
				 data_merged_document.copyToFile(new File(file_path));
				 
		 	 }
		 	 catch(Exception e)
		 	 {
		 		 response.sendError(400,e.getMessage());
		 	 }
	  }
}
```

## 構建和部署

要構建項目，請執行以下步驟：

* 開啟 **命令提示符窗口**
* 導航到 `c:\aemformsbundles\mysite\core`
* 執行命令 `mvn clean install -PautoInstallBundle`
* 以上命令將自動生成捆綁包並將其部署到在localhost:4502AEM上運行的實例

此捆綁包也將位於以下位置 `C:\AEMFormsBundles\mysite\core\target`。 此捆綁包也可以部署AEM到 [Felix網路控制台。](http://localhost:4502/system/console/bundles)


## TestServlet解析程式

將瀏覽器指向 [servlet解析程式URL](http://localhost:4502/system/console/servletresolver?url=%2Fbin%2FmergedataWithAcroform&amp;method=POST)。 這將告訴您將為給定路徑調用的Servlet，如下面螢幕抓圖所示
![Servlet — 解析器](assets/servlet-resolver.JPG)

## Test使用Postman的Servlet

![test-servlet-postman](assets/test-servlet-postman.JPG)
