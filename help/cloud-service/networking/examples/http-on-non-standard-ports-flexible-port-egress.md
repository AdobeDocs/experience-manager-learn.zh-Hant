---
title: 非標準埠上的HTTP/HTTPS連接，用於靈活的埠輸出
description: 瞭解如何使HTTP/HTTPS請求從as a Cloud Service到AEM外部Web服務在非標準埠上運行，以用於靈活埠出口。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
exl-id: c8cc0385-9e94-4120-9fb1-aeccbfcc8aa4
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 0%

---

# 非標準埠上的HTTP/HTTPS連接，用於靈活的埠輸出

非標準埠（不是80/443）上的HTTP/HTTPS連接必須從as a Cloud Service中，但它們不需要任何特殊 `portForwards` 規則，並可AEM以使用 `AEM_PROXY_HOST` 和保留的代理埠 `AEM_HTTP_PROXY_PORT` 或 `AEM_HTTPS_PROXY_PORT` 取決於目標是HTTP/HTTPS。

## 高級網路支援

以下高級網路選項支援以下代碼示例。

確保 [適當](../advanced-networking.md#advanced-networking) 本教程之後，高級網路配置已設定完畢。

| 無高級網路 | [靈活的埠出口](../flexible-port-egress.md) | [專用出口IP地址](../dedicated-egress-ip-address.md) | [虛擬專用網路](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✘ | ✘ |

>[!CAUTION]
>
> 此代碼示例僅用於 [靈活埠出口](../flexible-port-egress.md)。 類似但不同的代碼示例可用於 [非標準埠上專用出口IP地址和VPN的HTTP/HTTPS連接](./http-dedicated-egress-ip-vpn.md)。

## 代碼示例

此Java™代碼示例是OSGi服務的示例，該服務可以在AEMas a Cloud Service中運行，該在8080上與外部Web伺服器建立HTTP連接。 與HTTPS Web伺服器的連接使用環境變數 `AEM_PROXY_HOST` 和 `AEM_HTTPS_PROXY_PORT` (預設為 `proxy.tunnel:3128` &lt; AEM 6094)。

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

        // Use System.getenv("AEM_PROXY_HOST") and proxy port System.getenv("AEM_HTTP_PROXY_PORT") 
        // or System.getenv("AEM_HTTPS_PROXY_PORT"), depending on if the destination requires HTTP/HTTPS

        if (System.getenv("AEM_PROXY_HOST") != null) {
            // Create a ProxySelector that uses to AEM's provided AEM_PROXY_HOST, with a fallback of proxy.tunnel, and proxy port using the AEM_HTTP_PROXY_PORT variable. 
            // If the destination requires HTTPS, then use the variable AEM_HTTPS_PROXY_PORT instead of AEM_HTTP_PROXY_PORT.
 
            ProxySelector proxySelector = ProxySelector.of(new InetSocketAddress(
                System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel"), 
                Integer.parseInt(System.getenv().get("AEM_HTTP_PROXY_PORT"))));

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
