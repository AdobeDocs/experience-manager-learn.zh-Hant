---
title: 用於填充清單變數的自定義進程步驟
description: 用於填充文檔和字串類型的清單變數的自定義進程步驟
feature: Workflow
topic: Development
version: 6.5
role: Developer
level: Beginner
kt: kt-8063
exl-id: 09d9eabf-4815-4159-b6c7-cf2ebc8a2df5
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 1%

---

# 自定義流程步驟


實現了自定義流程步驟，用附件和附件名稱填充「陣列清單」類型的工作流變數。 此變數隨後將用於「發送電子郵件」工作流元件。 如果您不熟悉建立OSGi捆綁包，請 [按照這些說明](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html?lang=en)

自定義流程步驟中的代碼執行以下操作

* 查詢有效負載資料夾下的所有自適應表單附件。 資料夾名稱作為進程參數傳遞給進程步驟。

* 填充 `listOfDocuments` 工作流變數
* 填充 `attachmentNames` 工作流變數
* 設定工作流變數的值(`no_of_attachments`)

```java
 package com.aemforms.formattachments.core;

import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;
import javax.jcr.Node;
import javax.jcr.Session;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.day.cq.search.PredicateGroup;
import com.day.cq.search.Query;
import com.day.cq.search.QueryBuilder;
import com.day.cq.search.result.Hit;
import com.day.cq.search.result.SearchResult;

@Component(property = {
        Constants.SERVICE_DESCRIPTION + "=PopulateListOfDocuments",
        "process.label" + "=PopulateListOfDocuments"
})

public class PopulateListOfDocuments implements WorkflowProcess {

        private static final Logger log = LoggerFactory.getLogger(PopulateListOfDocuments.class);
        @Reference
        QueryBuilder queryBuilder;

        @Override
        public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments) throws WorkflowException
        {
                String payloadPath = workItem.getWorkflowData().getPayload().toString();
                log.debug("The payload path  is" + payloadPath);
                MetaDataMap metaDataMap = workItem.getWorkflow().getWorkflowData().getMetaDataMap();
                Session session = workflowSession.adaptTo(Session.class);
                Map < String, String > map = new HashMap < String, String > ();
                map.put("path", workItem.getWorkflowData().getPayload().toString() + "/" + processArguments.get("PROCESS_ARGS", "string").toString());
                map.put("type", "nt:file");
                Query query = queryBuilder.createQuery(PredicateGroup.create(map), workflowSession.adaptTo(Session.class));
                query.setStart(0);
                query.setHitsPerPage(20);
                SearchResult result = query.getResult();
                log.debug("Get result hits " + result.getHits().size());
                int no_of_attachments = result.getHits().size();
                Document[] listOfDocuments = new Document[no_of_attachments];
                String[] attachmentNames = new String[no_of_attachments];
                int i = 0;
                for (Hit hit: result.getHits()) {
                        try {
                                String attachmentPath = hit.getPath();
                                log.debug("The hit path is" + hit.getPath());
                                Node attachmentNode = session.getNode(attachmentPath + "/jcr:content");
                                InputStream attachmentStream = attachmentNode.getProperty("jcr:data").getBinary().getStream();
                                listOfDocuments[i] = new Document(attachmentStream);
                                attachmentNames[i] = new String(hit.getTitle());
                                log.debug("Added " + hit.getTitle() + "to the list");
                                i++;
                        } catch (Exception e) {
                                log.error("Unable to obtain attachment", e);
                        }
                }

                metaDataMap.put("no_of_attachments", no_of_attachments);
                metaDataMap.put("listOfDocuments", listOfDocuments);
                metaDataMap.put("attachmentNames", attachmentNames);

                log.debug("Updated workflow");
        }

}
```

>[!NOTE]
>
> 請確保在工作流中定義了以下變數，以便代碼工作
> *文檔清單*  — 文檔ArrayList類型的變數
> *附件名稱*  — 字串ArrayList類型的變數
> *no_of_attachments*  — 雙變數類型

## 後續步驟

[Test本地系統上的解決方案](./test.md)
