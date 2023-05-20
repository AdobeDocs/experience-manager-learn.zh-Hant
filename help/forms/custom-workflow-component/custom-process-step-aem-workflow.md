---
title: 使用對話框實現自定義流程步驟
description: 使用自定義進程步驟將自適應表單附件寫入檔案系統
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2021-06-09T00:00:00Z
exl-id: 149d2c8c-bf44-4318-bba8-bec7e25da01b
source-git-commit: 38e0332ef2ef45a73a81f318975afc25600392a8
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 0%

---

# 自定義流程步驟

本教程適用於需要實施自定義工作流元件的AEM Forms客戶。建立工作流元件的第一步是編寫將與工作流元件關聯的Java代碼。 在本教程中，我們將編寫簡單的java類以將自適應表單附件儲存到檔案系統。此Java代碼將讀取工作流元件中指定的參數。

編寫java類並將類部署為OSGi捆綁包需要執行以下步驟

## 建立Maven項目

第一步是使用適當的AdobeMaven Archetype建立主項目。 詳細步驟列在本中 [文章](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html)。 將主項目導入eclipse後，您就可以開始編寫可以在流程步驟中使用的第一個OSGi元件。


### 建立實現WorkflowProcess的類

在eclipse IDE中開啟maven項目。 展開 **項目名稱** > **核** 的子菜單。 展開src/main/java資料夾。 您應看到以「核心」結尾的包。 建立在此包中實現WorkflowProcess的Java類。 您需要覆蓋execute方法。 執行方法的簽名如下：public void execute(WorkItem workItem、WorkflowSession workflowSession、MetaDataMap processArguments)拋出WorkflowException

在本教程中，我們將將添加到「自適應表單」的附件作為「工作流」的一部分寫入檔案AEM系統。

要完成此用例，請編寫以下java類

讓我們看看這個代碼

```java
package com.mysite.core;
import java.io.File;
import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;
import javax.jcr.Node;
import javax.jcr.Session;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.day.cq.search.PredicateGroup;
import com.day.cq.search.Query;
import com.day.cq.search.QueryBuilder;
import com.day.cq.search.result.Hit;
import com.day.cq.search.result.SearchResult;
@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=Custom component to wrtie form attachments to file system",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Custom component to wrtie form attachments to file system"
})
public class WriteFormAttachmentsToFileSystem implements WorkflowProcess {

  private static final Logger log = LoggerFactory.getLogger(WriteFormAttachmentsToFileSystem.class);
  @Reference
  QueryBuilder queryBuilder;

  @Override
  public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap metaDataMap)
  throws WorkflowException {

    String attachmentsPath = metaDataMap.get("attachmentsPath", String.class);

    log.debug("Got attachments path: " + attachmentsPath);
    String saveToLocation = metaDataMap.get("SaveToLocation", String.class);
    log.debug("Got save location: " + saveToLocation);

    log.debug("The seperator is" + File.separator);
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    Map < String, String > map = new HashMap < String, String > ();
    map.put("path", payloadPath + "/" + attachmentsPath);
    File saveLocationFolder = new File(saveToLocation);
    if (!saveLocationFolder.exists()) {
      saveLocationFolder.mkdirs();
    }

    map.put("type", "nt:file");
    Query query = queryBuilder.createQuery(PredicateGroup.create(map), workflowSession.adaptTo(Session.class));
    query.setStart(0);
    query.setHitsPerPage(20);

    SearchResult result = query.getResult();
    log.debug("Got  " + result.getHits().size() + " attachments ");
    Node attachmentNode = null;
    for (Hit hit: result.getHits()) {
      try {
        String path = hit.getPath();
        log.debug("The attachment title is  " + hit.getTitle() + " and the attachment path is  " + path);
        attachmentNode = workflowSession.adaptTo(Session.class).getNode(path + "/jcr:content");
        InputStream documentStream = attachmentNode.getProperty("jcr:data").getBinary().getStream();
        Document attachmentDoc = new Document(documentStream);
        attachmentDoc.copyToFile(new File(saveLocationFolder + File.separator + hit.getTitle()));
        attachmentDoc.close();
      } catch (Exception e) {
        log.error("Error saving file " + e.getMessage());
      }
    }
  }
}
```


* attachmentsPath — 這與在將Adaptive Form的提交操作配置為調用工作流時在Adaptive Form中指定的位置AEM相同。 這是您希望附件相對於工作流負載保存AEM的資料夾的名稱。

* saveToLocation — 這是您希望將附件保存到伺服器文AEM件系統的位置。

這兩個值將作為進程參數使用工作流元件的對話框傳遞

![進程步驟](assets/custom-workflow-component.png)

QueryBuilder服務用於查詢attachmentsPath資料夾下nt:file類型的節點。 其餘的代碼通過搜索結果迭代以建立文檔對象並將其保存到檔案系統


>[!NOTE]
>
>由於我們使用的是特定於AEM Forms的Document對象，因此需要在主項目中包含aemfd-client-sdk依賴項。

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.772</version>
</dependency>
```

#### 構建和部署

[按此處所述構建捆綁包](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html)
[確保已部署該捆綁包並處於活動狀態](http://localhost:4502/system/console/bundles)

## 後續步驟

建立 [自定義工作流元件](./custom-workflow-component.md)

