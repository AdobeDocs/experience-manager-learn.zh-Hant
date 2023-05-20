---
title: 自適應Forms中的預填充服務
description: 通過從後端資料源讀取資料預填充自適應表單。
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

# 在自適應Forms中使用Prefill服務

可以使用現有資料預填充自適應表單的欄位。 當用戶開啟表單時，這些欄位的值將預先填充。 預填充自適應表單域的方法有多種。 本文將研究利用AEM Forms預填服務進行自適應表格預填。

要瞭解有關預填充自適應表單的各種方法的詳細資訊， [請關注本文檔](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)

要使用預填充服務預填充自適應表單，必須建立實現 `com.adobe.forms.common.service.DataXMLProvider` 。 方法 `getDataXMLForDataRef` 將具有生成和返回資料的邏輯，自適應表單將使用這些資料來預填充欄位。 在此方法中，可以從任何源獲取資料並返回資料文檔的輸入流。 下面的示例代碼提取登錄用戶的用戶配置檔案資訊並構建其輸入流被自適應表單使用的XML文檔。

在下面的代碼段中，我們有一個實現DataXMLProvider介面的類。 我們可以訪問已登錄用戶，然後獲取已登錄用戶的配置檔案資訊。 然後，我們使用名為「data」的根節點元素建立XML文檔，並將相應的元素添加到此資料節點。 一旦構建XML文檔，則返回XML文檔的輸入流。

然後，將此類構建為OSGi捆綁包並部署到AEM中。 部署包後，此預填充服務便可用作自適應表單的預填充服務。

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

要在伺服器上test此功能，請執行以下步驟

* 確保已登錄 [用戶配置檔案](http://localhost:4502/security/users.html) 資訊已填寫。 此示例將查找已登錄用戶的FirstName、LastName和Email屬性。
* [下載並解壓電腦上的zip檔案內容](assets/prefillservice.zip)
* 使用 [AEM Web控制台](http://localhost:4502/system/console/bundles)
* 使用「建立」(Create)導入自適應窗體 |檔案從 [FormsAndDocuments部分](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 確保 [表格](http://localhost:4502/editor.html/content/forms/af/prefill.html) 正在使用 **&quot;自定義AEM Forms預填充服務&quot;** 作為預填服務。 這可以從的配置屬性中驗證 **窗體容器** 的子菜單。
* [預覽窗體](http://localhost:4502/content/dam/formsanddocuments/prefill/jcr:content?wcmmode=disabled)。 您應看到填充了正確值的表單。

>[!NOTE]
>
>如果已為com.aem.prefill.core.PrefillAdaptiveForm啟用調試，則生成的xml資料檔案將寫入伺服器安AEM裝資料夾。

