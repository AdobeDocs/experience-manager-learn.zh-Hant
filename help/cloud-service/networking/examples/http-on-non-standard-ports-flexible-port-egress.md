---
title: 非標準埠上的HTTP/HTTPS連接，可靈活輸出埠
description: 了解如何讓AEM的HTTP/HTTPS要求as a Cloud Service到非標準埠上執行的外部Web服務，以利執行彈性埠輸出。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
source-git-commit: 6ed26e5c9bf8f5e6473961f667f9638e39d1ab0e
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# 非標準埠上的HTTP/HTTPS連接，可靈活輸出埠

非標準埠(非80/443)上的HTTP/HTTPS連線必須從AEMas a Cloud Service中代理，但不需要任何特殊 `portForwards` 規則，並可使用AEM進階網路 `AEM_PROXY_HOST` 和保留的代理埠 `AEM_HTTP_PROXY_HOST` 或 `AEM_HTTPS_PROXY_HOST` 目的地是否為HTTP/HTTPS，依不同而定。

## 高級網路支援

下列進階網路選項支援下列程式碼範例。

| 無高級網路 | [靈活的埠輸出](../flexible-port-egress.md) | [專用的輸出IP地址](../dedicated-egress-ip-address.md) | [虛擬專用網](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✘ | ✘ |

>[!CAUTION]
>
> 此程式碼範例僅適用於 [靈活的埠輸出](../flexible-port-egress.md). 有類似但不同的程式碼範例可供使用 [非標準埠上專用輸出IP地址和VPN的HTTP/HTTPS連接](./http-on-non-standard-ports.md).

## 程式碼範例

此Java™程式碼範例是OSGi服務，可在AEMas a Cloud Service中執行，以便與8080的外部Web伺服器進行HTTP連線。 與HTTPS Web伺服器的連線使用環境變數 `AEM_PROXY_HOST` 和 `AEM_HTTPS_PROXY_PORT` (預設為 `proxy.tunnel:3128` 在AEM版本&lt; 6094)中。

>[!NOTE]
> 建議 [Java™ 11 HTTP API](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/package-summary.html) 用於從AEM進行HTTP/HTTPS呼叫。

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

        // Use System.getenv("AEM_PROXY_HOST") and proxy port System.getenv("AEM_HTTP_PROXY_HOST") 
        // or System.getenv("AEM_HTTPS_PROXY_HOST"), depending on if the destination requires HTTP/HTTPS

        if (System.getenv("AEM_PROXY_HOST") != null) {
            // Create a ProxySelector that maps to AEM's provided AEM_PROXY_HOST, with a fallback of proxy.tunnel
            // And proxy port contained in the AEM_HTTP_PROXY_PORT variable if the destination requires HTTP, then use the variable AEM_HTTPS_PROXY_PORT instead of AEM_HTTP_PROXY_PORT
            // The explicit fallback of 3128 will be obsoleted in Jan 2022, and only the  AEM_HTTP_PROXY_PORT/AEM_HTTPS_PROXY_PORT will be required
            ProxySelector proxySelector = ProxySelector.of(new InetSocketAddress(
                System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel"), 
                Integer.parseInt(System.getenv().getOrDefault("AEM_HTTP_PROXY_PORT", "3128"))));

            client = HttpClient.newBuilder().proxy(proxySelector).build();
            log.debug("Using HTTPClient with AEM_PROXY_HOST");
        } else {
            client = HttpClient.newBuilder().build();
            // If no proxy is set up (such as local dev)
            log.debug("Using HTTPClient without AEM_PROXY_HOST");
        }

        // Prepare the full URI to request, note this will have the
        // - Scheme (http/https)
        // - External host name
        // - External port
        // The external service URI, including the scheme/host/port, is defined in code, and NOT in Cloud Manager portForwards rules.
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
