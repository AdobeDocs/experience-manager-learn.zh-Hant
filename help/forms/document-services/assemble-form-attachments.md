---
title: 裝配表單附件
description: 按指定順序裝配表單附件
feature: Assembler
version: 6.4,6.5
kt: 6406
thumbnail: kt-6406.jpg
topic: Development
role: Developer
level: Experienced
exl-id: a5df8780-b7ab-4b91-86f6-a24392752107
last-substantial-update: 2021-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '633'
ht-degree: 0%

---

# 裝配表單附件

本文提供資產以按指定順序裝配自適應表單附件。 表單附件需要採用pdf格式才能使用此示例代碼。 以下是使用案例。
填寫自適應表單的用戶將一個或多個pdf文檔附加到表單。
在提交表單時，將表單附件匯編為生成一個pdf。 您可以指定附件的裝配順序以生成最終pdf。

## 建立實現WorkflowProcess介面的OSGi元件

建立實現 [com.adobe.granite.workflow.exec.WorkflowProcess介面](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/exec/WorkflowProcess.html)。 此元件中的代碼可以與工作流中的流程步驟元件AEM關聯。 在該元件中實現了介面com.adobe.granite.workflow.exec.WorkflowProcess的執行方法。

當提交自適應表單以觸發工AEM作流時，提交的資料將儲存在負載資料夾下的指定檔案中。 例如，這是已提交的資料檔案。 我們需要將IDCARD和銀行對帳單標籤下指定的附件組合起來。
![提交資料](assets/submitted-data.JPG)。

### 獲取標籤名稱

附件的順序在工作流中指定為進程步驟參數，如下面螢幕抓圖所示。 在此，我們將收集添加到現場身份證的附件，然後是銀行對帳單

![過程步驟](assets/process-step.JPG)

以下代碼段從進程參數中提取附件名稱

```java
String  []attachmentNames  = arg2.get("PROCESS_ARGS","string").toString().split(",");
```

### 從附件名稱建立DDX

然後我們需要建立 [文檔說明XML(DDX)](https://helpx.adobe.com/pdf/aem-forms/6-2/ddxRef.pdf) 匯編程式服務用於匯編文檔的文檔。 以下是從進程參數建立的DDX。 NoForms元素允許您在裝配基於XFA的文檔之前展平它們。 請注意，PDF源元素按在進程參數中指定的正確順序排列。

![ddx-xml](assets/ddx.PNG)

### 建立文檔映射

然後，我們將建立一個文檔映射，其中附件名稱為鍵，附件為值。 查詢生成器服務用於查詢有效負載路徑下的附件並生成文檔映射。 匯編器服務要匯編最終的pdf檔案，需要此文檔圖和DDX。

```java
public Map<String, Object> createMapOfDocuments(String payloadPath,WorkflowSession workflowSession )
{
  Map<String, String> queryMap = new HashMap<String, String>();
  Map<String,Object>mapOfDocuments = new HashMap<String,Object>();
  queryMap.put("type", "nt:file");
  queryMap.put("path",payloadPath);
  Query query = queryBuilder.createQuery(PredicateGroup.create(queryMap),workflowSession.adaptTo(Session.class));
  query.setStart(0);
  query.setHitsPerPage(30);
  SearchResult result = query.getResult();
  log.debug("Get result hits "+result.getHits().size());
  for (Hit hit : result.getHits()) {
    try {
          String path = hit.getPath();
          log.debug("The title "+hit.getTitle()+" path "+path);
          if(hit.getTitle().endsWith("pdf"))
           {
             com.adobe.aemfd.docmanager.Document attachmentDocument = new com.adobe.aemfd.docmanager.Document(path);
             mapOfDocuments.put(hit.getTitle(),attachmentDocument);
             log.debug("@@@@Added to map@@@@@ "+hit.getTitle());
           }
        }
    catch (Exception e)
       {
          log.debug(e.getMessage());
       }

}
return mapOfDocuments;
}
```

### 使用AssemblerService匯編文檔

建立DDX和文檔映射後，下一步是使用AssemblerService來匯編文檔。
以下代碼匯編並返回匯編的pdf。

```java
private com.adobe.aemfd.docmanager.Document assembleDocuments(Map<String, Object> mapOfDocuments, com.adobe.aemfd.docmanager.Document ddxDocument)
{
    AssemblerOptionSpec aoSpec = new AssemblerOptionSpec();
    aoSpec.setFailOnError(true);
    AssemblerResult ar = null;
    try
    {
        ar = assemblerService.invoke(ddxDocument, mapOfDocuments, aoSpec);
        return (com.adobe.aemfd.docmanager.Document) ar.getDocuments().get("GeneratedDocument.pdf");
    }
    catch (OperationException e)
    {
        log.debug(e.getMessage());
    }
    return null;
    
}
```

### 將已裝配的pdf保存到負載資料夾下

最後一步是將已裝配的pdf保存到負載資料夾下。 然後，可以在工作流的後續步驟中訪問此pdf，以便進一步處理。
以下代碼段用於將檔案保存在負載資料夾下

```java
Session session = workflowSession.adaptTo(Session.class);
javax.jcr.Node payloadNode =  workflowSession.adaptTo(Session.class).getNode(workItem.getWorkflowData().getPayload().toString());
log.debug("The payload Path is "+payloadNode.getPath());
javax.jcr.Node assembledPDFNode = payloadNode.addNode("assembled-pdf.pdf", "nt:file"); 
javax.jcr.Node jcrContentNode =  assembledPDFNode.addNode("jcr:content", "nt:resource");
Binary binary =  session.getValueFactory().createBinary(assembledDocument.getInputStream());
jcrContentNode.setProperty("jcr:data", binary);
log.debug("Saved !!!!!!"); 
session.save();
```

以下是裝配和儲存表單附件後的負載資料夾結構。

![有效載荷結構](assets/payload-structure.JPG)

### 使此功能在伺服器上AEM工作

* 下載 [裝配表單附件窗體](assets/assemble-form-attachments-af.zip) 到本地系統。
* 從[Forms與文檔](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments) 的子菜單。
* 下載 [工作流](assets/assemble-form-attachments.zip) 並使用包管AEM理器導入。
* 下載 [定制捆綁](assets/assembletaskattachments.assembletaskattachments.core-1.0-SNAPSHOT.jar)
* 使用 [Web控制台](http://localhost:4502/system/console/bundles)
* 將瀏覽器指向 [裝配附件窗體](http://localhost:4502/content/dam/formsanddocuments/assembleattachments/jcr:content?wcmmode=disabled)
* 在「ID文檔」中添加附件，並將幾個PDF文檔添加到銀行對帳單部分
* 提交表單以觸發工作流
* 檢查工作流 [crx中的有效負載資料夾](http://localhost:4502/crx/de/index.jsp#/var/fd/dashboard/payload) 對於已裝配的pdf

>[!NOTE]
> 如果已為自定義捆綁包啟用記錄器，則DDX和已裝配的檔案將寫入安裝的文AEM件夾。
