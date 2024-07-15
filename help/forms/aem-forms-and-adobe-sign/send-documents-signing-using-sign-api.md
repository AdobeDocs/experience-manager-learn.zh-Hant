---
title: 在AEM Forms中使用Adobe Sign API
description: 使用Adobe Sign協助程式方法傳送檔案以供簽署
feature: Adaptive Forms
jira: KT-15474
topic: Development
role: Developer
level: Beginner
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 3400b728-58ca-44c3-a882-e3170755f845
duration: 74
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 0%

---

# 使用Adobe Sign協助程式方法

在某些使用案例中，您可能需要在不使用AEM工作流程的情況下傳送檔案以索取簽名。 在這種情況下，使用本文提供的範例套件所公開的包裝函式方法會非常方便。

## 部署範例OSGi套件

[透過AEM OSGi Web主控台部署OSGi套件](assets/AdobeSignHelperMethods.core-1.0.0-SNAPSHOT.jar)。 透過AEM OSGi Web主控台的Configuration Manager，使用如下所示的OSGi設定，指定API整合金鑰和API使用者。

請注意，`AdobeSignHelperMethods` OSGi套件組合無法辨識為Adobe Experience Manager (AEM)產品代碼，因此Adobe支援不支援它。
![簽署組態](assets/sign-configuration.png)


## API檔案

下列專案可透過OSGi套件組合中提供的`AcrobatSignHelperMethods` OSGi服務取得。

### getTransientDocumentID

`String getTransientDocumentID(Document documentForSigning) throws IOException`


用來建立合約或網頁表單的檔案。 檔案會先由寄件者上傳至Acrobat Sign。 這稱為&#x200B;_暫時_，因為它在上傳後7天內才可供使用。 此方法接受`com.adobe.aemfd.docmanager.Document`並傳回暫時性檔案識別碼。

### getAgreementID

`String getAgreementId(String transientDocumentID, String email) throws ClientProtocolException, IOException`

使用暫時性檔案ID傳送待簽署檔案給電子郵件引數所識別的使用者。

### getWidgetID

`String getWidgetID(String transientDocumentID)`

Widget就像可重複使用的範本，可以多次向使用者展示並多次簽署。 使用此方法以暫時性檔案ID取得Widget ID。

### getWidgetURL

`String getWidgetURL(String widgetId) throws ClientProtocolException, IOException`

取得特定Widget ID的Widget URL。 然後可以將此Widget URL呈現給使用者以供簽署檔案。

## 使用API

`AcrobatSignHelperMethods`是OSGi服務，因此必須在您的Java程式碼中使用@Reference註解來加上註解。

```java
...
// Import the AcrobatSignHelperMethods from the provided bundle
import com.acrobatsign.core.AcrobatSignHelperMethods;
...

@Component(service = { Example.class })
public class ExampleImpl implements Example {

 // Gain a reference to the provided AcrobatSignHelperMethods OSGi service
 @Reference
 com.acrobatsign.core.AcrobatSignHelperMethods acrobatSignHelperMethods;

 function void example() { 
    ...
    // Use the AcrobatSignHelperMethods API methods in your code
    String transientDocumentId = acrobatSignHelperMethods.getTransientDocumentID(documentForSigning);

    String agreementId = acrobatSignHelperMethods.getAgreementId(transientDocumentID, "johndoe@example.com");
    ...
 }
}
```
