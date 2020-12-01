---
title: 實施自訂流程步驟
seo-title: 實施自訂流程步驟
description: 使用自定義流程步驟將自適應表單附件寫入檔案系統
seo-description: 使用自定義流程步驟將自適應表單附件寫入檔案系統
feature: workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '895'
ht-degree: 0%

---


# 自訂流程步驟

本教學課程適用於需要實作自訂程式步驟的AEM Forms客戶。 流程步驟可以執行ECMA指令碼或調用自定義java代碼來執行操作。 本教學課程將說明實施由流程步驟執行的WorkflowProcess所需的步驟。

實作自訂流程步驟的主要原因是擴充AEM工作流程。 例如，如果您在工作流程模型中使用AEM Forms元件，您可能想要執行下列作業

* 將最適化表單附件保存到檔案系統
* 控制提交的資料

要完成上述使用案例，您通常將編寫由流程步驟執行的OSGi服務。

## 建立Maven專案

第一步是使用適當的Adobe Maven Archetype來建立大型專案。 詳細步驟列於此[文章](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)中。 將您的主要專案匯入Eclipse後，您就可以開始編寫第一個OSGi元件，以便用於您的程式步驟。


### 建立實作WorkflowProcess的類別

在Eclipse IDE中開啟主專案。 展開&#x200B;**projectname** > **core**資料夾。 展開src/main/java資料夾。 您應該看到以&quot;core&quot;結尾的套件。 建立在此包中實現WorkflowProcess的Java類。 您需要覆寫execute方法。 execute方法的簽名如下
public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)拋出WorkflowException
execute方法可存取下列3個變數

**WorkItem**:workItem變數將授予對與工作流程相關資料的存取權。此處提供公用API檔案[。](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**WorkflowSession**:此workflowSession變數可讓您控制工作流程。公開API檔案可在[這裡](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)取得

**MetaDataMap**:所有與工作流關聯的元資料。傳遞給流程步驟的任何流程參數都可以使用MetaDataMap對象。[API檔案](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html)

在本教學課程中，我們將將新增至Adaptive Form的附件寫入AEM工作流程的檔案系統。

要完成此使用案例，請編寫以下java類

讓我們來看看這個程式碼

```
@Component(property = { Constants.SERVICE_DESCRIPTION + "=Write Adaptive Form Attachments to File System",
        Constants.SERVICE_VENDOR + "=Adobe Systems",
        "process.label" + "=Save Adaptive Form Attachments to File System" })
public class WriteFormAttachmentsToFileSystem implements WorkflowProcess {
     private static final Logger log = LoggerFactory.getLogger(WriteFormAttachmentsToFileSystem.class);
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
 
        String attachmentsFilePath = payloadPath + "/" + attachmentsPath + "/attachments";
        log.debug("The data file path is " + attachmentsFilePath);
 
        ResourceResolver resourceResolver = workflowSession.adaptTo(ResourceResolver.class);
 
        Resource attachmentsNode = resourceResolver.getResource(attachmentsFilePath);
        Iterator<Resource> attachments = attachmentsNode.listChildren();
        while (attachments.hasNext()) {
            Resource attachment = attachments.next();
            String attachmentPath = attachment.getPath();
            String attachmentName = attachment.getName();
 
            log.debug("The attachmentPath is " + attachmentPath + " and the attachmentname is " + attachmentName);
            com.adobe.aemfd.docmanager.Document attachmentDoc = new com.adobe.aemfd.docmanager.Document(attachmentPath,
                    attachment.getResourceResolver());
            try {
                File file = new File(saveToLocation + File.separator + workItem.getId());
                if (!file.exists()) {
                    file.mkdirs();
                }
 
                attachmentDoc.copyToFile(new File(file + File.separator + attachmentName));
 
                log.debug("Saved attachment" + attachmentName);
                attachmentDoc.close();
 
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
 
        }
 
    }
```

第1行——定義元件的屬性。 將OSGi元件與流程步驟關聯時，將會看到process.label屬性，如下面的螢幕截圖所示。

第13-15行——傳遞給此OSGi元件的進程參數使用&quot;,&quot;分隔符進行拆分。 隨後，attachmentPath和saveToLocation的值會從字串陣列中擷取。

* attachmentPath —— 這與您在Adaptive Form中指定的位置相同，當您設定Adaptive Form的提交動作來叫用AEM Workflow時。 這是您要將附件儲存在AEM中，與工作流程裝載相關的檔案夾名稱。

* saveToLocation —— 這是您要將附件儲存在AEM伺服器檔案系統上的位置。

這兩個值會作為流程參數傳遞，如下面的螢幕擷取所示。

![ProcessStep](assets/implement-process-step.gif)


第19行：然後，我們構建attachmentFilePath。 附件檔案路徑類似

    /var/fd/dashboard/payload/server0/2018-11-19/3EF6ENASOQTHCPLNDYVNAM7OKA_7/附件／附件

* 「附件」是與工作流的裝載相關的資料夾的名稱，該裝載是在配置Adaptive Form的提交選項時指定的。

   ![提交選項](assets/af-submit-options.gif)

第24-26行——獲取ResourceResolver，然後獲取指向attachmentFilePath的資源。

其餘的代碼通過使用API在指向attachmentFilePath的資源的子對象中循環來建立文檔對象。 此檔案物件是AEM Forms專屬的。 然後，我們使用文檔對象的copyToFile方法來保存文檔對象。

>[!NOTE]
>
>由於我們使用AEM Forms專屬的Document物件，因此您必須在您的主要專案中加入aemfd-client-sdk相依性。 群組ID為com.adobe.aemfd，物件ID為aemfd-client-sdk。

#### 建立和部署

[按照此處所述構建包確](https://helpx.adobe.com/experience-manager/using/maven_arch13.html#BuildtheOSGibundleusingMaven)
[保包已部署且處於活動狀態](http://localhost:4502/system/console/bundles)

建立工作流程模型。 在工作流模型中拖放流程步驟。 將流程步驟與「將最適化表單附件保存到檔案系統」關聯。

提供以逗號分隔的必要進程參數。 例如，附件，c:\\scrappp\\。 第一個引數是相對於工作流裝載將儲存Adaptive Form附件的資料夾。 這必須與您在配置Adaptive Form的提交操作時指定的值相同。 第二個參數是您希望附件儲存的位置。

建立最適化表單。 將「檔案附件」元件拖放到表單中。 設定表單的提交動作，以叫用在先前步驟中建立的工作流程。 提供適當的附件路徑。

儲存設定。

預覽表格。 添加幾個附件並提交表單。 附件應在工作流中指定的位置保存到檔案系統。

