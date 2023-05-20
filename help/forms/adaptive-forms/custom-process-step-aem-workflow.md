---
title: 實施自定義流程步驟
description: 使用自定義進程步驟將自適應表單附件寫入檔案系統
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 879518db-3f05-4447-86e8-5802537584e5
last-substantial-update: 2021-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '813'
ht-degree: 0%

---

# 自定義流程步驟

本教程適用於需要實施定制流程步驟的AEM Forms客戶。 進程步驟可以執行ECMA指令碼或調用自定義java代碼以執行操作。 本教程將介紹實施由進程步驟執行的WorkflowProcess所需的步驟。

實現自定義流程步驟的主要原因是擴展工AEM作流。 例如，如果在工作流模型中使用AEM Forms元件，則可能需要執行以下操作

* 將自適應表單附件保存到檔案系統
* 操縱提交的資料

要完成上述使用情形，您通常會編寫一個OSGi服務，該服務由流程步驟執行。

## 建立Maven項目

第一步是使用適當的AdobeMaven Archetype建立主項目。 詳細步驟列在本中 [文章](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html)。 將主項目導入eclipse後，您就可以開始編寫可以在流程步驟中使用的第一個OSGi元件。


### 建立實現WorkflowProcess的類

在eclipse IDE中開啟maven項目。 展開 **項目名稱** > **核** 的子菜單。 展開src/main/java資料夾。 您應看到以「核心」結尾的包。 建立在此包中實現WorkflowProcess的Java類。 您需要覆蓋execute方法。 執行方法的簽名如下：public void execute(WorkItem workItem、WorkflowSession workflowSession、MetaDataMap processArguments)拋出WorkflowException執行方法授予對以下3個變數的訪問權

**工作項**:workItem變數將授予對與工作流相關的資料的訪問權限。 公用API文檔可用 [給。](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**工作流會話**:此workflowSession變數將允許您控制工作流。 公用API文檔可用 [這裡](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**元資料映射**:與工作流關聯的所有元資料。 傳遞給進程步驟的任何進程參數都可使用MetaDataMap對象。[API文檔](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html)

在本教程中，我們將將添加到「自適應表單」的附件作為「工作流」的一部分寫入檔案AEM系統。

要完成此用例，請編寫以下java類

讓我們看看這個代碼

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

行1 — 定義元件的屬性。 process.label屬性是將OSGi元件與進程步驟關聯時將看到的內容，如下面螢幕截圖之一所示。

行13-15 — 傳遞給此OSGi元件的進程參數使用「，」分隔符進行拆分。 然後，從字串陣列中提取attachmentPath和saveToLocation的值。

* attachmentPath — 這與在將Adaptive Form的提交操作配置為調用工作流時在Adaptive Form中指定的位置AEM相同。 這是您希望附件相對於工作流負載保存AEM的資料夾的名稱。

* saveToLocation — 這是您希望將附件保存到伺服器文AEM件系統的位置。

這兩個值作為進程參數傳遞，如下面的螢幕快照所示。

![進程步驟](assets/implement-process-step.gif)

QueryBuilder服務用於查詢attachmentsPath資料夾下nt:file類型的節點。 其餘的代碼通過搜索結果迭代以建立文檔對象並將其保存到檔案系統


>[!NOTE]
>
>由於我們使用的是特定於AEM Forms的Document對象，因此需要在主項目中包含aemfd-client-sdk依賴項。 組ID為com.adobe.aemfd，項目ID為aemfd-client-sdk。

#### 構建和部署

[按此處所述構建捆綁包](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html)
[確保已部署該捆綁包並處於活動狀態](http://localhost:4502/system/console/bundles)

建立工作流模型。 在工作流模型中拖放流程步驟。 將流程步驟與「將自適應表單附件保存到檔案系統」關聯。

提供用逗號分隔的必要進程參數。 例如，附件，c:\\scrappp\\。 第一個參數是將相對於工作流負載儲存自適應表單附件時的資料夾。 這必須與配置Adaptive Form的提交操作時指定的值相同。 第二個參數是要儲存附件的位置。

建立最適化表單. 將「檔案附件」元件拖放到窗體中。 配置表單的提交操作以調用在前面步驟中建立的工作流。 提供相應的附件路徑。

保存設定。

預覽窗體。 添加幾個附件並提交表單。 附件應保存到工作流中您指定的位置中的檔案系統。
