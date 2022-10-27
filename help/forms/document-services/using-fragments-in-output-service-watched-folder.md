---
title: 在輸出服務中使用片段
description: 產生PDF檔案，其中片段位於crx存放庫中
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2022-09-07T00:00:00Z
thumbnail: ecma-fragments.jpg
source-git-commit: e1c16ff347f5f398c7bc47233049427eeffa2aab
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 0%

---

# 使用ECMA指令碼產生含有片段的PDF檔案{#developing-with-output-and-forms-services-in-aem-forms}


在本文中，我們將使用輸出服務，使用xdp片段來產生pdf檔案。 主要xdp和片段位於crx存放庫。 務必在AEM中模擬檔案系統資料夾結構。 例如，如果您在xdp的片段資料夾中使用片段，則必須建立名為 **片段** 在AEM的基礎資料夾下。 基本資料夾將包含基本xdp範本。 例如，如果檔案系統上有下列結構
* c:\xdptemplates - This will contain your base xdp template
* c:\xdptemplates\fragments - This folder will contain fragments and the main template will reference the fragment as shown below
   ![fragment-xdp](assets/survey-fragment.png).
* 資料夾xdpdocuments將包含您的基本範本，以及 **片段** 資料夾

您可以使用 [表單和檔案ui](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

以下是範例xdp的資料夾結構，其使用2個片段
![表單&amp;文檔](assets/fragment-folder-structure-ui.png)


* 輸出服務 — 通常，此服務用於將xml資料與xdp範本合併，或以pdf產生平面化的pdf。 如需更多詳細資訊，請參閱 [javado](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) （輸出服務）。 在此範例中，我們使用的片段位於crx存放庫中。


以下ECMA指令碼用於生成PDF。 請注意在程式碼中使用ResourceResolver和ResourceResolverHelper。 此代碼在任何用戶上下文之外運行，因此需要ResourceReolver。

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

**在系統上測試示例包**
* [部署DevelopingWithServiceUSer捆綁包](assets/DevelopingWithServiceUser.jar)
* 新增項目 **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** （如下方螢幕擷取所示）
   ![用戶映射器修正](assets/user-mapper-service-amendment.png)
* [下載並匯入範例xdp檔案和ECMA指令碼](assets/watched-folder-fragments-ecma.zip).
這會在您的c:/fragments和outputservice資料夾中建立已監看的資料夾結構

* [擷取範例資料檔案](assets/usingFragmentsSampleData.zip) 並將其放置在已觀看資料夾的安裝資料夾(c:\fragmentsandoutputservice\install)

* 檢查監看資料夾配置的結果資料夾，以查看生成的pdf檔案
