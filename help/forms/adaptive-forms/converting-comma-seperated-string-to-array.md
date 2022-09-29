---
title: 在AEM Forms工作流程中將逗號分隔字串轉換為字串陣列
description: 當您的表單資料模型有字串陣列作為輸入參數之一時，您需要在叫用表單資料模型的提交動作之前，按摩從適用性表單的提交動作產生的資料。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
kt: 8507
exl-id: 9ad69407-2413-416f-9cec-43f88989b31d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 0%

---

# 將逗號分隔字串轉換為字串陣列 {#setting-value-of-json-data-element-in-aem-forms-workflow}

當您的表單以表單資料模型為基礎，而表單資料模型以字串陣列作為輸入參數，您需要控制提交的最適化表單資料以插入字串陣列。 例如，如果您已將核取方塊欄位綁定到類型字串陣列的表單資料模型元素，則來自核取方塊欄位的資料會以逗號分隔字串格式。 下列范常式式碼顯示如何以字串陣列取代以逗號分隔的字串。

## 建立處理步驟

當我們希望工作流程執行特定邏輯時，AEM工作流程會使用處理步驟。 處理步驟可與ECMA指令碼或OSGi服務相關聯。 我們的自訂程式步驟會執行OSGi服務。

提交的資料格式如下。 businessUnits元素的值是以逗號分隔的字串，需要轉換為字串的陣列。

![submitted-data](assets/submitted-data-string.png)

與表單資料模型相關聯的rest端點的輸入資料預期會有字串陣列，如此螢幕擷取畫面所示。 處理步驟中的自訂程式碼會將提交的資料轉換為正確的格式。

![fdm-string-array](assets/string-array-fdm.png)

我們會將JSON物件路徑和元素名稱傳遞至處理步驟。 處理步驟中的程式碼會將元素的逗號分隔值取代為字串陣列。
![過程步驟](assets/create-string-array.png)

>[!NOTE]
>
>確認適用性表單提交選項中的資料檔案路徑已設為「Data.xml」。 這是因為處理步驟中的程式碼會在裝載資料夾下尋找名為Data.xml的檔案。

## 處理步驟代碼

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

範例套件可以是 [從此處下載](assets/CreateStringArray.CreateStringArray.core-1.0-SNAPSHOT.jar)
