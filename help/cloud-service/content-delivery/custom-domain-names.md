---
title: 自訂網域名稱選項
description: 瞭解如何為您的AEM as a Cloud Service託管網站管理和實作自訂網域名稱。
version: Cloud Service
feature: Cloud Manager, Custom Domain Names
topic: Architecture, Migration
role: Developer, Architect
level: Intermediate
doc-type: Article
duration: 0
last-substantial-update: 2024-08-09T00:00:00Z
jira: KT-15615
source-git-commit: 13657903c37b90c6d854dcba317dc1801d869de0
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 1%

---


# 自訂網域名稱選項

瞭解如何為您的AEM as a Cloud Service託管網站管理和實作網域名稱。

>[!VIDEO](https://video.tv.adobe.com/v/3432632?quality=12&learn=on)

## 開始之前

開始實作自訂網域名稱之前，請務必瞭解下列概念：

### 什麼是網域名稱

網域名稱是人類易記的網站名稱，例如adobe.com，指向網際網路上的特定位置（例如170.2.14.16的IP位址）。

### AEM as a Cloud Service中的預設網域名稱

依預設，AEM as a Cloud Service已布建預設網域名稱，結尾為`*.adobeaemcloud.com`。 針對`*.adobeaemcloud.com`發行的萬用字元SSL憑證會自動套用至所有環境，而此萬用字元憑證是Adobe的責任。

預設網域名稱為`https://<SERVICE-TYPE>-p<PROGRAM-ID>-e<ENVIRONMENT-ID>.adobeaemcloud.com`格式。

- `<SERVICE-TYPE>`可以是&#x200B;**作者**、**發佈**&#x200B;或&#x200B;**預覽**。
- `<PROGRAM-ID>`是方案的唯一識別碼。 一個組織可以有多個計畫。
- `<ENVIRONMENT-ID>`是環境的唯一識別碼，每個程式都包含四個環境： **快速開發(RDE)**、**dev**、**階段**&#x200B;和&#x200B;**prod**。 除了沒有預覽環境的&#x200B;**RDE**&#x200B;之外，每個環境都包含上述三種服務型別。

簡而言之，布建所有AEM as a Cloud Service環境後，您就有&#x200B;**11** （RDE沒有預覽環境）唯一URL與預設網域名稱結合。

### Adobe管理的CDN和客戶管理的CDN

為了減少延遲並改善網站效能，AEM as a Cloud Service已與Adobe管理的內容傳遞網路(CDN)整合。 Adobe管理的CDN已為所有環境自動啟用。 如需詳細資訊，請參閱[AEM as a Cloud Service快取](../caching/overview.md)。

不過，客戶也可以使用自己的CDN，稱為&#x200B;**客戶管理的CDN**。 這並非必要，但很少有客戶因公司政策或其他原因使用此功能。 在此情況下，客戶應負責管理CDN設定和設定。

### 自訂網域名稱

對於品牌、真實性和業務開發目的，自訂網域名稱總是比預設網域名稱更可取。 但是，它們只能套用至&#x200B;**發佈**&#x200B;和&#x200B;**預覽**&#x200B;服務型別，而非&#x200B;**作者**。

新增自訂網域名稱時，您必須提供指定自訂網域的有效SSL憑證。 SSL憑證必須是由受信任的憑證授權單位(CA)簽署的有效憑證。

客戶通常會將自訂網域名稱用於Prod環境(AEM as a Cloud Service網站)，有時也會用於&#x200B;**stage**&#x200B;或&#x200B;**dev**&#x200B;等較低環境。

| AEM服務型別 | 支援自訂網域？ |
|---------------------|:-----------------------:|
| 作者 | ✘ |
| 預覽 | ✔ |
| 發佈 | ✔ |

## 實作網域名稱

若要使用Adobe管理的CDN或客戶管理的CDN實作網域名稱，下列流程圖會引導您完成此程式：

![網域名稱管理流程圖](./assets/domain-name-management-flowchart.png){width="800" zoomable="yes"}

此外，下表會指引您從何處管理特定設定：

| 自訂網域名稱和 | 新增SSL憑證至 | 新增網域名稱到 | 設定DNS記錄於 | 需要HTTP標頭驗證CDN規則嗎？ |
|---------------------|:-----------------------:|-----------------------:|-----------------------:|-----------------------:|
| Adobe管理的CDN | AdobeCloud Manager | AdobeCloud Manager | DNS託管服務 | ✘ |
| 客戶管理的CDN | CDN供應商 | CDN供應商 | DNS託管服務 | ✔ |

### 逐步教學課程

現在您已經瞭解網域名稱管理程式，您可以依照下列教學課程，實作AEM as a Cloud Service網站的自訂網域名稱：

**[具有Adobe管理的CDN的自訂網域名稱](./custom-domain-name-with-adobe-managed-cdn.md)**：在本教學課程中，您將瞭解如何以Adobe管理的CDN **將自訂網域名稱新增到**AEM as a Cloud Service網站。
**[使用客戶管理的CDN的自訂網域名稱](./custom-domain-names-with-customer-managed-cdn.md)**：在本教學課程中，您將瞭解如何使用客戶管理的CDN **將自訂網域名稱新增到** AEM as a Cloud Service網站。

