---
title: 組合表單附件
description: 按指定順序組合表單附件
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

# 組合表單附件

本文提供以指定順序組合最適化表單附件的資產。 表單附件必須為pdf格式，此范常式式碼才能運作。 以下是使用案例。
填寫最適化表單的使用者會將一或多個PDF檔案附加至表單。
提交表單時，組合表單附件以生成一個pdf。 您可以指定裝配附件以生成最終pdf的順序。

## 建立實作WorkflowProcess介面的OSGi元件

建立實作的OSGi元件 [com.adobe.granite.workflow.exec.WorkflowProcess介面](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/exec/WorkflowProcess.html). 此元件中的代碼可與AEM工作流程中的處理步驟元件相關聯。 在此元件中實現了介面com.adobe.granite.workflow.exec.WorkflowProcess的執行方法。

提交適用性表單以觸發AEM工作流程時，提交的資料會儲存在裝載資料夾下的指定檔案中。 例如，這是提交的資料檔案。 我們需要組合在idcard和bankstatement標籤下指定的附件。
![submitted-data](assets/submitted-data.JPG).

### 取得標籤名稱

附件的順序在工作流中指定為處理步驟參數，如下方螢幕擷取畫面所示。 在此，我們組裝添加到現場身份證的附件，然後是銀行對帳單

![過程步驟](assets/process-step.JPG)

下列程式碼片段會從程式引數中擷取附件名稱

```java
String  []attachmentNames  = arg2.get("PROCESS_ARGS","string").toString().split(",");
```

### 從附件名稱建立DDX

然後我們需要 [文檔描述XML(DDX)](https://helpx.adobe.com/pdf/aem-forms/6-2/ddxRef.pdf) 組合器服務用於組合文檔的文檔。 以下是從進程參數建立的DDX。 NoForms元素可讓您在組裝XFA型檔案之前先展平這些檔案。 請注意，PDF源元素按流程參數中指定的正確順序排列。

![ddx-xml](assets/ddx.PNG)

### 建立文檔地圖

然後，我們將建立一個文檔映射，其中附件名稱為鍵，附件為值。 查詢建立器服務用於查詢裝載路徑下的附件，並建立檔案地圖。 組合器服務需要此文檔映射和DDX來組合最終的PDF。

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

### 使用AssemblerService來組合文檔

建立DDX和文檔映射後，下一步是使用AssemblerService來組合文檔。
以下代碼組合併返回組合的pdf。

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

### 在裝載資料夾下儲存已組合的pdf

最後一步是將已組合的pdf儲存在裝載資料夾下。 然後，您就可以在工作流程的後續步驟中存取此pdf，以便進一步處理。
下列程式碼片段用於將檔案儲存在裝載資料夾下

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

以下是組裝和儲存表單附件後的裝載資料夾結構。

![有效載荷結構](assets/payload-structure.JPG)

### 若要讓此功能在您的AEM伺服器上運作

* 下載 [組合表單附件表單](assets/assemble-form-attachments-af.zip) 到本地系統。
* 從匯入表單[Forms和檔案](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments) 頁面。
* 下載 [工作流程](assets/assemble-form-attachments.zip) 並使用套件管理器匯入至AEM。
* 下載 [自訂套件](assets/assembletaskattachments.assembletaskattachments.core-1.0-SNAPSHOT.jar)
* 使用部署並啟動捆綁包 [Web主控台](http://localhost:4502/system/console/bundles)
* 將瀏覽器指向 [組合附件表單](http://localhost:4502/content/dam/formsanddocuments/assembleattachments/jcr:content?wcmmode=disabled)
* 在「ID文檔」中添加附件，並將一些PDF文檔添加到銀行對帳單部分
* 提交表單以觸發工作流程
* 檢查工作流程的 [crx中的裝載資料夾](http://localhost:4502/crx/de/index.jsp#/var/fd/dashboard/payload) 對於組合的pdf

>[!NOTE]
> 如果您已為自訂套件組合啟用記錄器，則DDX和已組合的檔案會寫入AEM安裝的資料夾。
