---
title: 在帶監視資料夾的輸出服務中使用片段
description: 生成包含駐留在crx儲存庫中的片段的PDF文檔
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2022-09-07T00:00:00Z
thumbnail: ecma-fragments.jpg
exl-id: 6b0bd2f1-b8ee-4f96-9813-8c11aedd3621
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---

# 使用ECMA指令碼生成帶片段的pdf文檔{#developing-with-output-and-forms-services-in-aem-forms}


在本文中，我們將使用輸出服務使用xdp片段生成pdf檔案。 主xdp和片段駐留在crx儲存庫中。 在中模擬檔案系統資料夾結構非常重要AEM。 例如，如果在xdp的片段資料夾中使用片段，則必須建立一個名為 **碎片** 下AEM。 基本資料夾將包含基xdp模板。 例如，如果檔案系統上具有以下結構
* c:\xdptemplates - This will contain your base xdp template
* c:\xdptemplates\fragments - This folder will contain fragments and the main template will reference the fragment as shown below
   ![片段 — xdp](assets/survey-fragment.png)。
* 資料夾xdpdocuments將包含您的基本模板和 **碎片** 資料夾

可以使用 [窗體和文檔ui](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

下面是使用2個片段的示例xdp的資料夾結構
![窗體&amp;文檔](assets/fragment-folder-structure-ui.png)


* 輸出服務 — 通常，此服務用於將xml資料與xdp模板或pdf合併，以生成拼合的pdf。 有關詳細資訊，請參閱 [javadox](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) 的子菜單。 在此示例中，我們使用駐留在crx儲存庫中的片段。


以下ECMA指令碼已使用生成PDF。 請注意代碼中ResourceResolver和ResourceResolverHelper的使用。 需要ResourceReolver，因為此代碼在任何用戶上下文之外運行。

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

**test系統上的示例包**
* [部署DevelopingWithServiceUSer捆綁包](assets/DevelopingWithServiceUser.jar)
* 添加條目 **DevegingWithServiceUser.core:getformsresourceresolver=fd-service** 如下螢幕快照所示，在用戶映射器服務修正中
   ![用戶映射器修正](assets/user-mapper-service-amendment.png)
* [下載並導入示例xdp檔案和ECMA指令碼](assets/watched-folder-fragments-ecma.zip)。
這將在c:/fragments和outputservice資料夾中建立受監視的資料夾結構

* [提取示例資料檔案](assets/usingFragmentsSampleData.zip) 並將其放在監視資料夾的安裝資料夾中(c:\fragmentsandoutputservice\install)

* 檢查監視資料夾配置的結果資料夾以查找生成的pdf檔案
