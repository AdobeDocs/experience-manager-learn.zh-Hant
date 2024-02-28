---
title: 使用以核心元件為基礎的最適化表單中的資料產生PDF
description: 將核心元件型表單提交的資料與工作流程中的XDP範本合併
version: 6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
jira: KT-15025
last-substantial-update: 2024-02-26T00:00:00Z
source-git-commit: acfd982ce471c8510cb59ed4d353f1d47dcfb5dc
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 0%

---

# 根據核心元件提交的表單資料，產生PDF

以下是將「核心元件」改為大寫的修訂文字：

典型案例涉及透過核心元件型調適型表單提交的資料產生PDF。 此資料一律為JSON格式。 若要使用轉譯PDFAPI產生PDF，必須將JSON資料轉換為XML格式。 此 `toString` 方法 `org.json.XML` 用於此轉換。 如需詳細資訊，請參閱 [檔案： `org.json.XML.toString` 方法](https://www.javadoc.io/doc/org.json/json/20171018/org/json/XML.html#toString-java.lang.Object-).

## 以JSON結構描述為基礎的最適化表單

請依照下列步驟，為您的最適化表單建立JSON結構描述：

### 產生XDP的範例資料

若要簡化程式，請遵循這些精細化步驟：

1. 在AEM Forms Designer中開啟XDP檔案。
1. 導覽至「檔案」>「表單屬性」>「預覽」。
1. 選取「產生預覽資料」。
1. 按一下「產生」。
1. 指派有意義的檔案名稱，例如 `form-data.xml`.

### 從XML資料產生JSON結構描述

您可以利用任何免費的線上工具來 [將XML轉換為JSON](https://jsonformatter.org/xml-to-jsonschema) 使用上一步驟中產生的XML資料。

### 將JSON轉換為XML的自訂工作流程程式

提供的程式碼會將JSON轉換為XML，並將產生的XML儲存在名為的工作流程處理變數中 `dataXml`.

```java
import org.slf4j.LoggerFactory;
import com.adobe.granite.workflow.WorkflowException;
import java.io.InputStream;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import javax.jcr.Node;
import javax.jcr.Session;
import org.json.JSONObject;
import org.json.XML;
import org.slf4j.Logger;
import org.osgi.service.component.annotations.Component;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;

@Component(property = {
    "service.description=Convert JSON to XML",
    "process.label=Convert JSON to XML"
})
public class ConvertJSONToXML implements WorkflowProcess {

    private static final Logger log = LoggerFactory.getLogger(ConvertJSONToXML.class);

    @Override
    public void execute(final WorkItem workItem, final WorkflowSession workflowSession, final MetaDataMap arg2) throws WorkflowException {
        String processArgs = arg2.get("PROCESS_ARGS", "string");
        log.debug("The process argument I got was " + processArgs);
        
        String submittedDataFile = processArgs;
        String payloadPath = workItem.getWorkflowData().getPayload().toString();
        log.debug("The payload in convert json to xml " + payloadPath);
        
        String dataFilePath = payloadPath + "/" + submittedDataFile + "/jcr:content";
        try {
            Session session = workflowSession.adaptTo(Session.class);
            Node submittedJsonDataNode = session.getNode(dataFilePath);
            InputStream jsonDataStream = submittedJsonDataNode.getProperty("jcr:data").getBinary().getStream();
            BufferedReader streamReader = new BufferedReader(new InputStreamReader(jsonDataStream, "UTF-8"));
            StringBuilder stringBuilder = new StringBuilder();
            String inputStr;
            while ((inputStr = streamReader.readLine()) != null) {
                stringBuilder.append(inputStr);
            }
            JSONObject submittedJson = new JSONObject(stringBuilder.toString());
            log.debug(submittedJson.toString());
            
            String xmlString = XML.toString(submittedJson);
            log.debug("The json converted to XML " + xmlString);
            
            MetaDataMap metaDataMap = workItem.getWorkflow().getWorkflowData().getMetaDataMap();
            metaDataMap.put("xmlData", xmlString);
        } catch (Exception e) {
            log.error("Error converting JSON to XML: " + e.getMessage(), e);
        }
    }
}
```

### 建立工作流程

若要處理表單提交，請建立包含兩個步驟的工作流程：

1. 初始步驟會採用自訂程式，將提交的JSON資料轉換為XML。
1. 後續步驟會透過結合XML資料與XDP範本來產生PDF。

![json-to-xml](assets/json-to-xml-process-step.png)


## 部署範常式式碼

若要在本機伺服器上測試此功能，請遵循下列簡化步驟：

1. [透過AEM OSGi Web主控台下載及安裝自訂套件組合](assets/convertJsonToXML.core-1.0.0-SNAPSHOT.jar).
1. [匯入工作流程封裝](assets/workflow_to_render_pdf.zip).
1. [匯入範例最適化表單和XDP範本](assets/adaptive_form_and_xdp_template.zip).
1. [預覽最適化表單](http://localhost:4502/content/dam/formsanddocuments/f23/jcr:content?wcmmode=disabled).
1. 填寫一些表單欄位。
1. 提交表單以起始AEM工作流程。
1. 在工作流程的裝載資料夾中找到轉譯的PDF。

