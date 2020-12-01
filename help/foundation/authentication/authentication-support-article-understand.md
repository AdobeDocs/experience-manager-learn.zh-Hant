---
title: 瞭解AEM中的驗證支援
description: 'AEM支援的驗證（有時也會授權）機制的整合檢視。 '
version: 6.3, 6.4, 6.5
feature: authentication
topics: authentication, security
activity: understand
audience: architect, developer, implementer
doc-type: article
kt: 406
translation-type: tm+mt
source-git-commit: 703321483454435e33c0aa26d66110271fc095d8
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---


# 瞭解AEM 6.x的驗證支援

AEM支援的驗證（有時也會授權）機制的整合檢視。

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
            <td><strong>AEM做為標準身分提供者</strong></td>
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
            <td>表單型</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>基於令牌（帶<a href="https://docs.adobe.com/content/help/en/experience-manager-65/administering/security/encapsulated-token.html" target="_blank">封裝令牌</a>）</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>非AEM系統做為標準身分提供者</strong></td>
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

&lt;a0/⁕>透過社群專案提供，但Adobe不直接支援。**
