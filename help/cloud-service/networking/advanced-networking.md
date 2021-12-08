---
title: 高級網路
description: 了解AEM as a Cloud Service的進階網路選項。
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
source-git-commit: 6f047a76693bc05e64064fce6f25348037749f4c
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 0%

---


# 高級網路

AEM as a Cloud Service提供三個管理與外部服務的連接的選項。 Cloud Manager程式及其AEMas a Cloud Service環境一次只能使用單一類型的進階網路設定，因此請確定已選取最合適的類型。

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
