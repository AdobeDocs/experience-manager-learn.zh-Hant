---
title: 組合表單附件
description: 按指定順序裝配表單附件
feature: assembler
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 6406
thumbnail: kt-6406.jpg
translation-type: tm+mt
source-git-commit: 3e8b820939c2d39ef9a17f7d7aaef87cd9cdbbbb
workflow-type: tm+mt
source-wordcount: '633'
ht-degree: 0%

---


# 組合表單附件

本文提供資產以指定順序組合最適化表單附件。 表單附件必須為pdf格式，此范常式式碼才能運作。 以下是使用案例。
使用者填寫最適化表單會將一或多份PDF檔案附加至表單。
在提交表單時，將表單附件組合為一個PDF。 您可以指定附件的組裝順序，以生成最終PDF。

## 建立實作WorkflowProcess介面的OSGi元件

建立實作[com.adobe.granite.workflow.exec.WorkflowProcess介面](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/exec/WorkflowProcess.html)的OSGi元件。 此元件中的程式碼可與AEM工作流程中的流程步驟元件相關聯。 在此元件中實現了介面com.adobe.granite.workflow.exec.WorkflowProcess的執行方法。

提交最適化表單以觸發AEM工作流程時，提交的資料會儲存在裝載檔案夾下的指定檔案中。 例如，這是已提交的資料檔案。 我們需要將idcard和bankstatements標籤下指定的附件組合起來。
![submitted-data](assets/submitted-data.JPG).

### 取得標籤名稱

附件的順序在工作流中指定為流程步驟參數，如下面螢幕抓圖所示。 我們在這裡將添加到現場身份證的附件組合起來，然後是銀行結算表

![process-step](assets/process-step.JPG)

以下代碼片段從進程參數中提取附件名稱

```java
String  []attachmentNames  = arg2.get("PROCESS_ARGS","string").toString().split(",");
```

### 從附件名稱建立DDX

然後，我們需要建立[文檔描述XML(DDX)](https://helpx.adobe.com/pdf/aem-forms/6-2/ddxRef.pdf)文檔，該文檔由匯編器服務用來組合文檔。 以下是從進程參數建立的DDX。 NoForms元素允許您在組裝基於XFA的文檔之前對其進行平面化。 請注意，PDF來源元素的順序正確，如流程引數中所指定。

![ddx-xml](assets/ddx.PNG)

### 建立檔案地圖

然後，我們將建立一個文檔的映射，其中附件名稱作為鍵，附件作為值。 查詢構建器服務用於查詢裝載路徑下的附件並構建文檔的映射。 匯編服務需要此文檔映射和DDX來組合最終的pdf。

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
下列程式碼會組合併傳回已組合的pdf。

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

### 將匯整的PDF儲存在裝載資料夾下

最後一個步驟是將組合的pdf儲存在裝載檔案夾下。 然後，您可以在工作流程的後續步驟中存取此PDF，以進一步處理。
下列程式碼片段可用來將檔案儲存在裝載資料夾下

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

以下是組合和儲存表單附件後的裝載資料夾結構。

![有效載荷結構](assets/payload-structure.JPG)

### 若要讓此功能在您的AEM伺服器上運作

* 將[組合表單附件表單](assets/assemble-form-attachments-af.zip)下載到本地系統。
* 從[表單與檔案](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)頁面匯入表單。
* 下載[workflow](assets/assemble-form-attachments.zip)並使用套件管理器匯入AEM。
* 下載[自訂搭售](assets/assembletaskattachments.assembletaskattachments.core-1.0-SNAPSHOT.jar)
* 使用[Web控制台](http://localhost:4502/system/console/bundles)部署並啟動包
* 將瀏覽器指向[AssembleAttachments Form](http://localhost:4502/content/dam/formsanddocuments/assembleattachments/jcr:content?wcmmode=disabled)
* 在「ID檔案」中新增附件和數張PDF檔案至銀行對帳單區段
* 提交表單以觸發工作流程
* 檢查crx](http://localhost:4502/crx/de/index.jsp#/var/fd/dashboard/payload)中工作流程的[裝載資料夾，以取得已組合的pdf

>[!NOTE]
> 如果您已為自訂搭售啟用記錄程式，則DDX和已組合的檔案會寫入AEM安裝的資料夾。

