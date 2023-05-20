---
title: 與AEM Forms建立您的第一個OSGi服務
description: 與AEM Forms建立第一項OSGi服務
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

OSGi服務是Java類或服務介面，以及作為名稱/值對的許多服務屬性。 所述服務屬性區別於提供具有相同服務介面的服務的不同服務提供商。

OSGi服務由其服務介面語義定義並作為服務對象實現。 服務的功能由服務實現的介面定義。 因此，不同的應用可以實現相同的服務。 服務介面允許捆綁介面（而非實現）進行交互。 應指定服務介面，並盡可能少地提供實現詳細資訊。

## 定義介面

一種簡單的介面，可將資料與 <span class="x x-first x-last">XDP</span> 的下界。

```java
package com.mysite.samples;

import com.adobe.aemfd.docmanager.Document;

public interface MyfirstInterface
{
    public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument);
}
 
```

## 實現介面

建立名為 `com.mysite.samples.impl` 以保存介面的實現。

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

注釋 `@Component(...)` 聯機10將此Java類標籤為OSGi元件，並將其註冊為OSGi服務。

的 `@Reference` 注釋是OSGi聲明性服務的一部分，並用於注入 [輸出服務](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) 變數 `outputService`。


## 構建和部署捆綁包

* 開啟 **命令提示符窗口**
* 瀏覽到 `c:\aemformsbundles\mysite\core`
* 執行命令 `mvn clean install -PautoInstallBundle`
* 以上命令將自動生成捆綁包並將其部署到在localhost:4502AEM上運行的實例

此捆綁包也將位於以下位置 `C:\AEMFormsBundles\mysite\core\target`。 此捆綁包也可以部署AEM到 [Felix網路控制台。](http://localhost:4502/system/console/bundles)

## 使用服務

現在，您可以在JSP頁中使用該服務。 以下代碼段顯示如何獲取對服務的訪問權限和使用由服務實現的方法

```java
MyFirstAEMFormsService myFirstAEMFormsService = sling.getService(com.mysite.samples.MyFirstAEMFormsService.class);
com.adobe.aemfd.docmanager.Document generatedDocument = myFirstAEMFormsService.mergeDataWithXDPTemplate(xdp_or_pdf_template,xmlDocument);
```

包含JSP頁的示例包可以是 [從此處下載](assets/learning_aem_forms.zip)

[完整的捆綁包可供下載](assets/mysite.core-1.0.0-SNAPSHOT.jar)

## Test包

使用 [軟體包管理器](http://localhost:4502/crx/packmgr/index.jsp)

使用郵遞員進行POST調用並提供如下螢幕抓圖所示的輸入參數
![郵差](assets/test-service-postman.JPG)

## 後續步驟

[建立Sling Servlet](./create-servlet.md)

