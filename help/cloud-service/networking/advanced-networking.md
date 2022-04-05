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
source-git-commit: 6e7130cd98700bdb5e7f330ca0506fe89ea0eb94
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 0%

---

# 高級網路

AEMas a Cloud Service提供高級聯網功能，可精確管理與as a Cloud Service程式的AEM連接。

|  | [生產計畫](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html) | [沙箱方案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) |
|---------------------------------------------------|:-----------------------:|:---------------------|
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
