---
title: 實用服務
description: AEM Forms開發人員的一些實用程式服務
feature: 適用性表單
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 開發
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 2%

---


# 實用服務

此範例套件組合提供實用程式服務，供AEM Forms開發人員使用。 提供下列服務。


```java
package aemformsutilityfunctions.core;
import java.util.Map;
import com.adobe.aemfd.docmanager.Document;
public interface AemFormsUtilities
{
public abstract com.adobe.aemfd.docmanager.Document createDDXFromMapOfDocuments(Map<String, com.adobe.aemfd.docmanager.Document> paramMap);
public abstract org.w3c.dom.Document w3cDocumentFromStrng(String xmlString);
public abstract com.adobe.aemfd.docmanager.Document orgw3cDocumentToAEMFDDocument(org.w3c.dom.Document xmlDocument);
public abstract String saveDocumentInCrx(String jcrPath,String fileExtension, Document documentToSave);

}
```

範例套件組合可從此處[下載](assets/aemformsutilityfunctions.aemformsutilityfunctions.core-1.0-SNAPSHOT.jar)

## 使用公用程式服務的范常式式碼

以下是JSP頁面中用來從字串建立org.w3c.dom.Document的程式碼，並轉換檔案並將其儲存在CRX存放庫中，如下列程式碼片段所示。

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## 必備條件


您需要部署[DevelopingWithServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar)並啟動套件。


如果您打算使用這些公用程式服務將檔案儲存在CRX存放庫中，請依照[與服務使用者一文開發](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en#adaptive-forms)操作。 請務必為fd-service使用者提供適當CRX資料夾的[必要權限](http://localhost:4502/useradmin)。

