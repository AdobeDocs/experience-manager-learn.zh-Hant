---
title: 自訂指派工作通知
description: 在指派任務通知電子郵件中包含表單資料
feature: Workflow
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6279
thumbnail: KT-6279.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 0cb74afd-87ff-4e79-a4f4-a4634ac48c51
last-substantial-update: 2020-07-07T00:00:00Z
duration: 144
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 0%

---

# 自訂指派工作通知

指派任務元件用於將任務指派給工作流程參與者。 當任務指派給使用者或群組時，會傳送電子郵件通知給已定義的使用者或群組成員。
此電子郵件通知通常包含與任務相關的動態資料。 此動態資料是使用系統產生的[中繼資料屬性](https://experienceleague.adobe.com/docs/experience-manager-65/forms/publish-process-aem-forms/use-metadata-in-email-notifications.html?lang=zh-Hant#using-system-generated-metadata-in-an-email-notification)擷取。
若要在電子郵件通知中包含來自已提交表單資料的值，我們需要建立自訂中繼資料屬性，然後在電子郵件範本中使用這些自訂中繼資料屬性



## 建立自訂中繼資料屬性

建議的方法是建立實作[WorkitemUserMetadataService](https://helpx.adobe.com/tw/experience-manager/6-5/forms/javadocs/com/adobe/fd/workspace/service/external/WorkitemUserMetadataService.html#getUserMetadataMap--)的getUserMetadata方法的OSGI元件

下列程式碼會建立4個中繼資料屬性（_firstName_、_lastName_、_reason_&#x200B;和&#x200B;_amountRequested_），並從提交的資料設定其值。 例如，中繼資料屬性&#x200B;_firstName_&#x200B;的值設定為從提交的資料中稱為firstName的元素的值。 下列程式碼假設最適化表單提交的資料為xml格式。 以JSON結構描述或表單資料模型為基礎的最適化Forms會產生JSON格式的資料。


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

## 在任務通知電子郵件範本中使用自訂中繼資料屬性

在電子郵件範本中，您可以使用下列語法包含中繼資料屬性，其中amountRequested是中繼資料屬性`${amountRequested}`

## 設定指派任務以使用自訂中繼資料屬性

建立OSGi元件並將其部署到AEM伺服器後，請如下所示設定「指派工作」元件以使用自訂中繼資料屬性。


![工作通知](assets/task-notification.PNG)

## 允許使用自訂中繼資料屬性

![自訂中繼資料屬性](assets/custom-meta-data-properties.PNG)

## 若要在您的伺服器上嘗試此動作

* [設定Day CQ郵件服務](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=zh-Hant#configuring-the-mail-service)
* 將有效的電子郵件識別碼與[管理員使用者](http://localhost:4502/security/users.html)建立關聯
* 使用[封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)下載並安裝[工作流程與通知範本](assets/workflow-and-task-notification-template.zip)
* 下載[最適化表單](assets/request-travel-authorization.zip)，並從[表單與檔案ui](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)匯入至AEM。
* 使用[網頁主控台](http://localhost:4502/system/console/bundles)部署並啟動[自訂組合](assets/work-items-user-service-bundle.jar)
* [預覽並提交表單](http://localhost:4502/content/dam/formsanddocuments/requestfortravelauhtorization/jcr:content?wcmmode=disabled)

在表單提交時，任務指派通知會傳送到與管理員使用者相關聯的電子郵件ID。 下列熒幕擷圖顯示範例任務指派通知

![通知](assets/task-nitification-email.png)

>[!NOTE]
>指派任務通知的電子郵件範本必須採用以下格式。
>
> subject=任務已指派 — `${workitem_title}`
>
> message=字串，代表您的電子郵件範本，不含任何新行字元。

## 指派任務電子郵件通知中的任務註解

在某些情況下，您可能會想要在後續任務通知中包含先前任務擁有者的註解。 擷取任務最後註解的程式碼如下：

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

具有上述程式碼的套件組合可以從這裡[&#128279;](assets/samples.aemforms.taskcomments.taskcomments.core-1.0-SNAPSHOT.jar)下載
