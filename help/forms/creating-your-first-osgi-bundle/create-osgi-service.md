---
title: 使用AEM Forms建立您的第一個OSGi服務
description: 使用AEM Forms建立您的第一個OSGi服務
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 2f15782e-b60d-40c6-b95b-6c7aa8290691
last-substantial-update: 2021-04-23T00:00:00Z
duration: 87
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 0%

---

# OSGi服務

OSGi服務是Java類別或服務介面，以及許多作為名稱/值配對的服務屬性。 服務屬性會區分使用相同服務介面提供服務的不同服務提供者。

OSGi服務由其服務介面語義定義，並實作為服務物件。 服務的功能由其實作的介面定義。 因此，不同的應用程式可以實作相同的服務。 服務介面允許套件組合透過繫結介面進行互動，而不是實作。 指定的服務介面應儘可能包含較少的實作詳細資料。

## 定義介面

一個簡單介面，具有一個與<span class="x x-first x-last">XDP</span>範本合併資料的方法。

```java
package com.mysite.samples;

import com.adobe.aemfd.docmanager.Document;

public interface MyfirstInterface
{
    public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument);
}
 
```

## 實作介面

建立名為`com.mysite.samples.impl`的新封裝以儲存介面的實作。

```java
package com.mysite.samples.impl;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.output.api.OutputService;
import com.adobe.fd.output.api.OutputServiceException;
import com.mysite.samples.MyfirstInterface;
@Component(service = MyfirstInterface.class)
public class MyfirstInterfaceImpl implements MyfirstInterface {
  @Reference
  OutputService outputService;

  private static final Logger log = LoggerFactory.getLogger(MyfirstInterfaceImpl.class);

  @Override
  public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument) {
    com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
    pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
    try {
      return outputService.generatePDFOutput(xdpTemplate, xmlDocument, pdfOptions);

    } catch (OutputServiceException e) {

      log.error("Failed to merge data with XDP Template", e);

    }

    return null;
  }

}
```

第10行的註解`@Component(...)`會將此Java類別標籤為OSGi元件，並將其註冊為OSGi服務。

`@Reference`註解是OSGi宣告式服務的一部分，用來將[Outputservice](https://helpx.adobe.com/tw/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html)的參照插入變數`outputService`。


## 建置和部署套件組合

* 開啟&#x200B;**命令提示字元視窗**
* 瀏覽至`c:\aemformsbundles\mysite\core`
* 執行命令`mvn clean install -PautoInstallBundle`
* 上述命令會自動建置套件，並將其部署至localhost：4502上執行的AEM執行個體

此組合也可在下列位置`C:\AEMFormsBundles\mysite\core\target`使用。 也可以使用[Felix Web主控台](http://localhost:4502/system/console/bundles)將套件組合部署至AEM。

## 使用服務

您現在可以在JSP頁面中使用服務。 下列程式碼片段說明如何存取服務並使用服務實作的方法

```java
MyFirstAEMFormsService myFirstAEMFormsService = sling.getService(com.mysite.samples.MyFirstAEMFormsService.class);
com.adobe.aemfd.docmanager.Document generatedDocument = myFirstAEMFormsService.mergeDataWithXDPTemplate(xdp_or_pdf_template,xmlDocument);
```

包含JSP頁面的範例套件可從這裡[下載](assets/learning_aem_forms.zip)

[完整的套裝軟體可供下載](assets/mysite.core-1.0.0-SNAPSHOT.jar)

## 測試封裝

使用[封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)將封裝匯入並安裝到AEM中

使用Postman進行POST呼叫並提供輸入引數，如下方熒幕擷取畫面所示
![郵遞員](assets/test-service-postman.JPG)

## 後續步驟

[建立Sling Servlet](./create-servlet.md)

