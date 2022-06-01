---
title: 高級網路
description: 瞭解AEMas a Cloud Service的高級網路選項。
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
exl-id: d1c1a3cf-989a-4693-9e0f-c1b545643e41
source-git-commit: a18bea7986062ff9cb731d794187760ff6e0339f
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 0%

---

# 高級網路

AEMas a Cloud Service提供高級聯網功能，可精確管理與as a Cloud Service程式的AEM連接。

|  | [生產計畫](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html) | [沙箱方案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) |
|---------------------------------------------------|:-----------------------:|:---------------------:|
| 支援高級網路 | ✔ | ✘ |


高級AEM網路包括三個用於管理與外部服務的連接的選項。 Cloud Manager程式及其AEMas a Cloud Service環境一次只能使用一種類型的高級網路配置，因此確保選擇了最合適的類型。

|  | 標準埠上的HTTP/HTTPS | 非標準埠上的HTTP/HTTPS | 非HTTP/HTTPS連接 | 專用出口IP | &quot;無代理主機&quot;清單 | 連接到受VPN保護的服務 | 按IP限制AEM發佈通信 |
|-----------------------------------|:----------------------------:|:--------------------------------:|:--------------------------:|:-------------------:|:-------------------------------------:|:-------------------------------------:|:----:|
| __無高級網路__ | ✔ | ✘ | ✘ | ✘ | ✘ | ✘ | ✘ |
| [__靈活的埠出口__](./flexible-port-egress.md) | ✔ | ✔ | ✔ | ✘ | ✘ | ✘ | ✘ |
| [__專用出口IP地址__](./dedicated-egress-ip-address.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✘ | ✘ |
| [__虛擬專用網路__](./vpn.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |


有關選擇適當的高級網路類型時涉及的注意事項的詳細資訊，請參見 [高級網路文檔](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html)。

## 高級網路教程

一旦根據您組織的需求確定了最合適的高級網路選項，請按一下下面的相應教程，瞭解逐步說明和代碼示例。

<table>
  <tr>
   <td>
      <a  href="./flexible-port-egress.md"><img alt="靈活的埠出口" src="./assets/flexible-port-egress.png"/></a>
      <div><strong><a href="./flexible-port-egress.md">靈活的埠出口</a></strong></div>
      <p>
          允許非AEM標準埠上的出站as a Cloud Service通信。
      </p>
    </td>   
   <td>
      <a  href="./dedicated-egress-ip-address.md"><img alt="FleDedicated egress IP地址" src="./assets/dedicated-egress-ip-address.png"/></a>
      <div><strong><a href="./dedicated-egress-ip-address.md">專用出口IP地址</a></strong></div>
      <p>
        從專用AEMIP發起出站as a Cloud Service通信。
      </p>
    </td>   
   <td>
      <a  href="./vpn.md"><img alt="虛擬專用網(VPN)" src="./assets/vpn.png"/></a>
      <div><strong><a href="./vpn.md">虛擬專用網(VPN)</a></strong></div>
      <p>
        保護客戶或供應商基礎架構與AEMas a Cloud Service之間的通信。
      </p>
    </td>   
  </tr>
</table>

## 代碼示例

此集合提供了為特定使用情形而利用高級網路功能所需的配置和代碼示例。

確保 [高級網路配置](#advanced-networking) 已在完成這些教程之前設定。

<table><tr>
   <td>
      <a  href="./examples/email-service.md"><img alt="虛擬專用網(VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">電子郵件服務</a></strong></div>
      <p>
        OSGi配置示例AEM，用於連接到外部電子郵件服務。
      </p>
    </td>  
    <td>
        <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
        <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
        <p>
            Java™代碼示例，使用HTTP/HTTPS協AEM議將HTTP/HTTPS從as a Cloud Service連接到外部服務。
        </p>
    </td>
    <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="使用JDBC DataSourcePool的SQL連接" src="./assets//code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">使用JDBC DataSourcePool的SQL連接</a></strong></div>
      <p>
            通過配置JDBC資料源池連接到外部SQL資料AEM庫的Java™代碼示例。
      </p>
    </td>   
    </tr><tr>
    <td>
      <a  href="./examples/sql-java-apis.md"><img alt="使用Java API的SQL連接" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">使用Java™ API的SQL連接</a></strong></div>
      <p>
            Java™代碼示例使用Java™的SQL API連接到外部SQL資料庫。
      </p>
    </td>   
    <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html"><img alt="應用IP允許清單" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html">應用IP允許清單</a></strong></div>
      <p>
            配置IP允許清單，以便只有VPN通信可以訪問AEM。
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections"><img alt="基於路徑的VPN對AEM發佈的訪問限制" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections">基於路徑的VPN對AEM發佈的訪問限制</a></strong></div>
      <p>
            AEM發佈上的特定路徑需要VPN訪問。
      </p>
    </td>
</tr>
</table>
