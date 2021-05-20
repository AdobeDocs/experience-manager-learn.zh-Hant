---
title: 了解AEM中的驗證支援
description: 'AEM支援的驗證（有時也是授權）機制的整合檢視。 '
version: 6.3, 6.4, 6.5
feature: 使用者和群組
topics: authentication, security
activity: understand
audience: architect, developer, implementer
doc-type: article
kt: 406
topic: 架構
role: Architect
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 5%

---


# 了解AEM 6.x中的驗證支援

AEM支援的驗證（有時也是授權）機制的整合檢視。

*下表說明使用者如何驗證至AEM。*

<table>
    <tbody>
        <tr>
            <td><strong>驗證</strong></td>
            <td><strong>AEM 6.3</strong></td>
            <td><strong>AEM 6.4</strong></td>
            <td><strong>AEM 6.5</strong></td>
        </tr>
        <tr>
            <td><strong>AEM作為標準身分提供者</strong></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>基本驗證</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>Forms</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>以代號為基礎（w/ <a href="https://docs.adobe.com/content/help/en/experience-manager-65/administering/security/encapsulated-token.html" target="_blank">封裝代號</a>）</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>非AEM系統作為標準身份提供者</strong></td>
            <td></td>
            <td></td>
            <td></td>
            <tr>
                <td><a href="https://docs.adobe.com/content/help/en/experience-manager-65/administering/security/ldap-config.html" target="_blank">LDAP</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://docs.adobe.com/content/help/en/experience-manager-65/deploying/configuring/single-sign-on.html" target="_blank">SSO</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://docs.adobe.com/content/help/en/experience-manager-65/administering/security/saml-2-0-authenticationhandler.html" target="_blank">SAML 2.0</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-oauth-server-functionality-in-aem.html" target="_blank">OAuth 1.0a和2.0</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://sling.apache.org/documentation/the-sling-engine/authentication/authentication-authenticationhandler/openid-authenticationhandler.html" target="_blank">OpenID</a></td>
                <td>⁕</td>
                <td>⁕</td>
                <td>*</td>
            </tr>
    </tbody>
</table>

⁕ *透過社群專案提供，但不直接受Adobe支援。*
