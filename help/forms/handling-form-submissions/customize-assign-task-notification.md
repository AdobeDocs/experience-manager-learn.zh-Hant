---
title: 自定義分配任務通知
description: 在分配任務通知電子郵件中包括表單資料
sub-product: forms
feature: Workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
kt: 6279
thumbnail: KT-6279.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 0cb74afd-87ff-4e79-a4f4-a4634ac48c51
source-git-commit: eb2a807587ab918be82d00d50bf1b338df58e84c
workflow-type: tm+mt
source-wordcount: '489'
ht-degree: 1%

---

# 自定義分配任務通知

分配任務元件用於將任務分配給工作流參與者。 將任務分配給用戶或組時，電子郵件通知將發送到定義的用戶或組成員。
此電子郵件通知通常包含與任務相關的動態資料。 此動態資料是使用系統生成的 [元資料屬性](https://experienceleague.adobe.com/docs/experience-manager-65/forms/publish-process-aem-forms/use-metadata-in-email-notifications.html#using-system-generated-metadata-in-an-email-notification)。
要在電子郵件通知中包含來自已提交表單資料的值，我們需要建立自定義元資料屬性，然後在電子郵件模板中使用這些自定義元資料屬性



## 建立自定義元資料屬性

建議的方法是建立一個OSGI元件，該元件實現 [工作項用戶元資料服務](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/workspace/service/external/WorkitemUserMetadataService.html#getUserMetadataMap--)

以下代碼建立4個元資料屬性(_名字_。_姓氏_。_原因_ 和 _請求金額_)，並根據提交的資料設定其值。 例如，元資料屬性 _名字_ s的值設定為提交資料中名為firstName的元素的值。 以下代碼假定自適應表單的已提交資料為xml格式。 基於JSON架構或表單資料模型的自適應Forms生成JSON格式的資料。


```java
package com.aemforms.workitemuserservice.core;

import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;

import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.xpath.XPath;

import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.*;


import com.adobe.fd.workspace.service.external.WorkitemUserMetadataService;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.metadata.MetaDataMap;
@Component(property={Constants.SERVICE_DESCRIPTION+"=A sample implementation of a user metadata service.",
Constants.SERVICE_VENDOR+"=Adobe Systems",
"process.label"+"=Sample Custom Metadata Service"})


public class WorkItemUserServiceImpl implements WorkitemUserMetadataService {
private static final Logger log = LoggerFactory.getLogger(WorkItemUserServiceImpl.class);

@Override
public Map<String, String> getUserMetadata(WorkItem workItem, WorkflowSession workflowSession,MetaDataMap metadataMap)
{
HashMap<String, String> customMetadataMap = new HashMap<String, String>();
String payloadPath = workItem.getWorkflowData().getPayload().toString();
String dataFilePath = payloadPath + "/Data.xml/jcr:content";
Session session = workflowSession.adaptTo(Session.class);
DocumentBuilderFactory factory = null;
DocumentBuilder builder = null;
Document xmlDocument = null;
javax.jcr.Node xmlDataNode = null;
try
{
    xmlDataNode = session.getNode(dataFilePath);
    InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
    XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
    factory = DocumentBuilderFactory.newInstance();
    builder = factory.newDocumentBuilder();
    xmlDocument = builder.parse(xmlDataStream);
    Node firstNameNode = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/firstName")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    log.debug("The value of first name element  is " + firstNameNode.getTextContent());
    Node lastNameNode = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/lastName")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    Node amountRequested = (org.w3c.dom.Node) xPath
            .compile("afData/afUnboundData/data/amountRequested")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    Node reason = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/reason")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    customMetadataMap.put("firstName", firstNameNode.getTextContent());
    customMetadataMap.put("lastName", lastNameNode.getTextContent());
    customMetadataMap.put("amountRequested", amountRequested.getTextContent());
    customMetadataMap.put("reason", reason.getTextContent());
    log.debug("Created  " + customMetadataMap.size() + " metadata  properties");

}
catch (Exception e)
{
    log.debug(e.getMessage());
}
return customMetadataMap;
}

}
```

## 在任務通知電子郵件模板中使用自定義元資料屬性

在電子郵件模板中，可以使用以下語法包括元資料屬性，其中amountRequested是元資料屬性 `${amountRequested}`

## 配置分配任務以使用自定義元資料屬性

在將OSGi元件構建並部署到伺服器AEM後，將「分配任務」元件配置為使用自定義元資料屬性，如下所示。


![任務通知](assets/task-notification.PNG)

## 啟用自定義元資料屬性的使用

![自定義元資料屬性](assets/custom-meta-data-properties.PNG)

## 在伺服器上嘗試

* [配置日CQ郵件服務](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html#configuring-the-mail-service)
* 將有效電子郵件ID與 [管理員用戶](http://localhost:4502/security/users.html)
* 下載並安裝 [工作流和通知模板](assets/workflow-and-task-notification-template.zip) 使用 [軟體包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* 下載 [自適應窗體](assets/request-travel-authorization.zip) 從AEM中 [窗體和文檔ui](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)。
* 部署並啟動 [自定義捆綁包](assets/work-items-user-service-bundle.jar) 使用 [Web控制台](http://localhost:4502/system/console/bundles)
* [預覽並提交表單](http://localhost:4502/content/dam/formsanddocuments/requestfortravelauhtorization/jcr:content?wcmmode=disabled)

表單提交任務分配通知將發送到與管理員用戶關聯的電子郵件ID。 以下螢幕快照顯示任務分配通知示例

![通知](assets/task-nitification-email.png)

>[!NOTE]
>分配任務通知的電子郵件模板需要採用以下格式。
>
> subject=已分配任務 —  `${workitem_title}`
>
> message=表示您的電子郵件模板且沒有任何新行字元的字串。

## 分配任務電子郵件通知中的任務注釋

在某些情況下，您可能希望在後續任務通知中包括前一個任務所有者的注釋。 下面列出了用於捕獲任務最後注釋的代碼：

```java
package samples.aemforms.taskcomments.core;

import org.osgi.service.component.annotations.Component;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

import javax.jcr.Session;

import org.osgi.framework.Constants;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.HistoryItem;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.metadata.MetaDataMap;

import com.adobe.fd.workspace.service.external.WorkitemUserMetadataService;
@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=A sample implementation of a user metadata service.",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Capture Workflow Comments"
})

public class CaptureTaskComments implements WorkitemUserMetadataService {
  private static final Logger log = LoggerFactory.getLogger(CaptureTaskComments.class);
  @Override
  public Map <String, String> getUserMetadata(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap metadataMap) {
    HashMap < String, String > customMetadataMap = new HashMap < String, String > ();
    workflowSession.adaptTo(Session.class);
    try {
      List <HistoryItem> workItemsHistory = workflowSession.getHistory(workItem.getWorkflow());
      int listSize = workItemsHistory.size();
      HistoryItem lastItem = workItemsHistory.get(listSize - 1);
      String reviewerComments = (String) lastItem.getWorkItem().getMetaDataMap().get("workitemComment");
      log.debug("####The comment I got was ...." + reviewerComments);
      customMetadataMap.put("comments", reviewerComments);
      log.debug("Created  " + customMetadataMap.size() + " metadata  properties");

    } catch (Exception e) {
      log.debug(e.getMessage());
    }
    return customMetadataMap;
  }

}
```

具有上述代碼的捆綁包可以 [從此處下載](assets/samples.aemforms.taskcomments.taskcomments.core-1.0-SNAPSHOT.jar)
