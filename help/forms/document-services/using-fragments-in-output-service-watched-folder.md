---
title: 搭配watched資料夾在輸出服務中使用片段
description: 產生PDF檔案，其片段位於crx存放庫中
feature: Output Service
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2022-09-07T00:00:00Z
thumbnail: ecma-fragments.jpg
exl-id: 6b0bd2f1-b8ee-4f96-9813-8c11aedd3621
duration: 84
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '347'
ht-degree: 0%

---

# 使用ECMA指令碼產生含片段的pdf檔案{#developing-with-output-and-forms-services-in-aem-forms}


在本文中，我們將使用輸出服務產生使用xdp片段的pdf檔案。 主要xdp和片段位於crx存放庫中。 在AEM中模擬檔案系統資料夾結構是很重要的事。 例如，如果您在xdp的片段資料夾中使用片段，您必須在AEM中的基底資料夾下建立名為&#x200B;**fragments**&#x200B;的資料夾。 基底資料夾將包含您的基底xdp範本。 例如，如果您的檔案系統有下列結構
* c：\xdptemplates — 這將包含您的基本xdp範本
* c：\xdptemplates\fragments — 此資料夾將包含片段，主要範本將參考如下所示的片段
  ![片段 — xdp](assets/survey-fragment.png)。
* 資料夾xdpdocuments將包含您的基礎範本和&#x200B;**片段**&#x200B;資料夾中的片段

您可以使用[表單和檔案ui](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)來建立必要的結構

以下是使用2個片段的範例xdp的資料夾結構
![表單檔案](assets/fragment-folder-structure-ui.png)


* 輸出服務 — 通常此服務用於合併xml資料與xdp範本或pdf以產生平面化pdf。 如需詳細資訊，請參閱輸出服務的[javadoc](https://helpx.adobe.com/tw/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html)。 在此範例中，我們使用crx存放庫中的片段。


下列ECMA指令碼已用於產生PDF。 請注意程式碼中使用ResourceResolver和ResourceResolverHelper。 需要ResourceReolver，因為此程式碼在任何使用者內容之外執行。

```java
var inputMap = processorContext.getInputMap();
var itr = inputMap.entrySet().iterator();
var entry = inputMap.entrySet().iterator().next();
var xmlData = inputMap.get(entry.getKey());
log.info("Got XML Data File");

var resourceResolverHelper = sling.getService(Packages.com.adobe.granite.resourceresolverhelper.ResourceResolverHelper);
var aemDemoListings = sling.getService(Packages.com.mergeandfuse.getserviceuserresolver.GetResolver);
log.info("Got service resolver");
var resourceResolver = aemDemoListings.getFormsServiceResolver();
//The ResourceResolverHelper execute's the following code within the context of the resourceResolver 
resourceResolverHelper.callWith(resourceResolver, {call: function()
       {
             //var statement = new Packages.com.adobe.aemfd.docmanager.Document("/content/dam/formsanddocuments/xdpdocuments/main.xdp",resourceResolver);
               var outputService = sling.getService(Packages.com.adobe.fd.output.api.OutputService);
            var pdfOutputOptions = new Packages.com.adobe.fd.output.api.PDFOutputOptions();
            pdfOutputOptions.setContentRoot("crx:///content/dam/formsanddocuments/xdpdocuments");
            pdfOutputOptions.setAcrobatVersion(Packages.com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
            var dataMergedDocument = outputService.generatePDFOutput("main.xdp",xmlData,pdfOutputOptions);
               //var dataMergedDocument = outputService.generatePDFOutput(statement,xmlData,pdfOutputOptions);
            processorContext.setResult("mergeddocument.pdf",dataMergedDocument);
            log.info("Generated the pdf document with fragments");
      }

 });
```

**若要在您的系統上測試範例封裝**
* [部署DevelopingWithServiceUSer套件](assets/DevelopingWithServiceUser.jar)
* 在使用者對應程式服務修正中新增專案&#x200B;**DevelopingWithServiceUser.core：getformsresourceresolver=fd-service**，如下方熒幕擷取畫面所示
  ![使用者對應程式修正](assets/user-mapper-service-amendment.png)
* [下載並匯入範例xdp檔案和ECMA指令碼](assets/watched-folder-fragments-ecma.zip)。
這會在您的c：/fragmentsandoutputservice資料夾中建立watched資料夾結構

* [擷取範例資料檔](assets/usingFragmentsSampleData.zip)，並將其置於watched資料夾的安裝資料夾中(c：\fragmentsandoutputservice\install)

* 檢查watched資料夾組態的結果資料夾中是否有產生的pdf檔案
