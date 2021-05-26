---
title: 在AEM Forms中建立您的第一個servlet
description: 建置您的第一個Sling Servlet，將資料與表單範本合併。
feature: 適用性表單
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
topic: 開發
role: Developer
level: Beginner
source-git-commit: c74c6f5627e69e32bbf0098d6b6bab122cace798
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 2%

---


# Sling Servlet

Servlet是一種類，用於擴展通過請求響應寫程式模型訪問的托管應用程式的伺服器的能力。 對於這些應用程式，Servlet技術定義了HTTP特定的Servlet類。
所有servlet都必須實作定義生命週期方法的Servlet介面。


AEM中的Servlet可註冊為OSGi服務：您可以擴充SlingSafeMethodsServlet以進行唯讀實作，或擴充SlingAllMethodsServlet，以實作所有RESTful操作。

## Servlet程式碼

```java
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

## 建置和部署

若要建置專案，請依照下列步驟操作：

* 開啟&#x200B;**命令提示窗口**
* 導航到 `c:\aemformsbundles\learningaemforms\core`
* 執行命令`mvn clean install -PautoInstallBundle`
* 上述命令會自動建立套件組合併部署至localhost:4502上執行的AEM執行個體

此套件也可在以下位置`C:\AEMFormsBundles\learningaemforms\core\target`使用。 此套件也可透過[Felix Web主控台部署至AEM。](http://localhost:4502/system/console/bundles)


## 測試Servlet解析器

將瀏覽器指向[servlet解析器URL](http://localhost:4502/system/console/servletresolver?url=%2Fbin%2FmergedataWithAcroform&amp;method=POST)。 這會告訴您將針對指定路徑叫用的servlet，如下方螢幕擷取畫面所示
![servlet-resolver](assets/servlet-resolver.JPG)

## 使用Postman測試Servlet

![test-servlet-postman](assets/test-servlet-postman.JPG)
