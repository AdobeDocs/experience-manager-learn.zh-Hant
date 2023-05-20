---
title: 更新資料庫中表單的簽名狀態
description: 使用工作流更新資料庫中籤名表單的簽名狀AEM態
feature: Adaptive Forms
version: 6.4,6.5
kt: 6888
thumbnail: 6888.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 75852a4b-7008-4c65-bab1-cc5dbf525e20
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '116'
ht-degree: 2%

---

# 更新簽名狀態

當用戶完成簽名儀式時，將觸發UpdateSignatureStatus工作流。 以下是工作流的流

![主工作流](assets/update-signature.PNG)

「更新簽名狀態」是自定義進程步驟。
實施自定義流程步驟的主要原因是擴展工AEM作流。 以下是用於更新簽名狀態的自定義代碼。
此自定義進程步驟中的代碼引用SignMultipleForms服務。


```java
@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=Update Signature Status in DB",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Update Signature Status in DB"
})

public class UpdateSignatureStatusWorkflowStep implements WorkflowProcess {
  private static final Logger log = LoggerFactory.getLogger(UpdateSignatureStatusWorkflowStep.class);@Reference
  SignMultipleForms signMultipleForms;@Override
  public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap args) throws WorkflowException {
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    String dataFilePath = payloadPath + "/Data.xml/jcr:content";
    Session session = workflowSession.adaptTo(Session.class);
    DocumentBuilderFactory factory = null;
    DocumentBuilder builder = null;
    Document xmlDocument = null;
    Node xmlDataNode = null;
    try {
      xmlDataNode = session.getNode(dataFilePath);
      InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
      factory = DocumentBuilderFactory.newInstance();
      builder = factory.newDocumentBuilder();
      xmlDocument = builder.parse(xmlDataStream);
      XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
      org.w3c.dom.Node node = (org.w3c.dom.Node) xPath.compile("/afData/afUnboundData/data/guid").evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
      String guid = node.getTextContent();
      StringWriter writer = new StringWriter();
      IOUtils.copy(xmlDataStream, writer, StandardCharsets.UTF_8);
      System.out.println("After ioutils copy" + writer.toString());
      signMultipleForms.updateSignatureStatus(writer.toString(), guid);
    }
    catch(Exception e) {
      log.debug(e.getMessage());
    }

  }

}
```

## Assets

更新簽名狀態工作流可以 [從此處下載](assets/update-signature-status-workflow.zip)

## 後續步驟

[自定義摘要步驟以顯示下一個用於簽名的表單](./customize-summary-component.md)
