---
title: 自定義分配任務通知
description: 在指派任務通知電子郵件中加入表單資料
sub-product: 表單
feature: 工作流程
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
kt: 6279
thumbnail: KT-6279.jpg
topic: 開發
role: 開發人員
level: 經驗豐富
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '450'
ht-degree: 2%

---


# 自定義分配任務通知

Assign Task元件用於將任務分配給工作流參與者。 將任務分配給用戶或組時，會向定義的用戶或組成員發送電子郵件通知。
此電子郵件通知通常包含與任務相關的動態資料。 使用系統生成的[元資料屬性](https://docs.adobe.com/content/help/en/experience-manager-65/forms/publish-process-aem-forms/use-metadata-in-email-notifications.html#using-system-generated-metadata-in-an-email-notification)讀取此動態資料。
若要在電子郵件通知中包含已提交表單資料的值，我們需要建立自訂中繼資料屬性，然後在電子郵件範本中使用這些自訂中繼資料屬性



## 建立自訂中繼資料屬性

建議的方法是建立OSGI元件，以實作[WorkitemUserMetadataService](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/workspace/service/external/WorkitemUserMetadataService.html#getUserMetadataMap--)的getUserMetadata方法

下列程式碼會建立4個中繼資料屬性（_firstName_、_lastName_、_reason_&#x200B;和&#x200B;_amountRequested_），並從提交的資料設定其值。 例如，中繼資料屬性&#x200B;_firstName_&#x200B;的值會從提交的資料設定為名為firstName的元素的值。 下列程式碼假設最適化表單的已提交資料為xml格式。 以JSON結構描述或表單資料模型為基礎的最適化Forms會產生JSON格式的資料。


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

在電子郵件範本中，您可使用下列語法加入中繼資料屬性，其中amountRequested是中繼資料屬性`${amountRequested}`

## 配置分配任務以使用自定義元資料屬性

在OSGi元件內建並部署到伺服器AEM後，請配置如下所示的分配任務元件以使用自定義元資料屬性。


![任務通知](assets/task-notification.PNG)

## 啟用自訂中繼資料屬性的使用

![自訂中繼資料屬性](assets/custom-meta-data-properties.PNG)

## 若要在您的伺服器上試用

* [配置日CQ郵件服務](https://docs.adobe.com/content/help/en/experience-manager-65/administering/operations/notification.html#configuring-the-mail-service)
* 將有效的電子郵件ID與[admin user](http://localhost:4502/security/users.html)關聯
* 使用[套件管理器](http://localhost:4502/crx/packmgr/index.jsp)下載並安裝[Workflow-and-notification-template](assets/workflow-and-task-notification-template.zip)
* 下載[最適化表單](assets/request-travel-authorization.zip)並從AEM[表單和檔案ui](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)匯入。
* 使用[Web控制台](http://localhost:4502/system/console/bundles)部署並啟動[自訂Bundle](assets/work-items-user-service-bundle.jar)
* [預覽並送出表單](http://localhost:4502/content/dam/formsanddocuments/requestfortravelauhtorization/jcr:content?wcmmode=disabled)

在表單提交任務分配通知時，會傳送給與管理員用戶關聯的電子郵件ID。 以下螢幕抓圖顯示任務分配通知示例

![通知](assets/task-nitification-email.png)

>[!NOTE]
>指派任務通知的電子郵件範本必須採用下列格式。
>
> subject=Task Assigned - `${workitem_title}`
>
> message=表示您的電子郵件範本而沒有任何新行字元的字串。
