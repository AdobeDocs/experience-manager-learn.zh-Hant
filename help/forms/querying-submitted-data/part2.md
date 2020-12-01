---
title: AEM Forms with JSON Schema and Data[Part2]
seo-title: AEM Forms with JSON Schema and Data[Part2]
description: 多部分教學課程，可引導您逐步瞭解使用JSON結構描述建立最適化表單及查詢提交資料的相關步驟。
seo-description: 多部分教學課程，可引導您逐步瞭解使用JSON結構描述建立最適化表單及查詢提交資料的相關步驟。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '371'
ht-degree: 0%

---


# 將提交的資料儲存在資料庫中


>[!NOTE]
>
>建議您使用MySQL 8做為資料庫，因為它支援JSON資料類型。 您還需要安裝適合MySQL DB的驅動程式。 我已使用此位置https://mvnrepository.com/artifact/mysql/mysql-connector-java/8.0.12中提供的驅動程式

為了將提交的資料儲存在資料庫中，我們將編寫一個servlet來提取綁定的資料和表單名稱並儲存。 以下提供處理表單提交並將afBoundData儲存在資料庫的完整程式碼。

我們建立自訂提交以處理表單提交。 在此自訂提交的post.POST.jsp中，我們將請求轉發到我們的servlet。

若要進一步瞭解自訂提交請求，請閱讀本[文章](https://helpx.adobe.com/experience-manager/kt/forms/using/custom-submit-aem-forms-article.html)

com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,&quot;/bin/storeafsubmission&quot;,null,null);

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

![connectionpool](assets/connectionpooled.gif)

要使系統正常工作，請遵循以下步驟

* [下載並解壓縮zip檔案](assets/aemformswithjson.zip)
* 使用JSON結構描述建立AdaptiveForm。 您可以使用本文資產中提供的JSON結構描述。 請務必正確設定表單的提交動作。 提交操作必須配置為「CustomSubmitHelpx」。
* 使用MySQL工作台工具導入schema.sql檔案，在MySQL實例中建立方案。 本教學課程資產中也會提供schema.sql檔案給您。
* 從Felix網頁主控台設定Apache Sling Connection Pooled DataSource
* 請確定您的資料來源名稱為&quot;aemformswithjson&quot;。 這是提供給您的範例OSGi Bundle所使用的名稱
* 有關屬性，請參閱上圖。 假設您將使用MySQL作為資料庫。
* 部署本文章資產中提供的OSGi套件。
* 預覽表單並送出。
* JSON資料將儲存在您匯入「schema.sql」檔案時建立的資料庫中。
