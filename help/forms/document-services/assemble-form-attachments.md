---
title: 組合表單附件
description: 以指定順序組合表單附件
feature: Assembler
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6406
thumbnail: kt-6406.jpg
topic: Development
role: Developer
level: Experienced
exl-id: a5df8780-b7ab-4b91-86f6-a24392752107
last-substantial-update: 2021-07-07T00:00:00Z
duration: 150
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 0%

---

# 組合表單附件

本文提供資產，以依指定順序組合最適化表單附件。 表單附件必須是PDF格式，此範常式式碼才能運作。 以下是使用案例。
使用者填寫最適化表單時，會將一或多個pdf檔案附加到表單中。
在表單提交時，組合表單附件以產生一個PDF。 您可以指定組裝附件以產生最終pdf的順序。

## 建立實作WorkflowProcess介面的OSGi元件

建立實作[com.adobe.granite.workflow.exec.WorkflowProcess介面](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/exec/WorkflowProcess.html)的OSGi元件。 此元件中的程式碼可與AEM工作流程中的流程步驟元件相關聯。 在此元件中實作介面com.adobe.granite.workflow.exec.WorkflowProcess的執行方法。

提交最適化表單以觸發AEM工作流程時，提交的資料會儲存在裝載資料夾下的指定檔案中。 例如，這是提交的資料檔案。 我們需要組合idcard和bankstatements標籤下指定的附件。
![已提交資料](assets/submitted-data.JPG)。

### 取得標籤名稱

附件的順序會在工作流程中指定為流程步驟引數，如下方熒幕擷取畫面所示。 我們在這裡組合新增到欄位資訊卡隨後是銀行帳戶的附件

![process-step](assets/process-step.JPG)

下列程式碼片段會從程式引數中擷取附件名稱

```java
String  []attachmentNames  = arg2.get("PROCESS_ARGS","string").toString().split(",");
```

### 從附件名稱建立DDX

然後，我們需要建立[檔案描述XML (DDX)](https://helpx.adobe.com/pdf/aem-forms/6-2/ddxRef.pdf)檔案，Assembler服務會使用這些檔案來組合檔案。 以下是從處理序引數建立的DDX。 NoForms元素可讓您在組裝XFA型檔案之前將其平面化。 請注意，PDF來源元素的順序正確無誤，如程式引數中所指定。

![ddx-xml](assets/ddx.PNG)

### 建立檔案地圖

然後，我們會以附件名稱作為索引鍵，以附件作為值來建立檔案地圖。 查詢產生器服務用於查詢裝載路徑下的附件，並建立檔案地圖。 組裝服務需要這份檔案地圖以及DDX來組裝最終pdf。

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

### 使用AssemblerService來組裝檔案

建立DDX和檔案結構圖後，下一步是使用AssemblerService來組裝檔案。
下列程式碼會組合併傳回組合的pdf。

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

### 將組合的pdf儲存在承載資料夾下

最後一步是將組合的pdf儲存在承載資料夾下。 接著，您可在工作流程的後續步驟中存取此PDF以進行進一步處理。
下列程式碼片段是用來將檔案儲存在裝載資料夾下

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

以下是組裝及儲存表單附件後的裝載資料夾結構。

![承載結構](assets/payload-structure.JPG)

### 若要讓此功能在您的AEM伺服器上運作

* 將[組合表單附件表單](assets/assemble-form-attachments-af.zip)下載到您的本機系統。
* 從[Forms和檔案](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)頁面匯入表單。
* 下載[工作流程](assets/assemble-form-attachments.zip)並使用封裝管理員匯入AEM。
* 下載[自訂套件](assets/assembletaskattachments.assembletaskattachments.core-1.0-SNAPSHOT.jar)
* 使用[網頁主控台](http://localhost:4502/system/console/bundles)部署及啟動套件組合
* 將瀏覽器指向[組合附件表單](http://localhost:4502/content/dam/formsanddocuments/assembleattachments/jcr:content?wcmmode=disabled)
* 在ID檔案中新增附件，並將一些pdf檔案新增至銀行對帳單區段
* 提交表單以觸發工作流程
* 檢查crx[&#128279;](http://localhost:4502/crx/de/index.jsp#/var/fd/dashboard/payload)中工作流程的裝載資料夾以取得組合的pdf

>[!NOTE]
> 如果您已啟用自訂套裝的記錄器，則DDX且已組裝的檔案會寫入您的AEM安裝檔案夾。
