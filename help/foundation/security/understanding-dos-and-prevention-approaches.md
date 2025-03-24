---
title: 瞭解DoS/DDoS預防
description: 瞭解如何防止及緩解針對AEM的DoS和DDoS攻擊。
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Security
topic: Security, Development
role: Admin, Architect, Developer
level: Beginner
doc-type: Article
duration: 75
last-substantial-update: 2024-03-30T00:00:00Z
jira: KT-15219
exl-id: 1d7dd829-e235-4884-a13f-b6ea8f6b4b0b
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 1%

---

# 瞭解AEM中的DoS/DDoS預防

瞭解可用於防止和減少AEM環境上的DoS和DDoS攻擊的選項。 在深入瞭解預防機制之前，請先簡要概述[DoS](https://developer.mozilla.org/en-US/docs/Glossary/DOS_attack)和[DoS](https://developer.mozilla.org/en-US/docs/Glossary/Distributed_Denial_of_Service)。

- DoS （拒絕服務）和DDoS （分散式拒絕服務）攻擊都是惡意嘗試破壞目標伺服器、服務或網路的正常運作，使其目標使用者無法存取。
- DoS攻擊通常來自單一來源，而DDoS攻擊來自多個來源。
- DDoS攻擊通常比DoS攻擊規模更大，因為多個攻擊裝置的資源合併在一起。
- 這些攻擊是透過以過多的流量淹沒目標來執行，並利用網路通訊協定中的漏洞。

下表說明如何防止和減少DoS和DDoS攻擊：

<table>
    <tbody>
        <tr>
            <td><strong>預防機制</strong></td>
            <td><strong>說明</strong></td>
            <td><strong>AEM as a Cloud Service </strong></td>
            <td><strong>AEM 6.5 (AMS)</strong></td>
            <td><strong>AEM 6.5 （內部部署）</strong></td>
        </tr>
        <tr>
            <td>Web應用程式防火牆(WAF)</td>
            <td>一種安全性解決方案，專為保護Web應用程式免受各種型別的攻擊而設計。</td>
            <td>
            <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis#waf-rules" target="_blank">WAF-DDoS保護授權</a></td>
            <td>透過AMS合約<a href="https://docs.aws.amazon.com/waf/" target="_blank">AWS</a>或<a href="https://azure.microsoft.com/en-us/products/web-application-firewall" target="_blank">Azure</a> WAF。</td>
            <td>您偏好的WAF</td>
        </tr>
        <tr>
            <td>ModSecurity</td>
            <td>ModSecurity （亦稱為'mod_security' Apache模組）是開放原始碼跨平台解決方案，可抵禦Web應用程式的一系列攻擊。<br/>在AEM as a Cloud Service中，這僅適用於AEM Publish服務，因為AEM Author服務前面沒有Apache Web Server和AEM Dispatcher。</td>
            <td colspan="3"><a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/foundation/security/modsecurity-crs-dos-attack-protection" target="_blank">啟用Modsecurity </a></td>
        </tr>
        <tr>
            <td>流量篩選器規則</td>
            <td>流量篩選規則可以用來在CDN層封鎖或允許請求。</td>
            <td><a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis" target="_blank">流量篩選規則範例</a></td>
            <td><a href="https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-type-rate-based.html" target="_blank">AWS</a>或<a href="https://learn.microsoft.com/en-us/azure/web-application-firewall/ag/rate-limiting-overview" target="_blank">Azure</a>規則限制功能。</td>
            <td>您偏好的解決方案</td>
        </tr>
    </tbody>
</table>

## 事後分析和持續改善

雖然在識別和防範DoS/DDoS攻擊方面並沒有萬能的標準流程，這取決於貴組織的安全程式。 **事件後分析和持續改善**&#x200B;是這個過程中的關鍵步驟。 以下是一些需要考量的最佳實務：

- 透過進行事件後分析（包括檢閱記錄、網路流量和系統設定），找出DoS/DDoS攻擊的根本原因。
- 根據事件後分析的發現，改善預防機制。

