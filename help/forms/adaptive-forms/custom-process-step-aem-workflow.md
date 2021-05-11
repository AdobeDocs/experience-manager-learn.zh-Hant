---
title: 實施自訂流程步驟
seo-title: 實施自訂流程步驟
description: 使用自定義流程步驟將自適應表單附件寫入檔案系統
seo-description: 使用自定義流程步驟將自適應表單附件寫入檔案系統
feature: 工作流程
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: 開發
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: dbc0a35ae96594fec1e10f411d57d2a3812c1cf2
workflow-type: tm+mt
source-wordcount: '833'
ht-degree: 0%

---


# 自訂流程步驟

本教學課程適用於需要實作自訂程式步驟的AEM Forms客戶。 流程步驟可以執行ECMA指令碼或調用自定義Java代碼來執行操作。 本教學課程將說明實施由流程步驟執行的WorkflowProcess所需的步驟。

實作自訂流程步驟的主要原因是擴充工AEM作流程。 例如，如果您在工作流模型中使用AEM Forms元件，則可能需要執行以下操作

* 將最適化表單附件保存到檔案系統
* 控制提交的資料

要完成上述使用案例，您通常將編寫由流程步驟執行的OSGi服務。

## 建立Maven專案

第一步是使用適當的AdobeMaven Archetype建立主項目。 詳細步驟列於此[文章](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/create-your-first-osgi-bundle.html?lang=en)中。 將您的主要專案匯入Eclipse後，您就可以開始編寫第一個OSGi元件，以便用於您的程式步驟。


### 建立實作WorkflowProcess的類別

在Eclipse IDE中開啟主專案。 展開&#x200B;**projectname** > **core**資料夾。 展開src/main/java資料夾。 您應該看到以&quot;core&quot;結尾的套件。 建立在此包中實現WorkflowProcess的Java類。 您需要覆寫execute方法。 execute方法的簽名如下
public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)拋出WorkflowException
execute方法可存取下列3個變數

**WorkItem**:workItem變數將授予對與工作流程相關資料的存取權。此處提供公用API檔案[。](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**WorkflowSession**:此workflowSession變數可讓您控制工作流程。公開API檔案可在[這裡](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)取得

**MetaDataMap**:所有與工作流關聯的元資料。傳遞給流程步驟的任何流程參數都可以使用MetaDataMap對象。[API檔案](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html)

在本教程中，我們將將添加到Adaptive Form的附件作為工作流的一部分寫入檔案AEM系統。

要完成此使用案例，請編寫以下java類

讓我們來看看這個程式碼

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

第1行——定義元件的屬性。 將OSGi元件與流程步驟關聯時，將會看到process.label屬性，如下面的螢幕截圖所示。

第13-15行——傳遞給此OSGi元件的進程參數使用&quot;,&quot;分隔符進行拆分。 隨後，attachmentPath和saveToLocation的值會從字串陣列中擷取。

* attachmentPath —— 這與在Adaptive Form中指定的位置相同，當您配置了Adaptive Form的提交操作以調用Workflow時AEM。 這是您希望附件相對於工作流的裝載保存在AEM的資料夾的名稱。

* saveToLocation —— 這是您希望附件保存在伺服器文AEM件系統上的位置。

這兩個值會作為流程參數傳遞，如下面的螢幕擷取所示。

![ProcessStep](assets/implement-process-step.gif)

QueryBuilder服務用於查詢attachmentsPath資料夾下nt:file類型的節點。 其餘的程式碼會重複搜尋結果，以建立Document物件並儲存至檔案系統


>[!NOTE]
>
>由於我們使用的是AEM Forms特有的Document物件，因此您必須在您的主要專案中納入aemfd-client-sdk相依性。 群組ID為com.adobe.aemfd，物件ID為aemfd-client-sdk。

#### 建立和部署

[按照此處所述構建包確](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/create-your-first-osgi-bundle.html?lang=en#build-your-project)
[保包已部署且處於活動狀態](http://localhost:4502/system/console/bundles)

建立工作流程模型。 在工作流模型中拖放流程步驟。 將流程步驟與「將最適化表單附件保存到檔案系統」關聯。

提供以逗號分隔的必要進程參數。 例如，附件，c:\\scrappp\\。 第一個引數是相對於工作流裝載將儲存Adaptive Form附件的資料夾。 這必須與您在配置Adaptive Form的提交操作時指定的值相同。 第二個參數是您希望附件儲存的位置。

建立最適化表單。 將「檔案附件」元件拖放到表單中。 設定表單的提交動作，以叫用在先前步驟中建立的工作流程。 提供適當的附件路徑。

儲存設定。

預覽表格。 添加幾個附件並提交表單。 附件應在工作流中指定的位置保存到檔案系統。

