---
title: 使用客戶管理的CDN的自訂網域名稱
description: 瞭解如何在使用客戶管理的CDN的AEM as a Cloud Service網站上實作自訂網域名稱。
version: Cloud Service
feature: Cloud Manager, Operations
topic: Administration, Architecture
role: Admin, Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-06-21T00:00:00Z
jira: KT-15945
thumbnail: KT-15945.jpeg
exl-id: fa9ee14f-130e-491b-91b6-594ba47a7278
source-git-commit: 98f1996dbeb6a683f98ae654e8fa13f6c7a2f9b2
workflow-type: tm+mt
source-wordcount: '1051'
ht-degree: 0%

---

# 使用客戶管理的CDN的自訂網域名稱

瞭解如何將自訂網域名稱新增至使用&#x200B;**客戶管理的CDN**&#x200B;的AEM as a Cloud Service網站。

在本教學課程中，使用客戶管理的CDN新增HTTPS可定址自訂網域名稱`wkndviaawscdn.enablementadobe.com`以及傳輸層安全性(TLS)，藉此加強範例[AEM WKND](https://github.com/adobe/aem-guides-wknd)網站的品牌。 在本教學課程中，AWS CloudFront是作為客戶管理的CDN，不過任何CDN提供者都應該與AEM as a Cloud Service相容。

>[!VIDEO](https://video.tv.adobe.com/v/3432561?quality=12&learn=on)

高層級步驟為：

具有客戶CDN](./assets/add-custom-domain-name-with-customer-CDN.png){width="800" zoomable="yes"}的![自訂網域名稱

## 先決條件

>[!VIDEO](https://video.tv.adobe.com/v/3432562?quality=12&learn=on)

- [OpenSSL](https://www.openssl.org/)和[dig](https://www.isc.org/blogs/dns-checker/)已安裝在您的本機電腦上。
- 存取協力廠商服務：
   - 憑證授權單位(CA) — 要求網站網域（例如[DigitCert](https://www.digicert.com/)）的已簽署憑證
   - 客戶CDN — 設定客戶CDN和新增SSL憑證和網域詳細資訊，例如AWS CloudFront、Azure CDN或Akamai。
   - 網域名稱系統(DNS)託管服務 — 為您的自訂網域新增DNS記錄，例如Azure DNS或AWS Route 53。
- 存取[Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)，將HTTP標頭驗證CDN規則部署至AEM as a Cloud Service環境。
- 範例[AEM WKND](https://github.com/adobe/aem-guides-wknd)網站已部署至[生產程式](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs)型別的AEM as a Cloud Service環境。

如果您無法存取協力廠商服務，請&#x200B;_與您的安全性或託管團隊共同作業，以完成步驟_。

## 產生SSL憑證

>[!VIDEO](https://video.tv.adobe.com/v/3427908?quality=12&learn=on)

您有兩個選項：

1. 使用`openssl`命令列工具 — 您可以為您的網站網域產生私密金鑰和憑證簽署要求(CSR)。 若要請求已簽署的憑證，請將CSR提交給憑證授權單位(CA)。
1. 您的託管團隊會提供您網站所需的私密金鑰和已簽署的憑證。

讓我們來回顧第一個選項的步驟。

若要產生私密金鑰和CSR，請執行以下命令，並在出現提示時提供所需資訊：

```bash
# Generate a private key and a CSR
$ openssl req -newkey rsa:2048 -keyout <YOUR-SITE-NAME>.key -out <YOUR-SITE-NAME>.csr -nodes
```

若要請求已簽署的憑證，請依照相關檔案的說明，將產生的CSR提供給CA。 CA簽署CSR後，您會收到簽署的憑證檔案。

### 檢閱簽署的憑證

在將已簽署的憑證新增至Cloud Manager之前先加以檢閱是良好的作法。 您可以使用以下命令來檢閱憑證詳細資訊：

```bash
# Review the certificate details
$ openssl crl2pkcs7 -nocrl -certfile <YOUR-SIGNED-CERT>.crt | openssl pkcs7 -print_certs -noout
```

簽署的憑證可能包含憑證鏈，其中包括根和中間憑證以及終端實體憑證。

Adobe Cloud Manager在個別的表單欄位&#x200B;_中接受終端實體憑證和憑證鏈結_，因此您必須從簽署的憑證中擷取終端實體憑證和憑證鏈結。

在本教學課程中，以`*.enablementadobe.com`網域所簽發的[DigitCert](https://www.digicert.com/)已簽署憑證為例。 透過在文字編輯器中開啟已簽署的憑證並複製`-----BEGIN CERTIFICATE-----`和`-----END CERTIFICATE-----`標籤之間的內容來擷取終端實體和憑證鏈結。

## 設定客戶管理的CDN

>[!VIDEO](https://video.tv.adobe.com/v/3432563?quality=12&learn=on)

設定客戶CDN (例如AWS CloudFront、Azure CDN或Akamai)，並新增SSL憑證和網域詳細資料。 本教學課程以AWS CloudFront為例。 然而，視您的CDN供應商而定，步驟可能會有所不同。 關鍵圖說文字為：

- 將SSL憑證新增至CDN。
- 將自訂網域名稱新增到CDN。
- 設定CDN以快取內容，例如影像、CSS和JavaScript檔案。
- 將`X-Forwarded-Host` HTTP標頭新增至CDN設定，好讓您的CDN在其傳送給AEMCD來源的所有要求中包含此標頭。
- 請確定`Host`標頭值設定為包含方案和環境ID且結尾為`adobeaemcloud.com`的預設AEM as a Cloud Service網域。 從客戶CDN傳遞至Adobe CDN的HTTP主機標頭值必須是預設AEM as a Cloud Service網域，任何其他值都會導致錯誤狀態。

## 設定DNS記錄

>[!VIDEO](https://video.tv.adobe.com/v/3432564?quality=12&learn=on)

若要設定自訂網域的DNS記錄，請按照以下步驟操作，

1. 為指向CDN網域名稱的自訂網域新增CNAME記錄。

此教學課程會新增自訂網域`wkndviaawscdn.enablementadobe.com`的CNAME記錄至Azure DNS，並將其指向AWS CloudFront散發網域名稱。

### 網站驗證

透過使用自訂網域名稱存取網站來驗證自訂網域名稱。
根據AEM as a Cloud Service環境中的vhhost設定，此設定不一定有效。

關鍵的安全性步驟是將HTTP標題驗證CDN規則部署至AEM as a Cloud Service環境。 規則可確保請求來自客戶CDN，而不是來自任何其他來源。

## 目前無HTTP標頭驗證CDN規則的工作狀態

>[!VIDEO](https://video.tv.adobe.com/v/3432565?quality=12&learn=on)

若沒有HTTP標頭驗證CDN規則，`Host`標頭值將設定為包含方案和環境ID且結尾為`adobeaemcloud.com`的預設AEM as a Cloud Service網域。 只有在部署了HTTP標頭驗證CDN規則的情況下，Adobe CDN才會將`Host`標頭值轉換為從客戶CDN接收的`X-Forwarded-Host`值。 否則，`Host`標頭值會依原樣傳遞至AEM as a Cloud Service環境，且不會使用`X-Forwarded-Host`標頭。

### 用於列印主機標頭值的範例servlet程式碼

以下servlet程式碼會列印JSON回應中的`Host`、`X-Forwarded-*`、`Referer`和`Via` HTTP標頭值。

```java
package com.adobe.aem.guides.wknd.core.servlets;

import java.io.IOException;
import java.util.Enumeration;

import javax.servlet.Servlet;
import javax.servlet.ServletException;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.resource.ResourceResolverFactory;
import org.apache.sling.api.servlets.HttpConstants;
import org.apache.sling.api.servlets.ServletResolverConstants;
import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

@Component(service = Servlet.class, property = {
        ServletResolverConstants.SLING_SERVLET_PATHS + "=/bin/verify-headers",
        ServletResolverConstants.SLING_SERVLET_METHODS + "=" + HttpConstants.METHOD_GET
})
public class VerifyHeadersServlet extends SlingSafeMethodsServlet {

    @Reference
    private ResourceResolverFactory resourceResolverFactory;

    @Override
    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response)
            throws ServletException, IOException {
        response.setContentType("application/json");
        response.setCharacterEncoding("UTF-8");

        // Create JSON response
        StringBuilder jsonResponse = new StringBuilder();
        jsonResponse.append("{");

        Enumeration<String> headerNames = request.getHeaderNames();
        boolean firstHeader = true;

        while (headerNames.hasMoreElements()) {
            String headerName = headerNames.nextElement();

            if (headerName.startsWith("X-Forwarded-") || headerName.startsWith("Host")
                    || headerName.startsWith("Referer") || headerName.startsWith("Via")) {
                if (!firstHeader) {
                    jsonResponse.append(",");
                }
                jsonResponse.append("\"").append(headerName).append("\": \"").append(request.getHeader(headerName))
                        .append("\"");
                firstHeader = false;
            }
        }

        jsonResponse.append("}");

        response.getWriter().write(jsonResponse.toString());
    }
}
```

若要測試servlet，請使用以下設定更新`../dispatcher/src/conf.dispatcher.d/filters/filters.any`檔案。 也請確定CDN已設定為&#x200B;**不快取** `/bin/*`路徑。

```plaintext
# Testing purpose bin
/0300 { /type "allow" /extension "json" /path "/bin/*"}
/0301 { /type "allow" /path "/bin/*"}
/0302 { /type "allow" /url "/bin/*"}
```

## 設定和部署HTTP標頭驗證CDN規則

>[!VIDEO](https://video.tv.adobe.com/v/3432566?quality=12&learn=on)

若要設定和部署HTTP標頭驗證CDN規則，請遵循下列步驟：

- 在`cdn.yaml`檔案中新增HTTP標頭驗證CDN規則，以下提供範例。

  ```yaml
  kind: "CDN"
  version: "1"
  metadata:
    envTypes: ["prod"]
  data:
    authentication:
      authenticators:
        - name: edge-auth
          type: edge
          edgeKey1: ${{CDN_EDGEKEY_080124}}
          edgeKey2: ${{CDN_EDGEKEY_110124}}
      rules:
        - name: edge-auth-rule
          when: { reqProperty: tier, equals: "publish" }
          action:
          type: authenticate
          authenticator: edge-auth
  ```

- 使用Cloud Manager UI建立秘密型別的環境變數(CDN_EDGEKEY_080124、CDN_EDGEKEY_110124)。
- 使用Cloud Manager管道將HTTP標題驗證CDN規則部署到AEM as a Cloud Service環境。

## 在X-AEM-Edge-Key HTTP標頭中傳遞密碼

>[!VIDEO](https://video.tv.adobe.com/v/3432567?quality=12&learn=on)

更新客戶CDN以在`X-AEM-Edge-Key` HTTP標頭中傳遞密碼。 Adobe CDN使用密碼來驗證來自客戶CDN的請求，並將`Host`標頭值轉換為從客戶CDN接收的`X-Forwarded-Host`的值。

## 端對端視訊

您也可以觀看端對端影片，該影片會示範上述步驟，將具有客戶管理的CDN的自訂網域名稱新增到AEM as a Cloud Service託管的網站。

>[!VIDEO](https://video.tv.adobe.com/v/3432568?quality=12&learn=on)
