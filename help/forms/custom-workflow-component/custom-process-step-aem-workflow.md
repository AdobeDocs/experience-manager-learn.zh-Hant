---
title: 使用對話方塊實作自訂處理步驟
description: 使用自訂處理步驟將適用性表單附件寫入檔案系統
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2021-06-09T00:00:00Z
exl-id: 149d2c8c-bf44-4318-bba8-bec7e25da01b
source-git-commit: 00257efe045eb85fb192bbb47f8e178cf909eb86
workflow-type: tm+mt
source-wordcount: '455'
ht-degree: 0%

---

# 自訂處理步驟

本教學課程適用於需要實作自訂工作流程元件的AEM Forms客戶。建立工作流程元件的第一步是撰寫將與工作流程元件相關聯的java程式碼。 在本教程中，我們將編寫簡單的java類，以將最適化表單附件儲存到檔案系統。此java代碼將讀取工作流元件中指定的參數。

要編寫java類並將類部署為OSGi捆綁包，需要執行以下步驟

## 建立Maven專案

第一步是使用適當的AdobeMaven原型來建立Maven專案。 詳細步驟列於 [文章](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html). 將maven專案匯入eclipse後，您就可以開始編寫可在處理步驟中使用的第一個OSGi元件。


### 建立實施WorkflowProcess的類

在eclipse IDE中開啟maven專案。 展開 **projectname** > **核心** 檔案夾。 展開src/main/java資料夾。 您應該會看到結尾為「core」的套件。 在此包中建立實施WorkflowProcess的Java類。 您需要覆寫執行方法。 執行方法的簽名如下：公共void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)擲回WorkflowException

在本教學課程中，我們將將新增至適用性表單的附件寫入檔案系統，作為AEM工作流程的一部分。

為了完成此使用案例，寫入了以下java類

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


* attachmentsPath — 這與您在適用性表單中指定的位置相同，與您在設定適用性表單的提交動作以叫用AEM Workflow時的位置相同。 這是您要將附件儲存至AEM中，而與工作流程裝載相關的資料夾名稱。

* saveToLocation — 這是您希望將附件保存在AEM伺服器的檔案系統上的位置。

這兩個值會使用工作流程元件的對話方塊，以程式引數的形式傳遞

![ProcessStep](assets/custom-workflow-component.png)

QueryBuilder服務用於查詢attachmentsPath資料夾下nt:file類型的節點。 其餘的代碼會在搜索結果中反覆顯示，以建立Document對象並將其保存到檔案系統


>[!NOTE]
>
>由於我們使用的是AEM Forms專屬的檔案物件，因此您必須在您的Maven專案中包含aemfd-client-sdk相依性。

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.772</version>
</dependency>
```

#### 建置和部署

[依照此處所述建立套件組合](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html)
[確保已部署並處於活動狀態](http://localhost:4502/system/console/bundles)
