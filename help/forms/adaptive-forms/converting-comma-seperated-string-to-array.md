---
title: 在AEM Forms工作流中將逗號分隔的字串轉換為字串陣列
description: 當表單資料模型具有字串陣列作為輸入參數之一時，您需要在調用表單資料模型的提交操作之前，按照自適應表單的提交操作生成的資料。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
kt: 8507
exl-id: 9ad69407-2413-416f-9cec-43f88989b31d
last-substantial-update: 2021-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 0%

---

# 將逗號分隔的字串轉換為字串陣列 {#setting-value-of-json-data-element-in-aem-forms-workflow}

當表單基於表單資料模型，該資料模型將字串陣列作為輸入參數，則需要處理提交的自適應表單資料以插入字串陣列。 例如，如果已將複選框欄位綁定到類型字串陣列的表單資料模型元素，則複選框欄位中的資料採用逗號分隔的字串格式。 下面列出的示例代碼說明如何用字串陣列替換逗號分隔的字串。

## 建立流程步驟

當希望工作流執行某AEM個邏輯時，在工作流中使用進程步驟。 該處理步驟可以與ECMA指令碼或OSGi服務相關聯。 我們的自定義進程步驟執行OSGi服務。

已提交的資料採用以下格式。 businessUnits元素的值是逗號分隔的字串，需要將其轉換為字串陣列。

![提交資料](assets/submitted-data-string.png)

與表單資料模型關聯的其餘端點的輸入資料需要字串陣列，如此螢幕抓圖所示。 流程步驟中的自定義代碼將提交的資料轉換為正確的格式。

![fdm字串陣列](assets/string-array-fdm.png)

我們將JSON對象路徑和元素名稱傳遞到進程步驟。 處理步驟中的代碼將元素的逗號分隔值替換為字串陣列。
![過程步驟](assets/create-string-array.png)

>[!NOTE]
>
>確保自適應表單的提交選項中的資料檔案路徑設定為「Data.xml」。 這是因為進程步驟中的代碼在負載資料夾下查找名為Data.xml的檔案。

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

示例包可以是 [從此處下載](assets/CreateStringArray.CreateStringArray.core-1.0-SNAPSHOT.jar)
