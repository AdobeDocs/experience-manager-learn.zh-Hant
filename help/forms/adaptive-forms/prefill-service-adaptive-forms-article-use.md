---
title: 適用性Forms中的預填服務
seo-title: 適用性Forms中的預填服務
description: 從後端資料來源擷取資料，以預先填入最適化表單。
seo-description: 從後端資料來源擷取資料，以預先填入最適化表單。
sub-product: 表單
feature: 適用性表單
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 26a8cba3-7921-4cbb-a182-216064e98054
discoiquuid: 936ea5e9-f5f0-496a-9188-1a8ffd235ee5
topic: 開發
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 0%

---


# 在適用性Forms中使用預填服務

您可以使用現有資料預先填寫最適化表單的欄位。 使用者開啟表單時，會預填這些欄位的值。 有多種方式可預先填寫最適化表單欄位。 在本文中，我們將探討如何使用AEM Forms預填服務預填最適化表單。

若要進一步了解預先填入最適化表單的各種方法，請參閱本檔案](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)[

若要使用預填服務預填適用性表單，您必須建立實作DataProvider介面的類別。 getPrefillData方法將具有邏輯，可建置並傳回最適化表單用來預先填入欄位的資料。 在此方法中，您可以從任何來源擷取資料，並傳回資料檔案的輸入流。 以下示例代碼提取登錄用戶的用戶配置檔案資訊，並構建XML文檔，其輸入流被返回以被自適應表單使用。

在以下的程式碼片段中，我們提供實作DataProvider介面的類別。 我們可以存取登入的使用者，然後擷取登入的使用者設定檔資訊。 然後，我們使用名為「data」的根節點元素建立XML文檔，並將適當的元素附加到此資料節點。 一旦構建XML文檔，則返回XML文檔的輸入流。

然後，此類會製作成OSGi套件組合併部署至AEM。 部署套件組合後，此預填服務便可用作最適化表單的預填服務。

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

要在伺服器上測試此功能，請執行以下操作

* [將zip檔案的內容下載並解壓縮至您的電腦](assets/prefillservice.zip)
* 請確定已完全填寫[使用者的設定檔](http://localhost:4502/libs/granite/security/content/useradmin)資訊。 此為範例運作的必要項目。 範例沒有任何錯誤檢查遺失的使用者設定檔屬性。
* 使用[AEM Web控制台](http://localhost:4502/system/console/bundles)部署套件組合
* 使用XSD建立適用性表單
* 將「自訂Aem表單預填服務」與最適化表單的預填服務建立關聯
* 將結構元素拖放至表單
* 預覽表單

>[!NOTE]
>
>如果適用性表單以XSD為基礎，請確定預填服務傳回的XML檔案符合您適用性表單所依據的XSD。
>
>如果適用性表單不是以XSD為基礎，則您必須手動系結欄位。 例如，要將適用性表單欄位綁定到XML資料中的fname元素，您將在適用性表單欄位的「綁定引用」中使用`/data/fname`。

