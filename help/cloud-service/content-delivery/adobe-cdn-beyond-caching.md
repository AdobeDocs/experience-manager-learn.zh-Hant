---
title: Adobe CDN — 快取以外的進階功能
description: 瞭解Adobe CDN除了快取以外的進階功能，例如在CDN設定流量、設定代號和認證、CDN錯誤頁面等。
version: Cloud Service
feature: Website Performance, CDN Cache
topic: Architecture, Performance, Content Management
role: Developer, Architect, User, Leader
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2024-08-21T00:00:00Z
jira: KT-15123
thumbnail: KT-15123.jpeg
exl-id: 8948a900-01e9-49ed-9ce5-3a057f5077e4
source-git-commit: 8795024a7b5e6d10cb2ff2f770dd3d080af85e68
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 0%

---

# Adobe CDN — 快取以外的進階功能

瞭解Adobe內容傳遞網路(CDN)除了快取以外的進階功能，例如在CDN設定流量、設定代號和認證、CDN錯誤頁面等。

除了快取內容以外，Adobe CDN還提供數種進階功能，有助於將您的網站效能最佳化。 這些功能包括：

- 在CDN設定流量
- 設定CDN認證和驗證
- CDN錯誤頁面

這些功能是&#x200B;**自助服務**&#x200B;功能。 已在AEM專案的`cdn.yaml`檔案中設定，並使用Cloud Manager設定管道進行部署。

>[!VIDEO](https://video.tv.adobe.com/v/3433104?quality=12&learn=on)

## 在CDN設定流量

讓我們瞭解與&#x200B;_在CDN_&#x200B;設定流量相關的主要功能：

- **DoS攻擊預防：** AdobeCDN在網路層吸收了DoS攻擊，使其無法連線到您的原始伺服器。
- **速率限制：**&#x200B;若要保護您的原始伺服器不受太多要求的影響，您可以在CDN上設定速率限制。
- **Web應用程式防火牆(WAF)：** WAF可保護您的網站，使其免受一般Web應用程式弱點的影響，例如SQL插入、跨網站指令碼等等。 使用此功能需要增強式安全性授權或WAF-DDoS保護授權。
- **要求轉換：**&#x200B;修改傳入的要求，例如設定或取消設定標頭、修改查詢引數、Cookie等等。
- **回應轉換：**&#x200B;修改傳出回應，例如設定或取消設定標頭。
- **來源選擇：**&#x200B;根據要求URL將流量路由到不同的來源伺服器(Adobe和非Adobe)。
- **URL重新導向：**&#x200B;將要求(HTTP 301/302)重新導向至不同的絕對或相對URL。

## 設定CDN認證和驗證

讓我們瞭解與&#x200B;_設定CDN認證和驗證_&#x200B;相關的金鑰功能：

- **清除API Token**：可讓您建立自己的清除金鑰，從快取中清除單一或群組或所有資源。
- **基本驗證**：當您想要限制存取您的網站或網站的一部分時，可以使用輕量型的驗證機制。 通常在上線前作為各種稽核流程的一部分需要。
- **HTTP標頭驗證**：當客戶管理的CDN將流量路由到AdobeCDN時使用。 AdobeCDN會根據`X-AEM-Edge-Key`標頭值來驗證傳入的要求。 可讓您為`X-AEM-Edge-Key`標頭建立自己的值。

## CDN錯誤頁面

讓我們瞭解與&#x200B;_CDN錯誤頁面_&#x200B;相關的主要功能：

- **品牌錯誤頁面**：當AdobeCDN無法連線到您的原始伺服器時，在&#x200B;_不可能的情況_&#x200B;中向您的使用者顯示品牌錯誤頁面。

## 實施方式

這些進階功能的實作包含兩個步驟：

1. **更新CDN設定檔**：使用必要的設定更新您AEM專案中的`cdn.yaml`檔案。 這些設定會新增為規則，並遵循規則語法。 規則三個主要元件： `name`、`when`和`action`。

2. **部署CDN設定檔案**：使用Cloud Manager設定管道部署更新的`cdn.yaml`檔案。 如需詳細資訊，請參閱[透過Cloud Manager部署規則](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager)。

### 範例

在以下範例中，範例WKND網站設定為將`/top3` URL重新導向至`/us/en/top3.html`。

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  experimental_redirects:
    rules:
      - name: redirect-top3-adventures
        when: { reqProperty: path, equals: "/top3" }
        action:
          type: redirect
          status: 302
          location: /us/en/top3.html
```

## 相關Tutorials

[使用流量篩選規則保護網站](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/overview)

[設定並部署HTTP標頭驗證CDN規則](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/content-delivery/custom-domain-names-with-customer-managed-cdn#configure-and-deploy-http-header-validation-cdn-rule)

[如何清除CDN快取](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/caching/how-to/purge-cache)

[設定CDN錯誤頁面](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/content-delivery/custom-error-pages#cdn-error-pages)

[在CDN設定流量](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#client-side-redirectors)

[設定CDN認證和驗證](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-credentials-authentication)

