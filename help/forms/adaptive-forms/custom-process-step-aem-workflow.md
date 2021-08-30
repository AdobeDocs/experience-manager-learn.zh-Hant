---
title: 實作自訂程式步驟
description: 使用自訂處理步驟將適用性表單附件寫入檔案系統
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
source-git-commit: 2b7f0f6c34803672cc57425811db89146b38a70a
workflow-type: tm+mt
source-wordcount: '815'
ht-degree: 0%

---


# 自訂處理步驟

本教學課程適用於需要實作自訂程式步驟的AEM Forms客戶。 處理步驟可執行ECMA指令碼，或呼叫自訂Java程式碼以執行操作。 本教學課程將說明實作由處理步驟執行的WorkflowProcess所需的步驟。

實作自訂程式步驟的主要原因是擴充AEM工作流程。 例如，如果您在工作流程模型中使用AEM Forms元件，則可能要執行下列操作

* 將適用性表單附件儲存至檔案系統
* 操控提交的資料

若要完成上述使用案例，您通常會撰寫由處理步驟執行的OSGi服務。

## 建立Maven專案

第一步是使用適當的AdobeMaven原型來建立Maven專案。 詳細步驟列於[article](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html)中。 將maven專案匯入eclipse後，您就可以開始編寫可在處理步驟中使用的第一個OSGi元件。


### 建立實施WorkflowProcess的類

在eclipse IDE中開啟maven專案。 展開&#x200B;**projectname** > **core**資料夾。 展開src/main/java資料夾。 您應該會看到結尾為「core」的套件。 在此包中建立實施WorkflowProcess的Java類。 您需要覆寫執行方法。 執行方法的簽名如下
public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)擲回WorkflowException
執行方法可存取下列3個變數

**工作項**:workItem變數將授予對與工作流相關資料的訪問權。公開API檔案可在[此處取得。](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**WorkflowSession**:此workflowSession變數可讓您控制工作流程。公開API檔案可在[此處](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)取得

**MetaDataMap**:與工作流程相關聯的所有中繼資料。任何傳遞到進程步驟的進程參數都可使用MetaDataMap對象。[API檔案](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html)

在本教學課程中，我們將將新增至適用性表單的附件寫入檔案系統，作為AEM工作流程的一部分。

為了完成此使用案例，寫入了以下java類

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

第1行 — 定義元件的屬性。 將OSGi元件與處理步驟關聯時，您會看到process.label屬性，如下方其中一個螢幕擷取畫面所示。

第13-15行 — 傳遞給此OSGi元件的進程參數使用「，」分隔符進行拆分。 然後，會從字串陣列中擷取attachmentPath和saveToLocation的值。

* attachmentPath — 這與您在適用性表單中指定的位置相同，當您設定適用性表單的提交動作來叫用AEM Workflow時。 這是您要將附件儲存至AEM中，而與工作流程裝載相關的資料夾名稱。

* saveToLocation — 這是您希望將附件保存在AEM伺服器的檔案系統上的位置。

這兩個值會以程式引數的形式傳遞，如下方螢幕擷取所示。

![ProcessStep](assets/implement-process-step.gif)

QueryBuilder服務用於查詢attachmentsPath資料夾下nt:file類型的節點。 其餘的代碼會在搜索結果中反覆顯示，以建立Document對象並將其保存到檔案系統


>[!NOTE]
>
>由於我們使用的是AEM Forms專屬的檔案物件，因此您必須在您的Maven專案中包含aemfd-client-sdk相依性。 群組ID為com.adobe.aemfd，工件ID為aemfd-client-sdk。

#### 建置和部署

[按照此處所述構建](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/create-your-first-osgi-bundle.html?lang=en#build-your-project)
[包確保包已部署且處於活動狀態](http://localhost:4502/system/console/bundles)

建立工作流模型。 在工作流模型中拖放流程步驟。 將流程步驟與「將適用性表單附件保存到檔案系統」相關聯。

提供以逗號分隔的必要處理引數。 例如，附件，c:\\scrappp\\。 第一個引數是相對於工作流程裝載儲存適用性表單附件的資料夾。 這必須與您在設定適用性表單的提交動作時指定的值相同。 第二個參數是您希望儲存附件的位置。

建立最適化表單。 將「檔案附件」元件拖放至表單。 配置表單的提交操作，以調用在先前步驟中建立的工作流。 提供適當的附件路徑。

儲存設定。

預覽表單。 新增幾個附件並提交表單。 附件應會儲存至檔案系統，且位於您在工作流程中指定的位置。

