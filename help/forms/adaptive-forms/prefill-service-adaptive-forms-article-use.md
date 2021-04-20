---
title: 最適化Forms的預填充服務
seo-title: 最適化Forms的預填充服務
description: 從後端資料來源擷取資料，以預先填入可調式表單。
seo-description: 從後端資料來源擷取資料，以預先填入可調式表單。
sub-product: 表單
feature: Adaptive Forms
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 26a8cba3-7921-4cbb-a182-216064e98054
discoiquuid: 936ea5e9-f5f0-496a-9188-1a8ffd235ee5
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '483'
ht-degree: 0%

---


# 在最適化Forms中使用預填充服務

您可以使用現有資料預先填寫最適化表單的欄位。 當使用者開啟表格時，這些欄位的值會預先填入。 有多種方式可預先填寫最適化表單欄位。 在本文中，我們將探討使用AEM Forms預填充服務來預填充自適應表單。

若要進一步瞭解預先填入最適化表單的各種方法，請遵循本檔案](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)[

若要使用預填服務預填最適化表單，您必須建立實作DataProvider介面的類別。 getPrefillData方法將具有邏輯來建立並傳回最適化表單會用來預先填入欄位的資料。 在此方法中，您可以從任何來源擷取資料並傳回資料檔案的輸入串流。 下列示例代碼讀取登錄用戶的用戶簡檔資訊，並構建XML文檔，其輸入流被返回為被自適應表單所消耗。

在下面的程式碼片段中，我們有一個類別實作DataProvider介面。 我們可存取已登入的使用者，然後擷取已登入使用者的設定檔資訊。 然後，我們使用名為&quot;data&quot;的根節點元素來建立XML檔案，並將適當的元素附加至此資料節點。 一旦構建XML文檔，就會返回XML文檔的輸入流。

然後，將此類製成OSGi捆綁包並部署到中AEM。 在部署包後，此預填充服務便可用作最適化表單的預填充服務。

```java
public class PrefillAdaptiveForm implements DataProvider {
 private Logger logger = LoggerFactory.getLogger(PrefillAdaptiveForm.class);

 public String getServiceName() {
  return "Default Prefill Service";
 }
 
 public String getServiceDescription() {
  return "This is default prefill service to prefill adaptive form with user data";
 }
 
 public PrefillData getPrefillData(final DataOptions dataOptions) throws FormsException {
  PrefillData prefillData = new PrefillData() {
   public InputStream getInputStream() {
    return getData(dataOptions);
   }
   
   public ContentType getContentType() {
    return ContentType.XML;
   }
  };
  return prefillData;
 }

 private InputStream getData(DataOptions dataOptions) throws FormsException {  
  try {
   Resource aemFormContainer = dataOptions.getFormResource();
   ResourceResolver resolver = aemFormContainer.getResourceResolver();
   Session session = resolver.adaptTo(Session.class);
   UserManager um = ((JackrabbitSession) session).getUserManager();
   Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
   DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
   DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
   Document doc = docBuilder.newDocument();
   Element rootElement = doc.createElement("data");
   doc.appendChild(rootElement);
   Element firstNameElement = doc.createElement("fname");
   firstNameElement.setTextContent(loggedinUser.getProperty("profile/givenName")[0].getString());
     .
     .
     .
   InputStream inputStream = new ByteArrayInputStream(rootElement.getTextContent().getBytes());
   return inputStream;
  } catch (Exception e) {
   logger.error("Error while creating prefill data", e);
   throw new FormsException(e);
  }
 }
}
```

若要在您的伺服器上測試此功能，請執行下列動作

* [下載並解壓郵遞區號檔的內容至您的電腦](assets/prefillservice.zip)
* 請確定登入[使用者的設定檔](http://localhost:4502/libs/granite/security/content/useradmin)資訊已完全填滿。 這是樣本運作所必需的。 範例不會檢查遺失的使用者描述檔屬性。
* 使用[Web控制台AEM](http://localhost:4502/system/console/bundles)部署包
* 使用XSD建立最適化表單
* 將「自訂Aem表單預填服務」關聯為最適化表單的預填服務
* 將架構元素拖放到表單
* 預覽表格

>[!NOTE]
>
>如果最適化表單是以XSD為基礎，請確定預填服務傳回的XML檔案符合您最適化表單所依據的XSD。
>
>如果最適化表單不以XSD為基礎，則您必須手動系結欄位。 例如，要將自適應表單域綁定到XML資料中的fname元素，您將在自適應表單域的「綁定」引用中使用`/data/fname`。

