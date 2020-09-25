---
title: 通過合併資料生成打印渠道文檔
seo-title: 通過合併資料生成打印渠道文檔
description: 瞭解如何透過合併輸入串流中包含的資料來產生列印頻道檔案
seo-description: 瞭解如何透過合併輸入串流中包含的資料來產生列印頻道檔案
feature: interactive-communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '501'
ht-degree: 0%

---

# 使用提交的資料產生列印渠道檔案

列印渠道檔案通常是透過表單資料模型的get服務從後端資料來源擷取資料而產生。 在某些情況下，您可能需要使用提供的資料產生列印渠道檔案。 例如——客戶填寫受益人表單的變更，而您可能想要使用提交表單的資料產生列印渠道檔案。 若要完成此使用案例，必須遵循下列步驟

## 建立預填服務

服務名稱&quot;ccm-print-test&quot;將用於訪問此服務。 一旦定義了此預填充服務，您就可以在servlet或工作流進程步驟實施中訪問此服務，以生成打印渠道文檔。

```java
import java.io.InputStream;
import org.osgi.service.component.annotations.Component;

import com.adobe.forms.common.service.ContentType;
import com.adobe.forms.common.service.DataOptions;
import com.adobe.forms.common.service.DataProvider;
import com.adobe.forms.common.service.FormsException;
import com.adobe.forms.common.service.PrefillData;

@Component(immediate = true, service = {DataProvider.class})
public class ICPrefillService implements DataProvider {

@Override
public String getServiceDescription() {
    // TODO Auto-generated method stub
    return "Prefill Service for IC Print Channel";
}

@Override
public String getServiceName() {
    // TODO Auto-generated method stub
    return "ccm-print-test";
}

@Override
public PrefillData getPrefillData(DataOptions options) throws FormsException {
    // TODO Auto-generated method stub
        PrefillData data = null;
        if (options != null && options.getExtras() != null && options.getExtras().get("data") != null) {
            InputStream is = (InputStream) options.getExtras().get("data");
            data = new PrefillData(is, options.getContentType() != null ? options.getContentType() : ContentType.JSON);
        }
        return data;
    }
}
```

### 建立WorkflowProcess實施

workflowProcess實作程式碼片段如下所示。當AEM Workflow中的程式步驟與此實作相關聯時，就會執行此程式碼。 此實施需要3個進程引數，說明如下：

* 配置最適化表單時指定的DataFile路徑的名稱
* 列印渠道範本的名稱
* 產生的列印渠道檔案的名稱

第98行——由於自適應表單基於表單資料模型，因此提取駐留在afBoundData資料節點中的資料。
第128行——設定「資料選項」服務名。 記下服務名。 它必須與先前程式碼清單第45行中傳回的名稱相符。
第135行——使用PrintChannel物件的演算方法產生檔案


```java
String params = arg2.get("PROCESS_ARGS","string").toString();
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    String dataFile = params.split(",")[0];
    final String icFileName = params.split(",")[1];
    String dataFilePath = payloadPath + "/"+dataFile+"/jcr:content";
    Session session = workflowSession.adaptTo(Session.class);
    Node xmlDataNode = null;
    try {
        xmlDataNode = session.getNode(dataFilePath);
        InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
        JsonParser jsonParser = new JsonParser();
        BufferedReader streamReader = null;
        try {
            streamReader = new BufferedReader(new InputStreamReader(xmlDataStream, "UTF-8"));
        } catch (UnsupportedEncodingException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        StringBuilder responseStrBuilder = new StringBuilder();
        String inputStr;
        try {
            while ((inputStr = streamReader.readLine()) != null)
                responseStrBuilder.append(inputStr);
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        String submittedDataXml = responseStrBuilder.toString();
        JsonObject jsonObject = jsonParser.parse(submittedDataXml).getAsJsonObject().get("afData").getAsJsonObject()
                .get("afBoundData").getAsJsonObject().get("data").getAsJsonObject();
        logger.info("Successfully Parsed gson" + jsonObject.toString());
        InputStream targetStream = IOUtils.toInputStream(jsonObject.toString());
        //InputStream targetStream = new ByteArrayInputStream(jsonObject.toString().getBytes());
        
        // Node dataNode = session.getNode(formName);
        logger.info("Got resource using resource resolver"
                );
        resHelper.callWith(getResolver.getFormsServiceResolver(), new Callable<Void>() {
            @Override
            public Void call() throws Exception {
                System.out.println("The target stream is "+targetStream.available());
                // TODO Auto-generated method stub
                com.adobe.fd.ccm.channels.print.api.model.PrintChannel printChannel = null;
                String formName = params.split(",")[2];
                logger.info("The form name I got was "+formName);
                printChannel = printChannelService.getPrintChannel(formName);
                logger.info("Did i get print channel?");
                com.adobe.fd.ccm.channels.print.api.model.PrintChannelRenderOptions options = new com.adobe.fd.ccm.channels.print.api.model.PrintChannelRenderOptions();
                options.setMergeDataOnServer(true);
                options.setRenderInteractive(false);
                com.adobe.forms.common.service.DataOptions dataOptions = new com.adobe.forms.common.service.DataOptions();
                dataOptions.setServiceName(printChannel.getPrefillService());
                // dataOptions.setExtras(map);
                dataOptions.setContentType(ContentType.JSON);
                logger.info("####Set the content type####");
                dataOptions.setFormResource(getResolver.getFormsServiceResolver().getResource(formName));
                dataOptions.setServiceName("ccm-print-test");
                dataOptions.setExtras(new HashMap<String, Object>());
                dataOptions.getExtras().put("data", targetStream);
                options.setDataOptions(dataOptions);
                logger.info("####Set the data options");
                com.adobe.fd.ccm.channels.print.api.model.PrintDocument printDocument = printChannel
                .render(options);
                logger.info("####Generated the document");
                com.adobe.aemfd.docmanager.Document uploadedDocument = new com.adobe.aemfd.docmanager.Document(
                    printDocument.getInputStream());
                logger.info("Generated the document");
                Binary binary = session.getValueFactory().createBinary(printDocument.getInputStream());
                Session jcrSession = workflowSession.adaptTo(Session.class);
                String dataFilePath = workItem.getWorkflowData().getPayload().toString();
                
                Node dataFileNode = jcrSession.getNode(dataFilePath);
                Node icPdf = dataFileNode.addNode(icFileName, "nt:file");
                Node contentNode = icPdf.addNode("jcr:content", "nt:resource");
                contentNode.setProperty("jcr:data", binary);
                jcrSession.save();
                logger.info("Copied the generated document");
                uploadedDocument.close();
                
                return null;
            }
```

若要在您的伺服器上測試此項，請依照下列步驟進行：

* [設定Day CQ Mail服務。](https://helpx.adobe.com/experience-manager/6-5/communities/using/email.html) 這是傳送電子郵件時所需的，因為檔案是以附件形式產生的。
* [部署使用服務使用者套件進行開發](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* 請確定您已在Apache Sling Service User Mapper Service Configuration中新增下列項目
* **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**
* [將與本文相關的資產下載並解壓縮至您的檔案系統](assets/prefillservice.zip)
* [使用AEM套件管理員匯入下列套件](http://localhost:4502/crx/packmgr/index.jsp)
   1. beneficiaryconfirmationic.zip
   2. changeofbeneficiaryform.zip
   3. generatebeneficiaryworkflow.zip
* [使用AEM Felix Web Console部署下列內容](http://localhost:4502/system/console/bundles)

   * GenerateIC.GenerateIC.core-1.0-SNAPSHOT.jar。 此套件包含本文提及的程式碼。

* [Open ChangeOfBenertForm](http://localhost:4502/content/dam/formsanddocuments/changebeneficiary/jcr:content?wcmmode=disabled)
* 請確定最適化表單已設定為送出至AEM Workflow，如下所示
   ![影像](assets/generateic.PNG)
* [設定工作流程模型。](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ChangesToBeneficiary.html)請確定流程步驟和傳送電子郵件元件的設定符合您的環境
* [預覽ChangeOfBenertForm。](http://localhost:4502/content/dam/formsanddocuments/changebeneficiary/jcr:content?wcmmode=disabled) 填寫部分詳細資訊並提交
* 工作流程應被調用，而IC打印渠道文檔應作為附件發送給在發送電子郵件元件中指定的收件人
