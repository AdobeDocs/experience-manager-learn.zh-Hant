---
title: 使用AEM Forms建立您的第一個OSGi服務
description: 使用AEM Forms建置您的第一個OSGi服務
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 2f15782e-b60d-40c6-b95b-6c7aa8290691
last-substantial-update: 2021-04-23T00:00:00Z
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 2%

---

# OSGi服務

OSGi服務是Java類或服務介面，以及許多作為名稱/值配對的服務屬性。 服務屬性區分提供具有相同服務介面的服務的不同服務提供商。

OSGi服務由其服務介面在語義上定義，並作為服務對象實現。 服務的功能由服務實施的介面定義。 因此，不同的應用程式可以實現相同的服務。 服務介面允許捆綁包通過綁定介面而非實施進行交互。 應指定服務介面，並盡可能減少實作詳細資料。

## 定義介面

簡單的介面，可透過一種方法合併資料與 <span class="x x-first x-last">XDP</span> 範本。

```java
package com.mysite.samples;

import com.adobe.aemfd.docmanager.Document;

public interface MyfirstInterface
{
    public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument);
}
 
```

## 實作介面

建立新套件，稱為 `com.mysite.samples.impl` 以保留介面的實作。

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

註解 `@Component(...)` 第10行會將此Java類標籤為OSGi元件，並將其註冊為OSGi服務。

此 `@Reference` 注釋是OSGi聲明服務的一部分，用於插入 [Outputservice](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) 變數中 `outputService`.


## 建立和部署套件

* 開啟 **命令提示窗口**
* 瀏覽到 `c:\aemformsbundles\mysite\core`
* 執行命令 `mvn clean install -PautoInstallBundle`
* 上述命令會自動建立套件組合併部署至localhost:4502上執行的AEM執行個體

此套件也可在下列位置使用 `C:\AEMFormsBundles\mysite\core\target`. 套件組合也可透過 [Felix Web Console。](http://localhost:4502/system/console/bundles)

## 使用服務

您現在可以在JSP頁中使用該服務。 下列程式碼片段顯示如何存取您的服務，以及如何使用服務實作的方法

```java
MyFirstAEMFormsService myFirstAEMFormsService = sling.getService(com.mysite.samples.MyFirstAEMFormsService.class);
com.adobe.aemfd.docmanager.Document generatedDocument = myFirstAEMFormsService.mergeDataWithXDPTemplate(xdp_or_pdf_template,xmlDocument);
```

包含JSP頁的示例包可以是 [從此處下載](assets/learning_aem_forms.zip)

[完整套件可供下載](assets/mysite.core-1.0.0-SNAPSHOT.jar)

## 測試套件

使用將套件匯入並安裝至AEM [封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)

使用postman進行POST呼叫，並提供輸入參數，如下方螢幕擷取畫面所示
![postman](assets/test-service-postman.JPG)

## 後續步驟

[建立Sling Servlet](./create-servlet.md)

