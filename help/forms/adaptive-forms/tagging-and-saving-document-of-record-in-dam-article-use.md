---
title: 在DAM中標籤和儲存AEM FormsDoR
description: 本文將介紹AEM Forms在DAM中生成的DoR的儲存和標籤的使用AEM實例。 文檔的標籤是基於提交的表單資料完成的。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 832f04b4-f22f-4cf9-8136-e3c1081de7a9
source-git-commit: 2ac0f6b3964590e5443700f730a3fc02cb3f63bc
workflow-type: tm+mt
source-wordcount: '611'
ht-degree: 0%

---

# 在DAM中標籤和儲存AEM FormsDoR {#tagging-and-storing-aem-forms-dor-in-dam}

本文將介紹AEM Forms在DAM中生成的DoR的儲存和標籤的使用AEM實例。 文檔的標籤是基於提交的表單資料完成的。

客戶的一個常見要求是在DAM中儲存並標籤AEM Forms生成的記錄文檔(DoR)AEM。 文檔的標籤必須基於自適應Forms提交的資料。 例如，如果提交資料中的雇傭狀態為「已退休」，則我們要用「已退休」標籤文檔，並將文檔儲存在DAM中。

使用案例如下：

* 用戶填寫自適應表單。 在自適應表單中，捕獲用戶的婚姻狀態(ex Single)和就業狀態(ex Retiled)。
* 在提交表單時，AEM將觸發工作流。 此工作流將文檔標籤為婚姻狀態（單一）和雇傭狀態（已退休），並將文檔儲存在DAM中。
* 一旦文檔儲存在DAM中，管理員就應能通過這些標籤搜索文檔。 例如，在「單個」(Single)或「已退休」(Retired)上搜索將獲取相應的DoR。

為了滿足此使用情形，編寫了自定義流程步驟。 在此步驟中，我們將從提交的資料中提取相應資料元素的值。 然後，我們使用此值構造標籤磁貼。 例如，如果婚姻狀態元素的值為「單一」，則標籤標題將變為**Peak:EmploymentStatus/Single。 **使用TagManager API，我們找到標籤並將標籤應用到DoR。

以下是在DAM中標籤和儲存記錄文檔的完整代AEM碼。

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

要使此示例在您的系統上工作，請按照以下步驟操作：
* [部署Developingwithserviceuser捆綁包](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [下載並部署setvalue包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。 這是自定義OSGI捆綁包，它從提交的表單資料中設定標籤。

* [下載示例自適應窗體](assets/tag-and-store-in-dam-adaptive-form.zip)

* [轉到Forms和文檔](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* 按一下建立 |檔案上傳並上載標籤和儲存在資料庫中 — adaptive-form.zip

* [導入文章資產](assets/tag-and-store-in-dam-assets.zip) 使用包AEM管理器
* 開啟 [預覽模式下的示例窗體](http://localhost:4502/content/dam/formsanddocuments/tagandstoreindam/jcr:content?wcmmode=disabled)。 **填寫所有欄位** 並提交表格。
* [導航到DAM中的「峰值」資料夾](http://localhost:4502/assets.html/content/dam/Peak)。 您應在「峰值」資料夾中看到「DoR」。 檢查文檔的屬性。 應適當標籤。
恭喜你！! 您已成功在系統上安裝示例

* 讓我們來探索 [工作流](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html) 在提交表單時觸發。
* 工作流的第一步是通過將申請人姓名和居住縣連接起來來建立唯一的檔案名。
* 工作流的第二步傳遞了需要標籤的標籤層次結構和表單域元素。 該處理步驟從提交的資料中提取值並構建需要標籤文檔的標籤標題。
* 如果要將DoR儲存在DAM中的其他資料夾中，請使用下面螢幕快照中指定的配置屬性指定資料夾位置。

另外兩個參數特定於DoR和資料檔案路徑，如在「自適應表單」提交選項中指定的。 請確保在此處指定的值與在「自適應表單」提交選項中指定的值匹配。

![標籤多爾](assets/tag_dor_service_configuration.gif)
