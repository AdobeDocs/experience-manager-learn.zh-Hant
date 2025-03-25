---
title: 在DAM中標籤和儲存AEM Forms DoR
description: 本文將逐步說明在AEM DAM中儲存和標籤AEM Forms產生的DoR的使用案例。 檔案的標籤是根據提交的表單資料完成的。
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 832f04b4-f22f-4cf9-8136-e3c1081de7a9
last-substantial-update: 2019-03-20T00:00:00Z
duration: 191
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---

# 在DAM中標籤和儲存AEM Forms DoR {#tagging-and-storing-aem-forms-dor-in-dam}

本文將逐步說明在AEM DAM中儲存和標籤AEM Forms產生的DoR的使用案例。 檔案的標籤是根據提交的表單資料完成的。

客戶的常見要求是儲存並標籤AEM Forms在AEM DAM中產生的記錄檔案(DoR)。 檔案的標籤必須根據最適化Forms提交的資料。 例如，如果提交資料中的僱用狀態為「已淘汰」，我們想使用「已淘汰」標籤檔案並將檔案儲存在DAM中。

使用案例如下：

* 使用者填寫最適化表單。 在最適化表單中，會擷取使用者的婚姻狀況（例如，單身）和僱用狀況（例如，已退休）。
* 在提交表單時會觸發AEM工作流程。 此工作流程會以婚姻狀態（單身）和僱用狀態（已淘汰）標籤檔案，並將檔案儲存在DAM中。
* 一旦檔案儲存在DAM中，管理員應該能夠按這些標籤搜尋檔案。 例如，搜尋「單一」或「已淘汰」會擷取適當的DoR。

為了滿足此使用案例，已編寫自訂流程步驟。 在此步驟中，我們會從提交的資料中擷取適當資料元素的值。 然後我們使用此值來建構標籤拼貼。 例如，如果婚姻狀況元素的值為「Single」，則標籤標題會變成**Peak：EmploymentStatus/Single。 **我們使用TagManager API來尋找標籤，並將標籤套用至DoR。

以下是在AEM DAM中標籤和儲存記錄檔案的完整程式碼。

```java
package com.aemforms.setvalue.core;
import java.io.InputStream;
import javax.jcr.Node;
import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.xpath.XPath;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;
import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowData;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.adobe.granite.workflow.model.WorkflowModel;
import com.day.cq.tagging.Tag;
import com.day.cq.tagging.TagManager;

@Component(property = {
   Constants.SERVICE_DESCRIPTION + "=Tag and Store Dor in DAM",
   Constants.SERVICE_VENDOR + "=Adobe Systems",
   "process.label" + "=Tag and Store Dor in DAM"
})
@Designate(ocd = TagDorServiceConfiguration.class)
public class TagAndStoreDoRinDAM implements WorkflowProcess
{
   private static final Logger log = LoggerFactory.getLogger(TagAndStoreDoRinDAM.class);

   private TagDorServiceConfiguration serviceConfig;
   @Activate
   public void activate(TagDorServiceConfiguration config)
   {
      this.serviceConfig = config;
   }
   @Override
   public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap arg2) throws WorkflowException
   {
       log.debug("The process arguments passed ..." + arg2.get("PROCESS_ARGS", "string").toString());
      String params = arg2.get("PROCESS_ARGS", "string").toString();
      WorkflowModel wfModel = workflowSession.getModel("/var/workflow/models/dam/update_asset");
      // Read the Tag DoR service configuration
      String damFolder = serviceConfig.damFolder();
      String dorPDFName = serviceConfig.dorPath();
      String dataXmlFile = serviceConfig.dataFilePath();
      log.debug("The Data Xml File is ..." + dataXmlFile + "DorPDFName" + dorPDFName);
      // Read the arguments passed to this workflow step
      String parameters[] = params.split(",");
      log.debug("The %%%% length of parameters is " + parameters.length);
      Tag[] tagArray = new Tag[parameters.length];
      WorkflowData wfData = workItem.getWorkflowData();
      String dorFileName = (String) wfData.getMetaDataMap().get("filename");
      log.debug("The dorFileName is ..." + dorFileName);
      String payloadPath = workItem.getWorkflowData().getPayload().toString();
      String dataFilePath = payloadPath + "/" + dataXmlFile + "/jcr:content";
      String dorDocumentPath = payloadPath + "/" + dorPDFName + "/jcr:content";
      log.debug("Data File Path" + dataFilePath);
      log.debug("Dor File Path" + dorDocumentPath);
      Session session = workflowSession.adaptTo(Session.class);
      ResourceResolver resourceResolver = workflowSession.adaptTo(ResourceResolver.class);
      com.day.cq.dam.api.AssetManager assetMgr = resourceResolver.adaptTo(com.day.cq.dam.api.AssetManager.class);
      DocumentBuilderFactory factory = null;
      DocumentBuilder builder = null;
      Document xmlDocument = null;
      Node xmlDataNode = null;
      Node dorDocumentNode = null;

      try
      {
         // create org.w3c.dom.Document object from submitted form data
         xmlDataNode = session.getNode(dataFilePath);
         log.debug("xml Data Node" + xmlDataNode.getName());
         dorDocumentNode = session.getNode(dorDocumentPath);
         log.debug("DOR Document Node is " + dorDocumentNode.getName());
         InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
         InputStream dorInputStream = dorDocumentNode.getProperty("jcr:data").getBinary().getStream();
         XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
         factory = DocumentBuilderFactory.newInstance();
         builder = factory.newDocumentBuilder();
         xmlDocument = builder.parse(xmlDataStream);
         String newFile = "/content/dam/" + damFolder + "/" + dorFileName;
         log.debug("the new file is ..." + newFile);
         // Store the DoR in DAM
         assetMgr.createAsset(newFile, dorInputStream, "application/pdf", true);
         WorkflowData wfDataLoad = workflowSession.newWorkflowData("JCR_PATH", newFile);
         log.debug("Wrote the document to DAM" + newFile);
         TagManager tagManager = resourceResolver.adaptTo(TagManager.class);
         Resource pdfDocumentNode = resourceResolver.getResource(newFile);
         Resource metadata = pdfDocumentNode.getChild("jcr:content/metadata");
         // Fetch the xml elements from the xml document
         for (int i = 0; i < parameters.length; i++)
            {
                String tagTitle = parameters[i].split("=")[0];
                log.debug("The tag title is" + tagTitle);
                String nameOfNode = parameters[i].split("=")[1];
                org.w3c.dom.Node xmlElement = (org.w3c.dom.Node) xPath.compile(nameOfNode).evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
                log.debug("###The value data node is " + xmlElement.getTextContent());
                Tag tagFound = tagManager.resolveByTitle(tagTitle + xmlElement.getTextContent());
                log.debug("The tag found was ..." + tagFound.getPath());
                tagArray[i] = tagFound;
            }
         tagManager.setTags(metadata, tagArray, true);
         workflowSession.startWorkflow(wfModel, wfDataLoad);
         log.debug("Workflow started");
         log.debug("Done setting tags");
         xmlDataStream.close();
         dorInputStream.close();
      } catch (Exception e)
            {
                 log.debug("The error message is " + e.getMessage());
            }

   }

}
```

若要讓此範例在您的系統上運作，請遵循下列步驟：
* [部署Developingwithserviceuser套件](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [下載並部署setvalue組合](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。 此自訂OSGI套件組合會從提交的表單資料中設定標籤。

* [下載最適化表單範例](assets/tag-and-store-in-dam-adaptive-form.zip)

* [前往Forms和檔案](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* 按一下建立 | 檔案上傳和上傳標籤 — and-store-in-dam-adaptive-form.zip

* [使用AEM封裝管理員匯入文章資產](assets/tag-and-store-in-dam-assets.zip)
* 在預覽模式](http://localhost:4502/content/dam/formsanddocuments/tagandstoreindam/jcr:content?wcmmode=disabled)中開啟[範例表單。 **填寫所有欄位**&#x200B;並提交表單。
* [導覽至DAM中的尖峰資料夾](http://localhost:4502/assets.html/content/dam/Peak)。 您應該會在Peak資料夾中看見DoR。 檢查檔案的屬性。 應該適當地加以標籤。
恭喜!! 您已成功在系統上安裝範例

* 讓我們探索在表單提交時觸發的[工作流程](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html)。
* 工作流程的第一步會串連申請人名稱與居住縣，以建立唯一的檔案名稱。
* 工作流程的第二個步驟會傳遞標籤階層以及需要標籤的表單欄位元素。 處理步驟會從提交的資料中擷取值，並建構標籤檔案所需的標籤標題。
* 如果您想要將DoR儲存在DAM中的其他資料夾，您可以使用底下熒幕擷取畫面中指定的設定屬性來指定資料夾位置。

其他兩個引數專用於最適化表單提交選項中指定的DoR和資料檔案路徑。 請確定您在此指定的值與您在最適化表單提交選項中指定的值相符。

![標籤Dor](assets/tag_dor_service_configuration.gif)
