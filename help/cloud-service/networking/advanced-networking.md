---
title: 高級網路
description: 了解AEM as a Cloud Service的進階網路選項。
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.png
last-substantial-update: 2022-10-13T00:00:00Z
exl-id: d1c1a3cf-989a-4693-9e0f-c1b545643e41
source-git-commit: d0b13fd37f1ed42042431246f755a913b56625ec
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 0%

---

# 高級網路

AEMas a Cloud Service提供進階的網路功能，可精確管理與AEMas a Cloud Service程式之間的連線。

|  | [生產計畫](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html) | [沙箱方案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) |
|---------------------------------------------------|:-----------------------:|:---------------------:|
| 支援高級網路 | ✔ | ✘ |


AEM進階網路由三種選項組成，用於管理與外部服務的連接。 Cloud Manager程式及其AEMas a Cloud Service環境一次只能使用單一類型的進階網路設定，因此請確定已選取最合適的類型。

|  | 標準埠上的HTTP/HTTPS | 非標準埠上的HTTP/HTTPS | 非HTTP/HTTPS連線 | 專用輸出IP | 「無代理主機」清單 | 連接到受VPN保護的服務 | 依IP限制AEM發佈流量 |
|-----------------------------------|:----------------------------:|:--------------------------------:|:--------------------------:|:-------------------:|:-------------------------------------:|:-------------------------------------:|:----:|
| __無高級網路__ | ✔ | ✘ | ✘ | ✘ | ✘ | ✘ | ✘ |
| [__靈活的埠輸出__](./flexible-port-egress.md) | ✔ | ✔ | ✔ | ✘ | ✘ | ✘ | ✘ |
| [__專用的輸出IP地址__](./dedicated-egress-ip-address.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✘ | ✘ |
| [__虛擬專用網__](./vpn.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |


有關選擇適當的高級網路類型時涉及的注意事項的詳細資訊，請參見 [進階網路檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html).

## 進階網路教學課程

在根據貴組織的需求確定最合適的進階網路選項後，按一下以下對應的教學課程以取得逐步指示和程式碼範例。

<table>
  <tr>
   <td>
      <a  href="./flexible-port-egress.md"><img alt="靈活的埠輸出" src="./assets/flexible-port-egress.png"/></a>
      <div><strong><a href="./flexible-port-egress.md">靈活的埠輸出</a></strong></div>
      <p>
          允許非標準埠上的傳出AEMas a Cloud Service流量。
      </p>
    </td>   
   <td>
      <a  href="./dedicated-egress-ip-address.md"><img alt="檔案專用輸出IP地址" src="./assets/dedicated-egress-ip-address.png"/></a>
      <div><strong><a href="./dedicated-egress-ip-address.md">專用的輸出IP地址</a></strong></div>
      <p>
        源自專用IP的傳出AEMas a Cloud Service流量。
      </p>
    </td>   
   <td>
      <a  href="./vpn.md"><img alt="虛擬專用網(VPN)" src="./assets/vpn.png"/></a>
      <div><strong><a href="./vpn.md">虛擬專用網(VPN)</a></strong></div>
      <p>
        保護客戶或供應商基礎架構與AEMas a Cloud Service之間的流量。
      </p>
    </td>   
  </tr>
</table>

## 程式碼範例

本集合提供針對特定使用案例運用進階網路功能所需的設定和程式碼範例。

確保 [高級網路配置](#advanced-networking) 是在遵循這些教學課程之前設定的。

<table><tr>
   <td>
      <a  href="./examples/email-service.md"><img alt="虛擬專用網(VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">電子郵件服務</a></strong></div>
      <p>
        OSGi設定範例，使用AEM連線至外部電子郵件服務。
      </p>
    </td>  
    <td>
        <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
        <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
        <p>
            Java™程式碼範例，使用HTTP/HTTPS通訊協定，將從AEMas a Cloud Service的HTTP/HTTPS連線至外部服務。
        </p>
    </td>
    <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="使用JDBC DataSourcePool的SQL連接" src="./assets//code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">使用JDBC DataSourcePool的SQL連接</a></strong></div>
      <p>
            通過配置AEM JDBC資料源池連接到外部SQL資料庫的Java™代碼示例。
      </p>
    </td>   
    </tr><tr>
    <td>
      <a  href="./examples/sql-java-apis.md"><img alt="使用Java API的SQL連接" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">使用Java™ API的SQL連接</a></strong></div>
      <p>
            使用Java™的SQL API連接到外部SQL資料庫的Java™代碼示例。
      </p>
    </td>   
    <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html"><img alt="套用IP允許清單" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html">套用IP允許清單</a></strong></div>
      <p>
            配置IP允許清單，使得只有VPN流量才能訪問AEM。
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections"><img alt="AEM Publish的路徑型VPN存取限制" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections">AEM Publish的路徑型VPN存取限制</a></strong></div>
      <p>
            AEM發佈上的特定路徑需要VPN存取權。
      </p>
    </td>
</tr>
</table>
