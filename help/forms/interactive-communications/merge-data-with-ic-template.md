---
title: 通過合併資料生成打印通道文檔
description: 瞭解如何通過合併輸入流中包含的資料來生成打印通道文檔
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 3bfbb4ef-0c51-445a-8d7b-43543a5fa191
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '479'
ht-degree: 1%

---

# 使用提交的資料生成打印渠道文檔

打印通道文檔通常通過從後端資料源通過表單資料模型的get服務獲取資料來生成。 在某些情況下，您可能需要使用提供的資料生成打印渠道文檔。 例如 — 客戶填寫受益人表單的更改，並且您可能希望使用已提交表單中的資料生成打印渠道文檔。 要完成此使用情形，需要執行以下步驟

## 建立預填服務

服務名稱「ccm-print-test」用於訪問此服務。 一旦定義了此預填充服務，您就可以在servlet或工作流進程步驟實現中訪問此服務以生成打印通道文檔。

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

### 建立WorkflowProcess實現

如下所示，workflowProcess實現代碼段。當工作流中的進程步驟與此AEM實現關聯時，將執行此代碼。 此實現要求3個進程參數，如下所述：

* 配置Adaptive Form時指定的DataFile路徑的名稱
* 打印渠道模板的名稱
* 生成的打印渠道文檔的名稱

第98行 — 由於自適應表單基於表單資料模型，因此提取駐留在afBoundData的資料節點中的資料。
行128 — 設定了「資料選項」服務名稱。 記下服務名稱。 它必須與上一個代碼清單的第45行中返回的名稱匹配。
行135 — 使用PrintChannel對象的呈現方法生成文檔


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

要在伺服器上test此功能，請執行以下步驟：

* [配置第CQ天郵件服務。](https://helpx.adobe.com/experience-manager/6-5/communities/using/email.html) 這是用作為附件生成的文檔發送電子郵件所必需的。
* [部署使用服務用戶包進行開發](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* 確保已在Apache Sling服務用戶映射器服務配置中添加以下條目
* **DevegingWithServiceUser.core:getformsresourceresolver=fd-service**
* [將與本文相關的資產下載並解壓縮到您的檔案系統](assets/prefillservice.zip)
* [使用包管理器導入以AEM下包](http://localhost:4502/crx/packmgr/index.jsp)
   1. beneficiaryconfirmationic.zip
   2. changeofbeneficiaryform.zip
   3. generatebeneficiaryworkflow.zip
* [使用Felix Web ConsoleAEM部署以下內容](http://localhost:4502/system/console/bundles)

   * GenerateIC.GenerateIC.core-1.0-SNAPSHOT.jar。 此捆綁包包含本文中提及的代碼。

* [開啟ChangeOfVientForm](http://localhost:4502/content/dam/formsanddocuments/changebeneficiary/jcr:content?wcmmode=disabled)
* 確保將自適應表單配置為提交到AEM工作流，如下所示
   ![影像](assets/generateic.PNG)
* [配置工作流模型。](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ChangesToBeneficiary.html)確保根據您的環境配置流程步驟和發送電子郵件元件
* [預覽ChangeOfVientForm。](http://localhost:4502/content/dam/formsanddocuments/changebeneficiary/jcr:content?wcmmode=disabled) 填寫一些詳細資訊並提交
* 工作流應被調用，IC打印通道文檔應作為附件發送到在發送電子郵件元件中指定的收件人
