---
title: AEM安全性通知（2018年11月）
seo-title: AEM安全性通知（2018年11月）
description: AEMExperience Manager安全性通知Dispatcher
seo-description: AEMExperience Manager安全性通知Dispatcher
version: 6.4
feature: Dispatcher
topics: security
activity: understand
audience: all
doc-type: article
uuid: 3ccf7323-4061-49d7-ae95-eb003099fd77
discoiquuid: 9d181b3e-fbd5-476d-9e97-4452176e495c
topic: 安全性
role: Architect
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '440'
ht-degree: 8%

---


# AEM安全性通知（2018年11月）

## 摘要

本文探討最近在AEM中報告的幾項最新和舊的弱點。 請注意，大部分已識別的弱點都是AEM產品的已知問題，且已先前識別，且有新的Dispatcher版本可供新弱點使用。 Adobe也敦促客戶完成[AEM安全性檢查清單](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html)並遵循相關准則。

## 需要動作

* AEM部署應該會開始使用最新的Dispatcher版本。
* 必須依據建議的設定，套用Dispatcher安全性規則。
* 應針對AEM部署完成[AEM安全性檢查清單](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html)。

## 漏洞和解決方法

| 問題 | 解析度 | 連結 |
|-------|------------|-------|
| 略過AEM Dispatcher規則 | 安裝最新版的Dispatcher(4.3.1)，並遵循建議的Dispatcher設定。 | 請參閱[AEM Dispatcher發行說明](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)和[設定Dispatcher](https://helpx.adobe.com/tw/experience-manager/dispatcher/using/dispatcher-configuration.html)。 |
| URL篩選器略過漏洞，可能用來規避Dispatcher規則 — CVE-2016-0957 | 這在舊版Dispatcher中已修正，但現在建議您安裝最新版Dispatcher(4.3.1)，並遵循建議的Dispatcher設定。 | 請參閱[AEM Dispatcher發行說明](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)和[設定Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html)。 |
| 與儲存的SWF檔案相關的XSS漏洞 | 此問題已透過先前發行的安全性修正解決。 | 請參閱[AEM安全性公告APSB18-10](https://helpx.adobe.com/security/products/experience-manager/apsb18-10.html)。 |
| 密碼相關漏洞 | 請遵循安全性檢查清單中的建議，以取得更強密碼。 | 請參閱[AEM安全性檢查清單](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) |
| 匿名用戶的磁碟使用暴露 | AEM 6.1和更新版本已解決此問題，對於AEM 6.0，可以修改現成權限，使其限制性更強。 | 請參閱AEM 6.1及更舊版本的[發行說明](https://helpx.adobe.com/tw/experience-manager/aem-previous-versions.html)。 |
| 匿名用戶的開放社交代理暴露 | 在6.0 SP2以來的版本中已解決此問題。 | 請參閱[AEM 6.1及更舊版本的發行說明](https://helpx.adobe.com/experience-manager/aem-previous-versions.html) 。 |
| 生產執行個體上的CRX Explorer Access | 「安全性檢查清單」中已涵蓋管理CRX Explorer存取，應從生產製作和發佈中移除CRX Explorer，若未移除，安全性健康狀況檢查會報告它。 | 請參閱[AEM安全性檢查清單](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security-checklist.html)。 |
| BGServlets公開 | 自AEM 6.2起已解決此問題。 | 請參閱[AEM 6.2發行說明](https://helpx.adobe.com/tw/experience-manager/6-2/release-notes.html) |

>[!MORELIKETHIS]
>
>* [AEM Dispatcher使用手冊](https://helpx.adobe.com/experience-manager/dispatcher/user-guide.html)
>* [AEM Dispatcher 發行說明](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [AEM安全性佈告欄](https://helpx.adobe.com/security.html#experience-manager)

