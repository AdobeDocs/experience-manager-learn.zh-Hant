---
title: 建立OSGi服務
description: 建立OSGi服務以儲存要簽名的表單
feature: 工作流程
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
thumbnail: 6886.jpg
kt: 6886
topic: 開發
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 1%

---


# 建立OSGi服務

下列程式碼是用來儲存需要簽署的表單。 每個要簽署的表單都與唯一的guid和客戶id相關聯。 因此，一個或多個表單可與相同的客戶ID關聯，但會為表單分配唯一的GUID。

## 介面

以下是使用的介面聲明

```java
package com.aem.forms.signmultipleforms;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
public interface SignMultipleForms
{
    public void insertData(String []formNames,String formData,String serverURL,WorkItem workItem, WorkflowSession workflowSession);
    public String getNextFormToSign(int customerID);
    public void updateSignatureStatus(String formData,String guid);
    public String getFormData(String guid);
}
```



## 插入資料

插入資料方法在由資料源標識的資料庫中插入一行。 資料庫中的每一列都對應一個表單，並以GUID和客戶ID唯一識別。 表單資料和表單URL也會儲存在此列中。 狀態欄是指出表單是否已填寫及簽署。 值0表示尚未簽名表單。

```java
@Override
public void insertData(String[] formNames, String formData, String serverURL, WorkItem workItem, WorkflowSession workflowSession) {
  String insertTableSQL = "INSERT INTO aemformstutorial.signingforms(formName,formData,guid,status,customerID) VALUES(?,?,?,?,?)";
  Random r = new Random();
  Connection con = getConnection();
  PreparedStatement pstmt = null;
  int customerIDGnerated = r.nextInt((1000 - 1) + 1) + 1;
  log.debug("The number of forms to insert are  " + formNames.length);
  try {
    for (int i = 0; i < formNames.length; i++) {
      log.debug("Inserting form name " + formNames[i]);
      UUID uuid = UUID.randomUUID();
      String randomUUIDString = uuid.toString();
      String formUrl = serverURL + "/content/dam/formsanddocuments/formsandsigndemo/" + formNames[i] + "/jcr:content?wcmmode=disabled&guid=" + randomUUIDString + "&customerID=" + customerIDGnerated;
      pstmt = con.prepareStatement(insertTableSQL);
      pstmt.setString(1, formUrl);
      pstmt.setString(2, formData.replace("<guid>3938</guid>", "<guid>" + uuid + "</guid>"));
      pstmt.setString(3, uuid.toString());
      pstmt.setInt(4, 0);
      pstmt.setInt(5, customerIDGnerated);
      log.debug("customerIDGnerated the insert statment  " + pstmt.execute());
      if (i == 0) {
        WorkflowData wfData = workItem.getWorkflowData();
        wfData.getMetaDataMap().put("formURL", formUrl);
        workflowSession.updateWorkflowData(workItem.getWorkflow(), wfData);
        log.debug("$$$$ Done updating the map");

}

}
    con.commit();
  }
  catch(Exception e) {
    log.debug(e.getMessage());
  }
  finally {
    if (pstmt != null) {
      try {
        pstmt.close();
      } catch(SQLException e) {

log.debug(e.getMessage());
      }
    }
    if (con != null) {
      try {
        con.close();
      } catch(SQLException e) {
        log.debug(e.getMessage());
      }
    }
  }

}
```


## 取得表單資料

下列程式碼可用來擷取與指定GUID相關聯的最適化表單資料。 之後，表單資料便可用來預先填入最適化表單。

```java
@Override
public String getFormData(String guid) {
  log.debug("### Getting form data asscoiated with guid " + guid);
  Connection con = getConnection();
  try {
    Statement st = con.createStatement();
    String query = "SELECT formData FROM aemformstutorial.signingforms where guid = '" + guid + "'" + "";
    log.debug(" The query being consrtucted " + query);
    ResultSet rs = st.executeQuery(query);
    while (rs.next()) {
      return rs.getString("formData");
    }
  } catch(SQLException e) {
    // TODO Auto-generated catch block
    log.debug(e.getMessage());
  }

  return null;

}
```

## 更新簽名狀態

成功完成簽署儀式會觸發與表單相關聯的AEM工作流程。 工作流程的第一步是程式步驟，用於更新資料庫中guid和客戶ID所識別列的狀態。 我們也會將表單資料中有符號元素的值設為Y，以指出表單已填入和有符號。 適用性表單將填入此資料，且xml資料中已簽署資料元素的值將用來顯示適當的訊息。 從自定義進程步驟中調用updateSignatureStatus代碼。


```java
public void updateSignatureStatus(String formData, String guid) {
  String updateStatment = "update aemformstutorial.signingforms SET formData = ?, status = ? where guid = ?";
  PreparedStatement updatePS = null;
  Connection con = getConnection();
  try {
    updatePS = con.prepareStatement(updateStatment);
    updatePS.setString(1, formData.replace("<signed>N</signed>", "<signed>Y</signed>"));
    updatePS.setInt(2, 1);
    updatePS.setString(3, guid);
    log.debug("Updated the signature status " + String.valueOf(updatePS.execute()));
  } catch(SQLException e) {
    log.debug(e.getMessage());
  }
  finally {
    try {
      con.commit();
      updatePS.close();
      con.close();
    } catch(SQLException e) {
      
      log.debug(e.getMessage());
    }

  }

}
```

## 取得下一個表格以簽署

下列程式碼用於取得下一個表單，以簽署狀態為0的指定customerID。 如果sql查詢未傳回任何列，我們會傳回字串&#x200B;**&quot;AllDone&quot;**，指出沒有其他表單可用於簽署指定客戶ID。

```java
@Override
public String getNextFormToSign(int customerID) {
  System.out.println("### inside my next form to sign " + customerID);
  String selectStatement = "SELECT formName FROM aemformstutorial.signingforms where status = 0 and customerID=" + customerID;
  Connection con = getConnection();
  try
   {
    Statement st = con.createStatement();
    ResultSet rs = st.executeQuery(selectStatement);
    while (rs.next()) {
      log.debug("Got result set object");
      return rs.getString("formName");
    }
    if (!rs.next()) {
      return "AllDone";
    }
  } catch(SQLException e) {
    log.debug(e.getMessage());
  }
  finally {
    try {
      con.close();
    } catch(SQLException e) {
      // TODO Auto-generated catch block
      log.debug(e.getMessage());
    }
  }

  return null;

}
```



## 資產

從這裡](assets/sign-multiple-forms.jar)下載與上述服務搭配的OSGi套件組合可以是[