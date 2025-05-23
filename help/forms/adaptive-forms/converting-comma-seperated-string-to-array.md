---
title: 在AEM Forms工作流程中將逗號分隔字串轉換為字串陣列
description: 當您的表單資料模型有一個字串陣列作為輸入引數之一時，您將需要在叫用表單資料模型的提交動作之前，對從調適型表單的提交動作產生的資料進行推測。
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-8507
exl-id: 9ad69407-2413-416f-9cec-43f88989b31d
last-substantial-update: 2021-06-09T00:00:00Z
duration: 115
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 0%

---

# 將逗號分隔的字串轉換為字串陣列 {#setting-value-of-json-data-element-in-aem-forms-workflow}

當您的表單是以具有字串陣列作為輸入引數的表單資料模型為基礎時，您需要操作提交的調適型表單資料以插入字串陣列。 例如，如果您將核取方塊欄位繫結至字串陣列型別的表單資料模型元素，則核取方塊欄位的資料會採用逗號分隔的字串格式。 下列程式碼範例說明如何將逗號分隔的字串取代為字串陣列。

## 建立流程步驟

當我們想要工作流程執行特定邏輯時，AEM工作流程會使用流程步驟。 流程步驟可以與ECMA指令碼或OSGi服務相關聯。 我們的自訂流程步驟會執行OSGi服務。

提交的資料採用以下格式。 businessUnits元素的值是以逗號分隔的字串，需要轉換為字串陣列。

![提交的資料](assets/submitted-data-string.png)

與表單資料模型相關聯之rest端點的輸入資料預期的是此熒幕擷取畫面中顯示的字串陣列。 程式步驟中的自訂程式碼會將中提交的資料轉換為正確格式。

![fdm-string-array](assets/string-array-fdm.png)

我們會將JSON物件路徑和元素名稱傳遞至流程步驟。 處理步驟中的程式碼會將元素的逗號分隔值取代為字串陣列。
![process-step](assets/create-string-array.png)

>[!NOTE]
>
>請確定最適化表單提交選項中的資料檔案路徑已設為「Data.xml」。 這是因為程式步驟中的程式碼會在裝載資料夾下尋找名為Data.xml的檔案。

## 處理步驟程式碼

```java
import java.io.BufferedReader;
import java.io.ByteArrayInputStream;
import java.io.InputStream;
import java.io.InputStreamReader;

import javax.jcr.Binary;
import javax.jcr.Node;
import javax.jcr.Session;

import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.google.gson.JsonArray;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

@Component(property = {
    Constants.SERVICE_DESCRIPTION + "=Create String Array",
    Constants.SERVICE_VENDOR + "=Adobe Systems",
    "process.label" + "=Replace comma seperated string with string array"
})

public class CreateStringArray implements WorkflowProcess {
    private static final Logger log = LoggerFactory.getLogger(CreateStringArray.class);
    @Override
    public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap arg2) throws WorkflowException {
        log.debug("The string I got was ..." + arg2.get("PROCESS_ARGS", "string").toString());
        String[] arguments = arg2.get("PROCESS_ARGS", "string").toString().split(",");
        String objectName = arguments[0];
        String propertyName = arguments[1];

        String objects[] = objectName.split("\\.");
        System.out.println("The params is " + propertyName);
        log.debug("The params string is " + objectName);
        String payloadPath = workItem.getWorkflowData().getPayload().toString();
        log.debug("The payload  in set Elmement Value in Json is  " + workItem.getWorkflowData().getPayload().toString());
        String dataFilePath = payloadPath + "/Data.xml/jcr:content";
        Session session = workflowSession.adaptTo(Session.class);
        Node submittedDataNode = null;
        try {
            submittedDataNode = session.getNode(dataFilePath);

            InputStream submittedDataStream = submittedDataNode.getProperty("jcr:data").getBinary().getStream();
            BufferedReader streamReader = new BufferedReader(new InputStreamReader(submittedDataStream, "UTF-8"));
            StringBuilder stringBuilder = new StringBuilder();

            String inputStr;
            while ((inputStr = streamReader.readLine()) != null)
                stringBuilder.append(inputStr);
            JsonParser jsonParser = new JsonParser();
            JsonObject jsonObject = jsonParser.parse(stringBuilder.toString()).getAsJsonObject();
            System.out.println("The json object that I got was " + jsonObject);
            JsonObject targetObject = null;

            for (int i = 0; i < objects.length - 1; i++) {
                System.out.println("The object name is " + objects[i]);
                if (i == 0) {
                    targetObject = jsonObject.get(objects[i]).getAsJsonObject();
                } else {
                    targetObject = targetObject.get(objects[i]).getAsJsonObject();

                }

            }

            System.out.println("The final object is " + targetObject.toString());
            String businessUnits = targetObject.get(propertyName).getAsString();
            System.out.println("The values of " + propertyName + " are " + businessUnits);

            JsonArray jsonArray = new JsonArray();

            String[] businessUnitsArray = businessUnits.split(",");
            for (String name: businessUnitsArray) {
                jsonArray.add(name);
            }

            targetObject.add(propertyName, jsonArray);
            System.out.println(" After updating the property " + targetObject.toString());
            InputStream is = new ByteArrayInputStream(jsonObject.toString().getBytes());
            System.out.println("The changed json data  is " + jsonObject.toString());
            Binary binary = session.getValueFactory().createBinary(is);
            submittedDataNode.setProperty("jcr:data", binary);
            session.save();

        } catch (Exception e) {
            System.out.println(e.getMessage());
        }

    }
}
```

範例組合可從這裡[&#128279;](assets/CreateStringArray.CreateStringArray.core-1.0-SNAPSHOT.jar)下載
