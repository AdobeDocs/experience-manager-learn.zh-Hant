---
title: AEM Forms，帶JSON架構和資料[Part2]
seo-title: AEM Forms with JSON Schema and Data[Part2]
description: 多部分教程，引導您完成建立帶JSON架構的自適應表單和查詢提交資料所涉及的步驟。
seo-description: Multi-Part tutorial to walk you through the steps involved in creating Adaptive Form with JSON schema and querying the submitted data.
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 29195c70-af12-4a22-8484-3c87a1e07378
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '342'
ht-degree: 0%

---

# 將提交的資料儲存在資料庫中


>[!NOTE]
>
>建議將MySQL 8用作資料庫，因為它支援JSON資料類型。 您還需要為MySQL資料庫安裝相應的驅動程式。 我已使用此位置https://mvnrepository.com/artifact/mysql/mysql-connector-java/8.0.12中的驅動程式

為了將提交的資料儲存到資料庫中，我們編寫一個Servlet來提取綁定的資料以及表單的名稱和儲存。 下面提供了處理表單提交和將afBoundData儲存在資料庫中的完整代碼。

我們建立了自定義提交以處理表單提交。 在此自定義提交的post.POST.jsp中，我們將請求轉發到我們的servlet。

要瞭解有關自定義提交請求的詳細資訊，請閱讀此 [文章](https://helpx.adobe.com/experience-manager/kt/forms/using/custom-submit-aem-forms-article.html)

com.adobe.aemds.guide.utils.GuideUtils.setForwardPath(slingRequest,&quot;/bin/storeafsubmission&quot;,null,null);

```java
package com.aemforms.json.core.servlets;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import javax.servlet.Servlet;
import javax.servlet.ServletException;
import javax.sql.DataSource;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.json.JSONException;
import org.json.JSONObject;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(service = Servlet.class, property = {

"sling.servlet.methods=get", "sling.servlet.methods=post",

"sling.servlet.paths=/bin/storeafsubmission"

})
public class HandleAdaptiveFormSubmission extends SlingAllMethodsServlet {
 private static final Logger log = LoggerFactory.getLogger(HandleAdaptiveFormSubmission.class);
 private static final long serialVersionUID = 1L;
 @Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=aemformswithjson))")
 private DataSource dataSource;

 protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws ServletException {
  JSONObject afSubmittedData;
  try {
   afSubmittedData = new JSONObject(request.getParameter("jcr:data"));
   // we will only store the data bound to schema
   JSONObject dataToStore = afSubmittedData.getJSONObject("afData").getJSONObject("afBoundData")
     .getJSONObject("data");
   String formName = afSubmittedData.getJSONObject("afData").getJSONObject("afSubmissionInfo")
     .getString("afPath");
   log.debug("The form name is " + formName);
   insertData(dataToStore, formName);

  } catch (JSONException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }

 }

 public void insertData(org.json.JSONObject jsonData, String formName) {
  log.debug("The json object I got to insert was " + jsonData.toString());
  String insertTableSQL = "INSERT INTO aemformswithjson.formsubmissions(formdata,formname) VALUES(?,?)";
  log.debug("The query is " + insertTableSQL);
  Connection c = getConnection();
  PreparedStatement pstmt = null;
  try {
   pstmt = null;
   pstmt = c.prepareStatement(insertTableSQL);
   pstmt.setString(1, jsonData.toString());
   pstmt.setString(2, formName);
   log.debug("Executing the insert statment  " + pstmt.executeUpdate());
   c.commit();
  } catch (SQLException e) {

   log.error("Getting errors", e);
  } finally {
   if (pstmt != null) {
    try {
     pstmt.close();
    } catch (SQLException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
    }
   }
   if (c != null) {
    try {
     c.close();
    } catch (SQLException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
    }
   }
  }
 }

 public Connection getConnection() {
  log.debug("Getting Connection ");
  Connection con = null;
  try {

   con = dataSource.getConnection();
   log.debug("got connection");
   return con;
  } catch (Exception e) {
   log.error("not able to get connection ", e);
  }
  return null;
 }

}
```

![連接池](assets/connectionpooled.gif)

要使系統正常工作，請執行以下步驟

* [下載並解壓縮zip檔案](assets/aemformswithjson.zip)
* 使用JSON架構建立AdaptiveForm。 可以使用作為本文章資產一部分提供的JSON架構。 確保已正確配置表單的提交操作。 提交操作需要配置為「CustomSubmitHelpx」。
* 通過使用MySQL Workbench工具導入schema.sql檔案，在MySQL實例中建立架構。 本教程資產中還提供schema.sql檔案。
* 從Felix Web控制台配置Apache Sling連接池化資料源
* 確保將資料源名稱命名為「aemformswithjson」。 這是提供給您的示例OSGi捆綁包使用的名稱
* 有關屬性，請參閱上圖。 這假定您將使用MySQL作為資料庫。
* 部署作為本文章資產一部分提供的OSGi捆綁包。
* 預覽表單並提交。
* JSON資料將儲存在導入&quot;schema.sql&quot;檔案時建立的資料庫中。
