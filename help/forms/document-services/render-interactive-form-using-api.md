---
title: 在AEM Forms使用Forms服務提供互動式PDF
description: 使用AEM Forms的Forms服務API呈現互動式PDF
feature: Forms Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9b2ef4c9-8360-480d-9165-f56a959635fb
source-git-commit: c462d48d26c9a7aa0e4cfc4f24005b41e8e82cb8
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---

# 在AEM Forms使用Forms服務提供互動式PDF

使用AEM Forms的Forms服務API呈現互動式PDF

在本文中，我們將看一下以下服務

* FormsService — 這是一項功能非常廣泛的服務，它允許您從PDF檔案導出/導入資料，並通過將xml資料合併到xdp模板中生成互動式pdf

官員 [此處列出了用於AEM FormsAPI的javadoc](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

以下代碼段使用FormsService的renderPDFForm操作呈現互動式pdf。 schengen.xdp是用於合併xml資料的模板。

```java
String uri = "crx:///content/dam/formsanddocuments";
PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
renderOptions.setContentRoot(uri);
Document interactivePDF = null;
try {
interactivePDF = formsService.renderPDFForm("schengen.xdp", xmlData, renderOptions);
} catch (FormsServiceException e) {
 e.printStackTrace();
}
return interactivePDF;
```

第1行：包含xdp模板的資料夾的位置

第2-4行：建立PDFFormRenderOptions並設定其屬性

第7行：使用FormsService的renderPDFForm服務操作生成交互PDF

第11行：將生成的互動式PDF返回給調用應用程式

**test系統上的示例包**
1. [下載並安裝DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [使用Felix Web控制台下載並安裝DocumentServices示例包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [使用包管理器下載並安裝AEM包](assets/downloadinteractivepdffrommobileform.zip)

1. [登錄到configMgr](http://localhost:4502/system/console/configMgr)
1. Adobe花崗岩CSRF濾波器的研究
1. 在排除的節中添加以下路徑並保存
1. /bin/generateinteractivepdf
1. 搜索 _Apache Sling服務用戶映射器服務_ 並按一下以開啟屬性
   1. 按一下 *+* 表徵圖（加號）以添加以下服務映射
      * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
   1. 按一下「保存」
1. [開啟移動窗體](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. 填入幾個欄位，然後按一下 ***下載並填充……。*** 按鈕
1. 應將互動式pdf下載到您的本地系統


示例包包含與移動表單關聯的自定義配置檔案。 請瀏覽 [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp) 的子菜單。 此jsp從移動表單中提取資料，並向裝載在上的Servlet發出POST請求 ***/bin/generateinteractivepdf*** 路徑。 Servlet將互動式PDF返回給調用應用程式。 customtoolbar.jsp中的代碼，然後將檔案下載到本地系統
