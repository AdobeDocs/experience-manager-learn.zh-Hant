---
title: 適用性Forms中的預填服務
description: 從後端資料來源擷取資料，以預先填入最適化表單。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f2c324a3-cbfa-4942-b3bd-dc47d8a3f7b5
last-substantial-update: 2021-11-27T00:00:00Z
source-git-commit: 381812397fa7d15f6ee34ef85ddf0aa0acc0af42
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 0%

---

# 在適用性Forms中使用預填服務

您可以使用現有資料預先填寫最適化表單的欄位。 使用者開啟表單時，會預填這些欄位的值。 有多種方式可預先填寫最適化表單欄位。 在本文中，我們將探討如何使用AEM Forms預填服務預填最適化表單。

若要進一步了解預先填入最適化表單的各種方法， [請參閱本檔案](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)

若要使用預填服務預填適用性表單，您必須建立實作的類別 `com.adobe.forms.common.service.DataXMLProvider` 介面。 方法 `getDataXMLForDataRef` 將具有邏輯，可建置並傳回最適化表單將使用的資料，以預先填入欄位。 在此方法中，您可以從任何來源擷取資料，並傳回資料檔案的輸入流。 以下示例代碼提取登錄用戶的用戶配置檔案資訊，並構建XML文檔，其輸入流被返回以被自適應表單使用。

在下面的代碼段中，我們有一個實現DataXMLProvider介面的類。 我們可以存取登入的使用者，然後擷取登入的使用者設定檔資訊。 然後，我們使用名為「data」的根節點元素建立XML文檔，並將適當的元素附加到此資料節點。 一旦構建XML文檔，則返回XML文檔的輸入流。

然後，此類會製作成OSGi套件組合併部署至AEM。 部署套件組合後，此預填服務便可用作最適化表單的預填服務。

```java
package com.aem.prefill.core;
import com.adobe.forms.common.service.DataXMLOptions;
import com.adobe.forms.common.service.DataXMLProvider;
import com.adobe.forms.common.service.FormsException;
import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.FileOutputStream;
import java.io.InputStream;
import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import org.apache.jackrabbit.api.JackrabbitSession;
import org.apache.jackrabbit.api.security.user.Authorizable;
import org.apache.jackrabbit.api.security.user.UserManager;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;
import org.w3c.dom.Element;

@Component
public class PrefillAdaptiveForm implements DataXMLProvider {
  private static final Logger log = LoggerFactory.getLogger(PrefillAdaptiveForm.class);

  @Override
  public String getServiceDescription() {

    return "Custom AEM Forms PreFill Service";
  }

  @Override
  public String getServiceName() {

    return "CustomAemFormsPrefillService";
  }

  @Override
  public InputStream getDataXMLForDataRef(DataXMLOptions dataXmlOptions) throws FormsException {
    InputStream xmlDataStream = null;
    Resource aemFormContainer = dataXmlOptions.getFormResource();
    ResourceResolver resolver = aemFormContainer.getResourceResolver();
    Session session = (Session) resolver.adaptTo(Session.class);
    try {
      UserManager um = ((JackrabbitSession) session).getUserManager();
      Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
      log.debug("The path of the user is" + loggedinUser.getPath());
      DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
      DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
      Document doc = docBuilder.newDocument();
      Element rootElement = doc.createElement("data");
      doc.appendChild(rootElement);

      if (loggedinUser.hasProperty("profile/givenName")) {
        Element firstNameElement = doc.createElement("fname");
        firstNameElement.setTextContent(loggedinUser.getProperty("profile/givenName")[0].getString());
        rootElement.appendChild(firstNameElement);
        log.debug("Created firstName Element");
      }

      if (loggedinUser.hasProperty("profile/familyName")) {
        Element lastNameElement = doc.createElement("lname");
        lastNameElement.setTextContent(loggedinUser.getProperty("profile/familyName")[0].getString());
        rootElement.appendChild(lastNameElement);
        log.debug("Created lastName Element");

      }
      if (loggedinUser.hasProperty("profile/email")) {
        Element emailElement = doc.createElement("email");
        emailElement.setTextContent(loggedinUser.getProperty("profile/email")[0].getString());
        rootElement.appendChild(emailElement);
        log.debug("Created email Element");

      }

      TransformerFactory transformerFactory = TransformerFactory.newInstance();
      ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
      Transformer transformer = transformerFactory.newTransformer();
      DOMSource source = new DOMSource(doc);
      StreamResult outputTarget = new StreamResult(outputStream);
      TransformerFactory.newInstance().newTransformer().transform(source, outputTarget);
      if (log.isDebugEnabled()) {
        FileOutputStream output = new FileOutputStream("afdata.xml");
        StreamResult result = new StreamResult(output);
        transformer.transform(source, result);
      }

      xmlDataStream = new ByteArrayInputStream(outputStream.toByteArray());
      return xmlDataStream;
    } catch (Exception e) {
      log.error("The error message is " + e.getMessage());
    }
    return null;

  }

}
```

要在伺服器上測試此功能，請執行以下操作

* 確認已登入 [使用者設定檔](http://localhost:4502/security/users.html) 資訊已填寫。 範例會尋找登入使用者的FirstName、LastName和Email屬性。
* [將zip檔案的內容下載並解壓縮至您的電腦](assets/prefillservice.zip)
* 使用部署prefill.core-1.0.0-SNAPSHOT套件組合 [AEM web console](http://localhost:4502/system/console/bundles)
* 使用「建立」匯入最適化表單 |從 [FormsAndDocuments區段](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 請確定 [表單](http://localhost:4502/editor.html/content/forms/af/prefill.html) 正在使用 **「自訂AEM Forms PreFill服務」** 作為預填服務。 這可透過 **表單容器** 區段。
* [預覽表單](http://localhost:4502/content/dam/formsanddocuments/prefill/jcr:content?wcmmode=disabled). 您應該會看到表單中填入正確值。

>[!NOTE]
>
>如果您已啟用com.aem.prefill.core.PrefillAdaptiveForm的除錯功能，則產生的xml資料檔案會寫入AEM伺服器安裝資料夾。

