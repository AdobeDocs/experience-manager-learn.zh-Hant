---
title: 實作自訂流程步驟
description: 使用自訂流程步驟將最適化表單附件寫入檔案系統
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 879518db-3f05-4447-86e8-5802537584e5
last-substantial-update: 2021-06-09T00:00:00Z
duration: 226
source-git-commit: 7ebc33932153cf024e68fc5932b7972d9da262a7
workflow-type: tm+mt
source-wordcount: '758'
ht-degree: 0%

---

# 自訂流程步驟

本教學課程適用於需要實作自訂流程步驟的AEM Forms客戶。 處理步驟可以執行ECMA指令碼或呼叫自訂Java™程式碼以執行作業。 本教學課程將說明實作由流程步驟執行的WorkflowProcess所需的步驟。

實作自訂流程步驟的主要原因是為了擴充AEM Workflow。 例如，如果您在工作流程模型中使用AEM Forms元件，您可能想要執行下列操作

* 將最適化表單附件儲存至檔案系統
* 處理提交的資料

為了完成上述使用案例，您通常會撰寫由流程步驟執行的OSGi服務。

## 建立Maven專案

第一步是使用適當的AdobeMaven原型建立Maven專案。 詳細步驟列於此 [文章](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html). 將Maven專案匯入到Eclipse後，您就可以開始編寫可在流程步驟中使用的第一個OSGi元件了。


### 建立實作WorkflowProcess的類別

在Eclipse IDE中開啟Maven專案。 展開 **projectname** > **核心** 資料夾。 展開 `src/main/java` 資料夾。 您應該會看到結尾為 `core`. 建立在此封裝中實作WorkflowProcess的Java™類別。 您需要覆寫execute方法。 執行方法的簽章如下：

```java
public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments) throws WorkflowException 
```

execute方法可存取下列3個變數：

**工作專案**：workItem變數可授予與工作流程相關資料的存取權。 公開API檔案已可供參考 [此處。](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**Workflowsession**：此workflowSession變數可讓您控制工作流程。 公開API檔案已可供參考 [此處](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html).

**中繼資料地圖**：與工作流程相關聯的所有中繼資料。 任何傳遞至流程步驟的流程引數都可使用MetaDataMap物件。[API檔案](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html)

在本教學課程中，我們會將新增至最適化表單的附件寫入檔案系統，做為AEM Workflow的一部分。

為了完成此使用案例，編寫了下列Java™類別

讓我們看看此程式碼

```java
package com.learningaemforms.adobe.core;

import java.io.File;
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
    Constants.SERVICE_DESCRIPTION + "=Write Adaptive Form Attachments to File System",
    Constants.SERVICE_VENDOR + "=Adobe Systems",
    "process.label" + "=Save Adaptive Form Attachments to File System"
})
public class WriteFormAttachmentsToFileSystem implements WorkflowProcess {

    private static final Logger log = LoggerFactory.getLogger(WriteFormAttachmentsToFileSystem.class);
    @Reference
    QueryBuilder queryBuilder;

    @Override
    public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)
    throws WorkflowException {
        // TODO Auto-generated method stub
        log.debug("The string I got was ..." + processArguments.get("PROCESS_ARGS", "string").toString());
        String[] params = processArguments.get("PROCESS_ARGS", "string").toString().split(",");
        String attachmentsPath = params[0];
        String saveToLocation = params[1];
        log.debug("The seperator is" + File.separator);
        String payloadPath = workItem.getWorkflowData().getPayload().toString();
        Map<String, String> map = new HashMap<String, String> ();
        map.put("path", payloadPath + "/" + attachmentsPath);
        File saveLocationFolder = new File(saveToLocation);
        if (!saveLocationFolder.exists()) {
            saveLocationFolder.mkdirs();
        }

        map.put("type", "nt:file");
        Query query = queryBuilder.createQuery(PredicateGroup.create(map), workflowSession.adaptTo(Session.class));
        query.setStart(0);
        query.setHitsPerPage(20);

        SearchResult result = query.getResult();
        log.debug("Got  " + result.getHits().size() + " attachments ");
        Node attachmentNode = null;
        for (Hit hit: result.getHits()) {
            try {
                String path = hit.getPath();
                log.debug("The attachment title is  " + hit.getTitle() + " and the attachment path is  " + path);
                attachmentNode = workflowSession.adaptTo(Session.class).getNode(path + "/jcr:content");
                InputStream documentStream = attachmentNode.getProperty("jcr:data").getBinary().getStream();
                Document attachmentDoc = new Document(documentStream);
                attachmentDoc.copyToFile(new File(saveLocationFolder + File.separator + hit.getTitle()));
                attachmentDoc.close();
            } catch (Exception e) {
                log.debug("Error saving file " + e.getMessage());
            }
```

第1行 — 定義元件的屬性。 此 `process.label` 屬性是您將OSGi元件與程式步驟建立關聯時會看到的內容，如下列其中一個熒幕擷取畫面所示。

行13-15 — 傳遞至此OSGi元件的程式引數會使用「，」分隔符號進行分割。 接著，系統會從字串陣列中擷取attachmentPath和saveToLocation的值。

* attachmentPath — 這是您在調適型表單中指定之位置，也就是您設定調適型表單的提交動作以叫用AEM Workflow時的位置。 這是您希望附件相對於工作流程裝載儲存在AEM中的資料夾名稱。

* saveToLocation — 這是您希望將附件儲存在AEM伺服器檔案系統中的位置。

這兩個值會以程式引數傳遞，如下面的熒幕擷圖所示。

![Processstep](assets/implement-process-step.gif)

QueryBuilder服務用於查詢型別的節點 `nt:file` 在attachmentsPath資料夾下。 其餘程式碼會逐一檢視搜尋結果，以建立Document物件並將其儲存至檔案系統。


>[!NOTE]
>
>由於我們使用的是AEM Forms專屬的Document物件，因此您需要在maven專案中包含aemfd-client-sdk相依性。 群組識別碼為 `com.adobe.aemfd` 而成品id為 `aemfd-client-sdk`.

#### 建置和部署

[依照此處的說明建置套件組合](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html)
[請確定該套件組合已部署且處於作用中狀態](http://localhost:4502/system/console/bundles)

建立工作流程模型。 在工作流程模型中拖放流程步驟。 將程式步驟與「將最適化表單附件儲存至檔案系統」相關聯。

提供必要的程式引數，以逗號分隔。 例如Attachments，c：\\scrappp\\。 第一個引數是相較於工作流程裝載儲存最適化表單附件時的資料夾。 這必須是您在設定最適化表單的提交動作時指定的相同值。 第二個引數是儲存附件的位置。

建立最適化表單。 將檔案附件元件拖放到表單上。 設定表單的提交動作，以叫用先前步驟建立的工作流程。 提供適當的附件路徑。

儲存設定。

預覽表單。 新增一些附件並提交表單。 附件應儲存至檔案系統，並置於您在工作流程中指定的位置。
