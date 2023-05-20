---
title: 使用ACS配置檔案預填充自適應表單
description: 使用ACS配置檔案預填充自適應Forms
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 502f4bdf-d4af-409f-a611-62b7a1a6065a
source-git-commit: 38e0332ef2ef45a73a81f318975afc25600392a8
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 0%

---

# 使用ACS配置檔案預填充自適應表單 {#prefilling-adaptive-form-using-acs-profile}

在此部分，我們使用從ACS獲取的配置檔案資訊預填充自適應表單。 AEM Forms擁有預填充自適應表格的強大功能。

要瞭解有關預填自適應表單的詳細資訊，請閱讀以下內容 [教程](https://helpx.adobe.com/experience-manager/kt/forms/using/prefill-service-adaptive-forms-article-use.html)。

要通過從ACS獲取資料來預填充自適應表單，我們假定ACS中存在與登錄用戶具有相同電子郵件的配AEM置檔案。 例如，如果登錄人員的電子郵件ID是csimms@adobe.comAEM，則我們希望在電子郵件為csimms@adobe.com的ACS中找到一個配置檔案。

使用REST API從ACS獲取配置檔案資訊需要執行以下步驟

* 生成JWT
* Exchange JWT for Access Token（訪問令牌的Exchange JWT）
* 通過電子郵件呼叫ACS並獲取配置檔案
* 使用配置檔案資訊生成XML文檔
* 返回AEM Forms使用的XML文檔的InputStream

![預過濾服務](assets/prefillserviceaf.gif)

將預填充服務與自適應表單關聯

以下是從ACS獲取和返回配置檔案資訊的代碼。

在第68行中，我們獲取用戶的電子郵AEM件ID。 通過向Adobe Campaign Standard撥打REST呼叫獲取配置檔案詳細資訊。 從提取的配置檔案詳細資訊中，XML文檔以AEM Forms理解的方式構建。 此文檔的輸入流將返回供AEM Forms使用。

```java
package aemforms.campaign.core;

import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.InputStream;

import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;
import javax.xml.transform.TransformerConfigurationException;
import javax.xml.transform.TransformerException;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.TransformerFactoryConfigurationError;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;

import org.apache.http.HttpHost;
import org.apache.http.HttpResponse;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.util.EntityUtils;
import org.apache.jackrabbit.api.JackrabbitSession;
import org.apache.jackrabbit.api.security.user.Authorizable;
import org.apache.jackrabbit.api.security.user.UserManager;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;
import org.w3c.dom.Element;

import com.adobe.forms.common.service.DataXMLOptions;
import com.adobe.forms.common.service.DataXMLProvider;
import com.adobe.forms.common.service.FormsException;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

import formsandcampaign.demo.CampaignConfigurationService;

@Component
public class PrefillAdaptiveFormWithCampaignProfile implements DataXMLProvider {
private static final Logger log = LoggerFactory.getLogger(PrefillAdaptiveFormWithCampaignProfile.class);
private static final String SERVER_FQDN = "mc.adobe.io";
private static final String ENDPOINT = "/campaign/profileAndServices/profile/byEmail?email=";
@Reference
CampaignService jwtService;
@Reference
CampaignConfigurationService campaignConfig;

Session session = null;

public JsonObject getProfileDetails()
 {
    String jwtToken = null;
    String email = null;
    log.debug("$$$$ in getProfile Details");
    try
       {
          jwtToken = jwtService.getAccessToken();
          UserManager um = ((JackrabbitSession) session).getUserManager();
          Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
          email = loggedinUser.getProperty("profile/email")[0].getString();
          log.debug("####Got email..." + email);
        }
        catch (Exception e)
        {
          log.error("Unable to generate JWT!\n", e);
        }
    String tenant = campaignConfig.getTenant();
    String apikey = campaignConfig.getApiKey();
    String path = "/" + tenant + ENDPOINT + email;
    HttpHost server = new HttpHost(SERVER_FQDN, 443, "https");
    HttpGet getReq = new HttpGet(path);
    getReq.addHeader("Cache-Control", "no-cache");
    getReq.addHeader("Content-Type", "application/json");
    getReq.addHeader("X-Api-Key", apikey);
    getReq.addHeader("Authorization", "Bearer " + jwtToken);
    HttpClient httpClient = HttpClientBuilder.create().build();
    try
       {
          HttpResponse result = httpClient.execute(server, getReq);
          String responseJson = EntityUtils.toString(result.getEntity());
          log.debug("The response Json" + responseJson);
          JsonObject responseJsonProfiles = new JsonParser().parse(responseJson).getAsJsonObject();
          log.debug("The json array is " + responseJsonProfiles.toString());
          return responseJsonProfiles.get("content").getAsJsonArray().get(0).getAsJsonObject();
        }
        catch (ClientProtocolException e)
       {
            e.printStackTrace();
        } catch (IOException e) {

      e.printStackTrace();
}
    return null;

}

public InputStream getDataXMLForDataRef(DataXMLOptions dataXmlOptions) throws FormsException {
// TODO Auto-generated method stub
    log.debug("Geting xml");
    InputStream xmlDataStream = null;
    Resource aemFormContainer = dataXmlOptions.getFormResource();
    ResourceResolver resolver = aemFormContainer.getResourceResolver();
    session = resolver.adaptTo(Session.class);
    JsonObject profile = getProfileDetails();
    log.debug("####profile last name ####" + profile.get("lastName").getAsString());
    try {
          DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
          DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
          Document doc = docBuilder.newDocument();
          Element rootElement = doc.createElement("data");
          doc.appendChild(rootElement);
          Element firstNameElement = doc.createElement("fname");
          firstNameElement.setTextContent(profile.get("firstName").getAsString());
          log.debug("created firstNameElement  " + profile.get("firstName").getAsString());
          Element lastNameElement = doc.createElement("lname");
          Element jobTitleElement = doc.createElement("jobTitle");
          jobTitleElement.setTextContent(profile.get("salutation").getAsString());
          Element cityElement = doc.createElement("city");
          cityElement.setTextContent(profile.get("location").getAsJsonObject().get("city").getAsString());
          log.debug("created cityElement  " + profile.get("location").getAsJsonObject().get("city").getAsString());
          Element countryElement = doc.createElement("country");
          countryElement.setTextContent(profile.get("location").getAsJsonObject().get("countryCode").getAsString());
          Element streetElement = doc.createElement("street");
          streetElement.setTextContent(profile.get("location").getAsJsonObject().get("address1").getAsString());
          Element postalCodeElement = doc.createElement("postalCode");
          postalCodeElement.setTextContent(profile.get("location").getAsJsonObject().get("zipCode").getAsString());
          Element genderElement = doc.createElement("gender");
          genderElement.setTextContent(profile.get("gender").getAsString());
          lastNameElement.setTextContent(profile.get("lastName").getAsString());
          Element emailElement = doc.createElement("email");
          emailElement.setTextContent(profile.get("email").getAsString());
          rootElement.appendChild(firstNameElement);
          rootElement.appendChild(lastNameElement);
          rootElement.appendChild(emailElement);
          rootElement.appendChild(streetElement);
          rootElement.appendChild(countryElement);
          rootElement.appendChild(cityElement);
          rootElement.appendChild(jobTitleElement);
          rootElement.appendChild(postalCodeElement);
          rootElement.appendChild(genderElement);
          ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
          DOMSource source = new DOMSource(doc);
          StreamResult outputTarget = new StreamResult(outputStream);
          TransformerFactory.newInstance().newTransformer().transform(source, outputTarget);
          xmlDataStream = new ByteArrayInputStream(outputStream.toByteArray());
          return xmlDataStream;

}
    catch (ParserConfigurationException e)
    {
          e.printStackTrace();
    }catch (TransformerConfigurationException e)
    {
        e.printStackTrace();
    } 
    catch (TransformerException e)
   {
        e.printStackTrace();
  } catch (TransformerFactoryConfigurationError e) {
// TODO Auto-generated catch block
e.printStackTrace();
}

return null;
}

@Override
public String getServiceDescription() {

return "Custom Aem Form Pre Fill Service using campaign";
}

@Override
public String getServiceName() {
// TODO Auto-generated method stub
return "Pre Fill Forms Using Campaign Profile";
}

}
```

要使此功能在您的系統上正常工作，請按照以下說明操作：

* [確保您已按照此處所述的步驟操作](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [使用包管理器將示例自AEM適應表單導入](assets/pre-fill-af-from-campaign.zip)
* 確保您與電子郵件AEMID由Adobe Campaign的配置檔案共用的用戶登錄。 例如，如果用戶的電子郵件IDAEM為johndoe@adobe.com，則您需要在電子郵件為johndoe@adobe.com的ACS中擁有一個配置檔案。
* [預覽窗體](http://localhost:4502/content/dam/formsanddocuments/prefillfromcampaign/jcr:content?wcmmode=disabled)。

## 後續步驟

[使用表單資料模型建立Adobe Campaign配置檔案](./partfour.md)

