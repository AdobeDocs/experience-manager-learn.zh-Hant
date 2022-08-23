---
title: 儲存自適應表單資料
description: 將自適應表單資料儲存到DataBase中作為工作流的一AEM部分
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 3dd552da-fc7c-4fc7-97ec-f20b6cc33df0
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '408'
ht-degree: 1%

---

# 將自適應表單提交儲存在資料庫中

在您選擇的資料庫中儲存已提交表單資料的方法有很多。 JDBC資料源可用於將資料直接儲存到資料庫中。 可以編寫自定義OSGI捆綁包以將資料儲存到資料庫中。 本文在工作流中使用自定義AEM的流程步驟來儲存資料。
使用案例是在自適應表AEM單提交上觸發工作流，工作流中的一個步驟將提交的資料儲存到資料庫中。



## JDBC連接池

* 轉到 [ConfigMgr](http://localhost:4502/system/console/configMgr)

   * 搜索「JDBC連接池」。 建立新的「日共用」JDBC連接池。 指定特定於資料庫的設定。

   * ![jdbc連接池](assets/aemformstutorial-jdbc.png)

## 指定資料庫詳細資訊

* 搜索「」**指定資料庫詳細資訊**&quot;
* 指定特定於資料庫的屬性。
   * DataSourceName：先前配置的資料源的名稱。
   * TableName — 要在其中儲存AF資料的表的名稱
   * FormName — 用於保存表單名稱的列名
   * ColumnName — 用於保存AF資料的列名

![插入資料](assets/specify-database-details.png)



## OSGi配置的代碼

```java
package com.aemforms.dbsamples.core.insertFormData;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;

@ObjectClassDefinition(name = "Specify Database details", description = "Specify Database details")

public @interface InsertFormDataConfiguration {
  @AttributeDefinition(name = "DataSourceName", description = "Data Source Name configured")
  String dataSourceName() default "";
  @AttributeDefinition(name = "TableName", description = "Name of the table")
  String tableName() default "";
  @AttributeDefinition(name = "FormName", description = "Column Name for form name")
  String formName() default "";
  @AttributeDefinition(name = "columnName", description = "Column Name for form data")
  String columnName() default "";

}
```

## 讀取配置值

```java
package com.aemforms.dbsamples.core.insertFormData;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;

@Component(service={InsertFormDataConfigurationService.class})
@Designate(ocd=InsertFormDataConfiguration.class)

public class InsertFormDataConfigurationService {
    public String TABLE_NAME;
    public String DATA_SOURCE_NAME;
    public String COLUMN_NAME;
    public String FORM_NAME;
    @Activate      
      protected final void activate(InsertFormDataConfiguration insertFormDataConfiguration)
      {
        TABLE_NAME = insertFormDataConfiguration.tableName();
        DATA_SOURCE_NAME = insertFormDataConfiguration.dataSourceName();
        COLUMN_NAME = insertFormDataConfiguration.columnName();
        FORM_NAME = insertFormDataConfiguration.formName();
      }
    public String getTABLE_NAME()
    {
        return TABLE_NAME;
    }
    public String getDATA_SOURCE_NAME()
    {
        return DATA_SOURCE_NAME;
    }
    public String getCOLUMN_NAME()
    {
        return COLUMN_NAME;
    }
    public String getFORM_NAME()
    {
        return FORM_NAME;
    }
}
```

## 用於實現流程步驟的代碼

```java
package com.aemforms.dbsamples.core.insertFormData;
import java.io.InputStream;
import java.io.StringWriter;
import java.nio.charset.StandardCharsets;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import javax.jcr.Node;
import javax.jcr.Session;
import javax.sql.DataSource;

import org.apache.commons.io.IOUtils;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.day.commons.datasource.poolservice.DataSourcePool;

@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=Insert Form Data in Database",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Insert Form Data in Database"
})

public class InsertAfData implements WorkflowProcess {
  @Reference
  InsertFormDataConfigurationService insertFormDataConfig;
  @Reference
  DataSourcePool dataSourcePool;
  private final Logger log = LoggerFactory.getLogger(getClass());
  @Override
  public void execute(WorkItem workItem, WorkflowSession session, MetaDataMap metaDataMap) throws WorkflowException {

    String proccesArgsVals = (String) metaDataMap.get("PROCESS_ARGS", (Object)
      "string");
    String[] values = proccesArgsVals.split(",");
    String AdaptiveFormName = values[0];
    String formDataFile = values[1];
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    Session jcrSession = (Session) session.adaptTo((Class) Session.class);
    String dataFilePath = payloadPath + "/" + formDataFile + "/jcr:content";
    log.debug("The data file path is " + dataFilePath);
    PreparedStatement ps = null;
    Connection con = null;
    DataSource dbSource = null;

    try {
      dbSource = (DataSource) dataSourcePool.getDataSource(insertFormDataConfig.getDATA_SOURCE_NAME());
      log.debug("Got db source");
      con = dbSource.getConnection();

      Node xmlDataNode = jcrSession.getNode(dataFilePath);
      InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
      StringWriter writer = new StringWriter();
      String encoding = StandardCharsets.UTF_8.name();
      IOUtils.copy(xmlDataStream, writer, encoding);
      String queryStmt = "insert into " + insertFormDataConfig.TABLE_NAME + "(" + insertFormDataConfig.COLUMN_NAME + "," + insertFormDataConfig.FORM_NAME + ") values(?,?)";
      log.debug("The query Stmt is " + queryStmt);
      ps = con.prepareStatement(queryStmt);
      ps.setString(1, writer.toString());
      ps.setString(2, AdaptiveFormName);
      ps.executeUpdate();

    } catch (Exception e) {
      log.debug("The error message is " + e.getMessage());
    } finally {
      if (ps != null) {
        try {
          ps.close();
        } catch (SQLException sqlException) {
          log.debug(sqlException.getMessage());
        }
      }
      if (con != null) {
        try {
          con.close();
        } catch (SQLException sqlException) {
          log.error("Unable to close connection to database", sqlException);
        }
      }
    }
  }

}
```

## 部署示例資產

* 確保已配置JDBC連接池
* 使用configMgr指定資料庫詳細資訊
* [下載Zip檔案並將其內容解壓到硬碟上](assets/article-assets.zip)

   * 使用部署jar檔案 [AEM Web控制台](http://localhost:4502/system/console/bundles)。 此jar檔案包含用於在資料庫中儲存表單資料的代碼。

   * 將兩個ZIP檔案導入 [AEM使用包管理器](http://localhost:4502/crx/packmgr/index.jsp)。 這樣你就能 [示例工作流](http://localhost:4502/editor.html/conf/global/settings/workflow/models/storeformdata.html) 和 [樣本自適應格式](http://localhost:4502/editor.html/content/forms/af/addformdataindb.html) 觸發表單提交時的工作流。 注意工作流步驟中的進程參數。 這些參數指示將包含自適應表單資料的資料檔案的格式名稱和名稱。 資料檔案儲存在crx儲存庫的負載資料夾下。 注意 [自適應形式](http://localhost:4502/editor.html/content/forms/af/addformdataindb.html) 配置為在提交時AEM觸發工作流和資料檔案配置(data.xml)

   * 預覽並填寫表單並提交。 您應看到在資料庫中建立的新行

