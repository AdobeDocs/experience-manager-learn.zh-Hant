---
title: 安AEM全通知（2018年11月）
seo-title: AEM Security Notification (November 2018)
description: AEMExperience Manager安全通知調度程式
seo-description: AEM Experience Manager Security Notification Dispatcher
version: 6.4
feature: Dispatcher
topics: security
activity: understand
audience: all
doc-type: article
uuid: 3ccf7323-4061-49d7-ae95-eb003099fd77
discoiquuid: 9d181b3e-fbd5-476d-9e97-4452176e495c
topic: Security
role: Architect
level: Beginner
exl-id: 1ee11a01-9e49-42f4-aec4-09e51f769f69
source-git-commit: 4b47daf82e27f6bea4be30e3cdd132f497f4c609
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 14%

---

# 安AEM全通知（2018年11月）

## 摘要

本文針對最近在中報告的幾個最新和舊的漏洞進AEM行。 請注意，大多數已識別的漏洞是產品的已知問題AEM，並且以前已識別到緩解問題，新的分發程式版本可用於新的漏洞。 Adobe還敦促客戶完成 [安AEM全核對表](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) 並遵循相關指引。

## 需要動作

* 部AEM署應開始使用最新的Dispatcher版本。
* 必鬚根據建議的配置應用調度程式安全規則。
* 的 [安AEM全核對表](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) 應完成部AEM署。

## 漏洞和解決方案

| 問題 | 解析度 | 連結 |
|-------|------------|-------|
| 繞過AEMDispatcher規則 | 安裝最新版本的Dispatcher(4.3.1)，並遵循建議的調度程式配置。 | 請參閱 [《 DispatcherAEM發佈說明》](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) 和 [配置Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html)。 |
| URL篩選器繞過漏洞，可用於規避調度程式規則 — CVE-2016-0957 | 這是在較舊版本的Dispatcher中修復的，但現在建議您安裝最新版本的Dispatcher(4.3.1)並遵循建議的Dispatcher配置。 | 請參閱 [《 DispatcherAEM發佈說明》](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) 和 [配置Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html)。 |
| 與儲存的SWF檔案相關的XSS漏洞 | 此問題已通過先前發佈的安全修復解決。 | 請參閱 [安AEM全公告APSB18-10](https://helpx.adobe.com/security/products/experience-manager/apsb18-10.html)。 |
| 與密碼相關的漏洞 | 按照安全檢查表中的建議獲取更強的密碼。 | 請參閱 [安AEM全核對表](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) |
| 匿名用戶的磁碟使用暴露 | 此問題已在6.1和更AEM高版本中解決，AEM對於6.0，現成權限可修改為限制性更強。 | 請參閱 [發行說明](https://helpx.adobe.com/tw/experience-manager/aem-previous-versions.html)6AEM.1及更高版本。 |
| 匿名用戶開放社交代理的暴露 | 在從6.0 SP2開始的版本中已解決此問題。 | 請參閱 [發行說明](https://helpx.adobe.com/tw/experience-manager/aem-previous-versions.html) 6AEM.1及更高版本。 |
| 生產實例上的CRX資源管理器訪問 | 管理CRX資源管理器訪問已在安全檢查表中涉及，應從生產作者中刪除CRX資源管理器並發佈，如果未刪除，安全運行狀況檢查將報告它。 | 請參閱 [安AEM全核對表](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security-checklist.html)。 |
| BGServlet已公開 | 自6.2以AEM來已解決。 | 請參閱 [AEM6.2發行說明](https://helpx.adobe.com/tw/experience-manager/6-2/release-notes.html) |

>[!MORELIKETHIS]
>
>* [《 DispatcherAEM使用手冊》](https://helpx.adobe.com/experience-manager/dispatcher/user-guide.html)
>* [AEM Dispatcher 發行說明](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [安AEM全公告](https://helpx.adobe.com/security.html#experience-manager)

