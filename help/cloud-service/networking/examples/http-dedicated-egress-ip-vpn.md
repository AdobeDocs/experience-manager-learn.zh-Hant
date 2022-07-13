---
title: 用於專用出口IP地址和VPN的HTTP/HTTPS連接
description: 瞭解如何使HTTP/HTTPS請求從為專用出口IP地AEM址和VPN運行的as a Cloud Service到外部Web服務
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
exl-id: a565bc3a-675f-4d5e-b83b-c14ad70a800b
source-git-commit: bdce84fdcc949c8f8d0690ee7110238d8e8d3e42
workflow-type: tm+mt
source-wordcount: '233'
ht-degree: 0%

---

# 用於專用出口IP地址和VPN的HTTP/HTTPS連接

HTTP/HTTPS連接在專用出口IP地AEM址或VPN的as a Cloud Service之外自動進行代理，不需要任何特殊的連接 `portForwards` 規則。

## 高級網路支援

以下高級網路選項支援以下代碼示例。

確保 [專用出口IP地址或VPN](../advanced-networking.md#advanced-networking) 本教程之後，高級網路配置已設定完畢。

| 無高級網路 | [靈活的埠出口](../flexible-port-egress.md) | [專用出口IP地址](../dedicated-egress-ip-address.md) | [虛擬專用網路](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✘ | ✔ | ✔ |

>[!CAUTION]
>
> 此代碼示例僅用於 [專用出口IP地址](../dedicated-egress-ip-address.md) 和 [虛擬專用網](../vpn.md)。 類似但不同的代碼示例可用於 [非標準埠上的HTTP/HTTPS連接，用於靈活埠輸出](./http-on-non-standard-ports-flexible-port-egress.md)。

## 代碼示例

此Java™代碼示例是OSGi服務的示例，該服務可以在AEMas a Cloud Service中運行，該在8080上與外部Web伺服器建立HTTP連接。 HTTPS（或HTTP）連接自動代理AEM出as a Cloud Service，不需要特殊開發。

>[!NOTE]
> 建議 [Java™ 11 HTTP API](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/package-summary.html) 用於從中進行HTTP/HTTPS調AEM用。

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/HttpExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.io.IOException;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.time.Duration;

@Component
public class HttpExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(HttpExternalServiceImpl.class);

    // client with connection pool reused for all requests
    private HttpClient client = HttpClient.newBuilder().build();

    @Override
    public boolean isAccessible() {

        // Prepare the full URI to request, note this will have the
        // - Scheme (http/https)
        // - External host name
        // - External port
        // The external service URI, including the scheme/host/port, is defined in code, rather than in Cloud Manager portForwards rules.
        URI uri = URI.create("http://api.example.com:8080/test.json");

        // Prepare the HttpRequest
        HttpRequest request = HttpRequest.newBuilder().uri(uri).timeout(Duration.ofSeconds(2)).build();

        // Send the HttpRequest using the configured HttpClient
        HttpResponse<String> response = null;
        try {
            // Request the URL
            response = client.send(request, HttpResponse.BodyHandlers.ofString());

            log.debug("HTTP response body: {} ", response.body());

            // Our simple example returns true is response is successful! (200 status code)
            return response.statusCode() == 200;
        } catch (IOException e) {
            return false;
        } catch (InterruptedException e) {
            return false;
        }
    }
}
```
