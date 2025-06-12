---
title: 了解 DoS/DDoS 預防措施
description: 了解如何預防和減輕 DoS 與 DDoS 針對 AEM 的攻擊。
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
workflow-type: ht
source-wordcount: '370'
ht-degree: 100%

---

# 了解 AEM 的 DoS/DDoS 預防措施

了解可以預防和減輕 DoS 與 DDoS 針對 AEM 環境之攻擊的選項。在深入探討預防機制之前，先大致了解 [DoS](https://developer.mozilla.org/en-US/docs/Glossary/DOS_attack) 和 [DDoS](https://developer.mozilla.org/en-US/docs/Glossary/Distributed_Denial_of_Service)。

- DoS (阻斷服務) 和 DDoS (分散式阻斷服務) 攻擊都是企圖破壞目標伺服器、服務或網路之正常運作，使目標使用者無法存取的惡意行為。
- DoS 攻擊通常來自單一來源，而 DDoS 攻擊則來自多個來源。
- 由於結合多個攻擊裝置的資源，DDoS 攻擊的規模通常比 DoS 攻擊更大。
- 這些攻擊會向目標發送過多流量使其癱瘓，並惡意探索網路通訊協定中的安全漏洞。

以下表格說明如何預防和減輕 DoS 與 DDoS 攻擊：

<table>
    <tbody>
        <tr>
            <td><strong>預防機制</strong></td>
            <td><strong>說明</strong></td>
            <td><strong>AEM as a Cloud Service </strong></td>
            <td><strong>AEM 6.5 (AMS)</strong></td>
            <td><strong>AEM 6.5 (內部部署)</strong></td>
        </tr>
        <tr>
            <td>網頁應用程式防火牆 (WAF)</td>
            <td>一項安全性解決方案，為協助網頁應用程式抵禦各種類型的攻擊。</td>
            <td>
            <a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis#waf-rules" target="_blank">WAF-DDoS 防護授權</a></td>
            <td><a href="https://docs.aws.amazon.com/waf/" target="_blank">AWS</a> 或者透過 AMS 合約的 <a href="https://azure.microsoft.com/en-us/products/web-application-firewall" target="_blank">Azure</a> WAF。</td>
            <td>您偏好的 WAF</td>
        </tr>
        <tr>
            <td>ModSecurity</td>
            <td>ModSecurity (又稱「mod_security」Apache 模組) 是一個開放原始碼、跨平台的解決方案，可以保護網頁應用程式防禦各種各樣的攻擊。<br/>在 AEM as a Cloud Service 中，這項解決方案僅適用於 AEM Publish 服務，因為 AEM Author 服務前面沒有 Apache 網頁伺服器和 AEM Dispatcher。</td>
            <td colspan="3"><a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/foundation/security/modsecurity-crs-dos-attack-protection" target="_blank">啟用 ModSecurity </a></td>
        </tr>
        <tr>
            <td>流量篩選器規則</td>
            <td>可以使用流量篩選器規則來封鎖或允許 CDN 層的要求。</td>
            <td><a href="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis" target="_blank">流量篩選器規則範例</a></td>
            <td><a href="https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-type-rate-based.html" target="_blank">AWS</a> 或者 <a href="https://learn.microsoft.com/en-us/azure/web-application-firewall/ag/rate-limiting-overview" target="_blank">Azure</a> 規則限制功能。</td>
            <td>您偏好的解決方案</td>
        </tr>
    </tbody>
</table>

## 事件後分析與持續改進

要辨識和預防 DoS/DDoS 攻擊並沒有一體適用的標準流程，而且需仰賴您組織的安全性流程。**事件後分析與持續改進**&#x200B;是這個流程中極重要的一步。您可以考慮以下最佳實務：

- 透過事件後分析 (包括檢閱記錄、網路流量和系統設定) 找出 DoS/DDoS 攻擊的根本原因。
- 根據事件後分析的結果改善預防機制。

