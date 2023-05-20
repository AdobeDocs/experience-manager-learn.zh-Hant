---
title: 儲存表單資料
description: 將表單資料與新附件映射一起儲存在資料庫中
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6538
thumbnail: 6538.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 2bd9fe63-8f42-4b89-95a0-13ade49bc31b
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '75'
ht-degree: 2%

---

# 儲存表單資料

下一步是建立一個服務，以在資料庫中插入新行，以儲存自適應表單資料和相關的附件。
下面的螢幕抓圖顯示資料庫中的一行。


![示例行](assets/sample-row.JPG)


以下代碼在資料庫中插入一行，其中包含相應的資料

```java
public String storeFormData(String formData, String attachmentsInfo, String telephoneNumber) {
    log.debug("******Inside my AEMFormsWith DB service*****");
    log.debug("### Inserting data ... " + formData + "and the telephone number to insert is  " + telephoneNumber);
    String insertRowSQL = "INSERT INTO aemformstutorial.formdatawithattachments(guid,afdata,attachmentsInfo,telephoneNumber) VALUES(?,?,?,?)";
    UUID uuid = UUID.randomUUID();
    String randomUUIDString = uuid.toString();
    log.debug("The insert query is " + insertRowSQL);
    Connection c = getConnection();
    PreparedStatement pstmt = null;
    try {
        pstmt = null;
        pstmt = c.prepareStatement(insertRowSQL);
        pstmt.setString(1, randomUUIDString);
        pstmt.setString(2, formData);
        pstmt.setString(3, attachmentsInfo);
        pstmt.setString(4, telephoneNumber);
        log.debug("Executing the insert statment  " + pstmt.executeUpdate());
        c.commit();
    } catch (SQLException e) {

        log.error("unable to insert data in the table", e.getMessage());
    } finally {
        if (pstmt != null) {
            try {
                pstmt.close();
            } catch (SQLException e) {
                log.debug("error in closing prepared statement " + e.getMessage());
            }
        }
        if (c != null) {
            try {
                c.close();
            } catch (SQLException e) {
                log.debug("error in closing connection " + e.getMessage());
            }
        }
    }
    return randomUUIDString;
}
```

## 後續步驟

[實施保存並退出](./create-servlet.md)

