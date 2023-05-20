---
title: 建立OSGi服務
description: 建立OSGi服務以儲存要簽名的表單
feature: Workflow
version: 6.4,6.5
thumbnail: 6886.jpg
kt: 6886
topic: Development
role: Developer
level: Experienced
exl-id: 49e7bd65-33fb-44d4-aaa2-50832dffffb0
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 1%

---

# 建立OSGi服務

已編寫以下代碼以儲存需要簽名的表單。 每個要簽名的表單都與唯一GUID和客戶ID關聯。 因此，一個或多個表單可以與同一客戶ID關聯，但將具有分配給表單的唯一GUID。

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

插入資料方法在資料庫中插入由資料源標識的行。 資料庫中的每一行都對應於一個表單，並由GUID和客戶ID唯一標識。 表單資料和表單URL也儲存在此行中。 狀態列用於指示表單是否已填充和簽名。 值0表示尚未簽名表單。

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


## 獲取表單資料

以下代碼用於獲取與給定GUID關聯的自適應表單資料。 然後使用表單資料來預填充自適應表單。

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

簽署儀式的成功完成觸發了與AEM表單關聯的工作流。 工作流中的第一步是一個流程步驟，該流程步驟將更新由guid和客戶id標識的行在資料庫中的狀態。 我們還將表單資料中帶符號元素的值設定為Y，以指示表單已填充和簽名。 自適應表單被填充有該資料，並且xml資料中帶符號資料元素的值被用於顯示適當的消息。 從自定義進程步驟調用updateSignatureStatus代碼。


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

## 獲取下一個表單以簽名

以下代碼用於獲取下一個用於為狀態為0的給定customerID簽名的表單。 如果sql查詢未返回任何行，則我們返回字串 **&quot;全部完成&quot;** 這表示沒有更多用於為給定客戶id簽名的表單。

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



## Assets

OSGi捆綁包與上述服務可 [從此處下載](assets/sign-multiple-forms.jar)

## 後續步驟

[建立主工作流以處理初始表單提交](./create-main-workflow.md)
