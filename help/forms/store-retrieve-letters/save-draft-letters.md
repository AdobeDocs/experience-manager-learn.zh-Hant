---
title: 儲存和擷取草稿信件
description: 瞭解如何儲存和擷取草稿信件
feature: Interactive Communication
doc-type: article
version: Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-10208
exl-id: dc6f64a0-7059-4392-9c29-e66bdef4fd4d
duration: 116
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---

# 儲存和擷取草稿信件

下列程式碼可用來儲存信件例項。 信件執行個體的中繼資料儲存在&#x200B;_icdrafts_&#x200B;資料表中。 產生並傳回唯一字串(draftID)。 然後會使用此唯一字串來擷取已儲存的信件例項。

```java
public String save(CCRDocumentInstance letterToSave) throws CCRDocumentException {
  String insertRowSQL = "INSERT INTO aemformstutorial.icdrafts(draftID,documentID,status,owner,name) VALUES(?,?,?,?,?)";
  log.debug(" in save IC Draft" + letterToSave.getDocumentId() + letterToSave.getName());
  UUID uuid = UUID.randomUUID();
  String uuidString = uuid.toString();
  Connection connection = getConnection();
  PreparedStatement pstmt = null;
  Document icData = letterToSave.getData();
  try {
    pstmt = connection.prepareStatement(insertRowSQL);
    pstmt.setString(1, uuidString);
    pstmt.setString(2, letterToSave.getDocumentId());
    pstmt.setString(3, "DRAFT");
    pstmt.setString(4, letterToSave.getCreatedBy());
    pstmt.setString(5, letterToSave.getName());
    icData.copyToFile(new File(uuidString + ".xml"));
    log.debug("Executing the insert statment  " + pstmt.executeUpdate());
    connection.commit();
  } catch (IOException | SQLException e) {
    log.debug("The error is " + e.getMessage());
  } finally {
    if (pstmt != null) {
      try {
        pstmt.close();
      } catch (SQLException e) {
        log.debug("Error in closing prepared statment" + e.getMessage());
      }
    }
    if (connection != null) {
      try {
        log.debug("Closing the connection in Save Letter Draft");
        connection.close();
      } catch (SQLException e) {
        log.debug("Error in closing connection" + e.getMessage());
      }
    }

  }

  return uuidString;
}
```

## 取得字母

下列程式碼是用來擷取已儲存的草稿字母。
若要載入已儲存的信件例項，您必須提供draftID。 根據此draftID，我們查詢資料庫以取得信件的其他中繼資料。 透過從檔案系統讀取適當的xml，相同的draftID可用於建立信函資料。 接著會建構並傳回CCRDocumentInstance物件。


```java
@Override
public CCRDocumentInstance get(String draftID) throws CCRDocumentException {

  String selectStatement = "Select documentID from aemformstutorial.icdrafts where draftID='" + draftID + "'";
  log.debug("The select statement is " + selectStatement);
  Connection connection = getConnection();
  Statement statement = null;
  String documentID = "";
  try {
    statement = connection.createStatement();
    ResultSet rs = statement.executeQuery(selectStatement);
    while (rs.next()) {
      documentID = rs.getString("documentID");

    }
  } catch (SQLException e) {
    log.debug("The error is " + e.getMessage());
  }
  Document draftData = new Document(new File(draftID + ".xml"));
  CCRDocumentInstance draftInstance = new CCRDocumentInstance(draftData, "abc", documentID, CCRDocumentInstance.Status.DRAFT);
  draftInstance.setId(draftID);
  return draftInstance;
}
```

### 更新字母

下列程式碼已用於更新已儲存的信件例項。 更新後的信件資料會使用信件ID寫入檔案系統。

```java
public void update(CCRDocumentInstance letterInstanceToUpdate) throws CCRDocumentException {
        Document icData = letterInstanceToUpdate.getData();
        String draftID = letterInstanceToUpdate.getId();
        log.debug("updating letter instance with draft id =  "+draftID);
        try
            {
                icData.copyToFile(new File(draftID+".xml"));
            } 
        catch (IOException e)
            {
                log.debug("Error updating "+e.getMessage());;
            }
        
    }
```

### 取得所有儲存的字母

AEM Forms未提供任何立即可用的使用者介面來列出已儲存的字母。 針對本文，我使用最適化表單以表格格式列出已儲存的信件例項。
您可以自訂查詢以擷取儲存的信件例項。 在此範例中，我正在查詢由「管理員」儲存的信件例項。

```java
    public List < CCRDocumentInstance > getAll(String arg0, Date arg1, Date arg2, Map < String, Object > arg3) throws CCRDocumentException {
      String selectStatement = "Select * from aemformstutorial.icdrafts where owner = 'admin'";
      Connection connection = getConnection();
      Statement statement = null;
      String documentID = "";
      List < CCRDocumentInstance > listOfDrafts = new ArrayList < CCRDocumentInstance > ();
      String draftID;
      String savedInstanceName = "";
      try {
        statement = connection.createStatement();
        ResultSet rs = statement.executeQuery(selectStatement);
        while (rs.next()) {
          documentID = rs.getString("documentID");
          draftID = rs.getString("draftID");
          savedInstanceName = rs.getString("name");
          Document draftData = new Document(new File(draftID + ".xml"));
          CCRDocumentInstance draftLetter = new CCRDocumentInstance(draftData, savedInstanceName, documentID, CCRDocumentInstance.Status.DRAFT);
          listOfDrafts.add(draftLetter);
        }
      } catch (SQLException e) {
        log.debug("The error is " + e.getMessage());
      } finally {
        if (statement != null) {
          try {
            statement.close();
          } catch (SQLException e) {
            log.debug("error in closing statement" + e.getMessage());
          }
        }
        if (connection != null) {
          try {
            connection.close();
          } catch (SQLException e) {
            log.debug("error in closing connection" + e.getMessage());
          }
        }
      }

      return listOfDrafts;
    }
```

### Eclipse專案

包含範例實作的eclipse專案可從這裡[下載](assets/icdrafts-eclipse-project.zip)
