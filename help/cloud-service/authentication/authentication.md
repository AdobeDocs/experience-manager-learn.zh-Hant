---
title: 在AEMas a Cloud Service
description: 瞭解「AEMas a Cloud Service」中的身份驗證。
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
kt: 10436
thumbnail: KT-10436.jpeg
source-git-commit: e666e38d6b2a7057f7016b35ad1034a4487e9bc7
workflow-type: tm+mt
source-wordcount: '115'
ht-degree: 4%

---


# AEMas a Cloud Service驗證

AEMas a Cloud Service支援多個身份驗證選項，並因服務類型而異。

|  | AEM 作者 | AEM 發佈 |
|-----------------------|:----------:|:-----------:|
| [Adobe IMS](../accessing/overview.md) | ✔ | ✘ |
| [SAML 2.0](./saml-2-0.md) | ✘ | ✔ |
| [令牌驗證](../../headless-tutorial/authentication/overview.md) | ✔ | ✔ |

## 驗證選項

按一下下面的相應連結以瞭解有關如何設定和使用身份驗證方法的詳細資訊。

<table>
  <tr>
   <td>
      <a  href="../accessing/overview.md"><img alt="Adobe IMS" src="./assets/card--adobe-ims.png"/></a>
      <div><strong><a href="../accessing/overview.md">Adobe IMS</a></strong></div>
      <p>
          使用Adobe IMS通過Adobe Admin Console管理AEM作者訪問。
      </p>
    </td>   
   <td>
      <a  href="./saml-2-0.md"><img alt="SAML 2.0" src="./assets/card--saml-2-0.png"/></a>
      <div><strong><a href="./saml-2-0.md">SAML 2.0</a></strong></div>
      <p>
        使用AEM發佈服務的SAML 2.0整合，將網站的用戶驗證到IDP。
      </p>
    </td>   
   <td>
      <a  href="../../headless-tutorial/authentication/overview.md"><img alt="Token" src="./assets/card--token.png"/></a>
      <div><strong><a href="../../headless-tutorial/authentication/overview.md">令牌驗證</a></strong></div>
      <p>
        允許應用程式和中間件使用AEMAPI服務令牌進行身份驗證。
      </p>
    </td>   
  </tr>
</table>
