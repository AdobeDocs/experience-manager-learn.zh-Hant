---
title: 通過合併資料生成打印通道文檔
seo-title: 通過合併資料生成打印通道文檔
description: 了解如何合併輸入流中包含的資料，以產生列印通道檔案
seo-description: 了解如何合併輸入流中包含的資料，以產生列印通道檔案
feature: 互動式通訊
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 開發
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '504'
ht-degree: 1%

---

# 使用提交的資料生成打印渠道文檔

列印管道檔案通常是透過表單資料模型的get服務，從後端資料來源擷取資料而產生。 在某些情況下，您可能需要使用提供的資料產生列印管道檔案。 例如 — 客戶填寫受益表單的更改，並且您可能希望使用已提交表單中的資料生成打印渠道文檔。 若要完成此使用案例，需遵循下列步驟

## 建立預填服務

服務名「ccm-print-test」將用於訪問此服務。 定義此預填服務後，您可以在servlet或工作流進程步驟實施中訪問此服務，以生成打印通道文檔。

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

如下所示，將執行workflowProcess實施代碼片段。當AEM Workflow中的進程步驟與此實施相關聯時，將執行此代碼。 此實作需要3個程式引數，說明如下：

* 配置適用性表單時指定的DataFile路徑名稱
* 打印管道模板的名稱
* 生成的打印通道文檔的名稱

第98行 — 由於適用性表單是以表單資料模型為基礎，因此會擷取駐留在afBoundData資料之資料節點中的資料。
第128行 — 設定了資料選項服務名。 記下服務名稱。 它必須與先前程式碼清單第45行中傳回的名稱相符。
第135行 — 使用PrintChannel對象的渲染方法生成文檔


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

若要在您的伺服器上測試，請執行下列步驟：

* [設定Day CQ Mail Service。](https://helpx.adobe.com/experience-manager/6-5/communities/using/email.html) 這是傳送電子郵件時所需的，因為檔案是以附件的形式產生。
* [使用服務用戶包部署開發](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* 請務必在Apache Sling Service使用者對應程式服務設定中新增下列項目
* **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**
* [將與本文相關的資產下載並解壓縮至您的檔案系統](assets/prefillservice.zip)
* [使用AEM套件管理器匯入下列套件](http://localhost:4502/crx/packmgr/index.jsp)
   1. beneficiaryconfirmationic.zip
   2. changeofbeneficiaryform.zip
   3. generatebeneficiaryworkflow.zip
* [使用AEM Felix Web Console部署下列項目](http://localhost:4502/system/console/bundles)

   * GenerateIC.GenerateIC.core-1.0-SNAPSHOT.jar。 此套件包含本文提及的程式碼。

* [開啟ChangeOfVentairForm](http://localhost:4502/content/dam/formsanddocuments/changebeneficiary/jcr:content?wcmmode=disabled)
* 確認最適化表單已設定為提交至AEM Workflow，如下所示
   ![影像](assets/generateic.PNG)
* [配置工作流模型。](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ChangesToBeneficiary.html)請確定程式步驟和傳送電子郵件元件的設定是根據您的環境所設定
* [預覽ChangeOfVentailForm。](http://localhost:4502/content/dam/formsanddocuments/changebeneficiary/jcr:content?wcmmode=disabled) 填寫一些詳細資訊並提交
* 該工作流應被調用，IC打印通道文檔應作為附件發送到在發送電子郵件元件中指定的收件人
