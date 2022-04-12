---
title: 非標準埠上的HTTP/HTTPS連接，用於專用出口IP地址和VPN
description: 瞭解如何使HTTP/HTTPS請求從AEMas a Cloud Service到外部Web服務在非標準埠上運行，用於專用出口IP地址和VPN
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
exl-id: a565bc3a-675f-4d5e-b83b-c14ad70a800b
source-git-commit: d00e47895d1b2b6fb629b8ee9bcf6b722c127fd3
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 0%

---

# 非標準埠上的HTTP/HTTPS連接，用於專用出口IP地址和VPN

非標準埠（不是80/443）上的HTTP/HTTPS連接必須從as a Cloud Service中，但它們不需要任何特殊 `portForwards` 規則，並可AEM以使用 `AEM_HTTP_PROXY_HOST`。 `AEM_HTTP_PROXY_PORT`。 `AEM_HTTPS_PROXY_HOST`, `AEM_HTTPS_PROXY_PORT`。

## 高級網路支援

以下高級網路選項支援以下代碼示例。

確保 [適當](../advanced-networking.md#advanced-networking) 本教程之後，高級網路配置已設定完畢。

| 無高級網路 | [靈活的埠出口](../flexible-port-egress.md) | [專用出口IP地址](../dedicated-egress-ip-address.md) | [虛擬專用網路](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✘ | ✔ | ✔ |

>[!CAUTION]
>
> 此代碼示例僅用於 [專用出口IP地址](../dedicated-egress-ip-address.md) 和 [虛擬專用網](../vpn.md)。 類似但不同的代碼示例可用於 [非標準埠上的HTTP/HTTPS連接，用於靈活埠輸出](./http-on-non-standard-ports-flexible-port-egress.md)。

## 代碼示例

此Java™代碼示例是OSGi服務的示例，該服務可以在AEMas a Cloud Service中運行，該在8080上與外部Web伺服器建立HTTP連接。 與HTTPS Web伺服器的連接使用 `AEM_HTTPS_PROXY_HOST` 和 `AEM_HTTPS_PROXY_PORT` 而不是  `AEM_HTTP_PROXY_HOST` 和 `AEM_HTTP_PROXY_PORT`。

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
import java.net.InetSocketAddress;
import java.net.ProxySelector;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.time.Duration;

@Component
public class HttpExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(HttpExternalServiceImpl.class);

    @Override
    public boolean isAccessible() {
        HttpClient client;

        // If the URL is http, use System.getenv("AEM_HTTP_PROXY_HOST") and System.getenv("AEM_HTTP_PROXY_PORT")
        // Else if the URL is https, us System.getenv("AEM_HTTPS_PROXY_HOST") and System.getenv("AEM_HTTPS_PROXY_PORT")

        if (System.getenv("AEM_HTTP_PROXY_HOST") != null) {
            // Create a ProxySelector that maps to AEM's provided AEM_HTTP_PROXY_HOST and AEM_HTTP_PROXY_PORT
            ProxySelector proxySelector = ProxySelector.of(
                    new InetSocketAddress(System.getenv("AEM_HTTP_PROXY_HOST"),
                            Integer.parseInt(System.getenv("AEM_HTTP_PROXY_PORT"))));
            // Create an HttpClient and provide the proxy selector that will use AEM's native HTTP proxy configuration
            client = HttpClient.newBuilder().proxy(proxySelector).build();
            log.debug("Using HTTPClient with AEM_HTTP_PROXY");
        } else {
            client = HttpClient.newBuilder().build();
            // If no proxy is set up (such as local dev)
            log.debug("Using HTTPClient without AEM_HTTP_PROXY");
        }

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
