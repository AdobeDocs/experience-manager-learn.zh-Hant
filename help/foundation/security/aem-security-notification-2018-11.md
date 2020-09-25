---
title: AEM安全性通知（2018年11月）
seo-title: AEM安全性通知（2018年11月）
description: AEM Experience Manager Security Notification Dispatcher
seo-description: AEM Experience Manager Security Notification Dispatcher
version: 6.4
feature: dispatcher
topics: security
activity: understand
audience: all
doc-type: article
uuid: 3ccf7323-4061-49d7-ae95-eb003099fd77
discoiquuid: 9d181b3e-fbd5-476d-9e97-4452176e495c
translation-type: tm+mt
source-git-commit: 1e615d1c51fa0c4c0db335607c29a8c284874c8d
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 5%

---


# AEM安全性通知（2018年11月）

## 摘要

本文說明最近在AEM中報告的幾項最新和舊有弱點。 請注意，大部分已識別的弱點都是AEM產品的已知問題，而且先前已發現緩解弱點，新的Dispatcher版本已可供新弱點使用。 Adobe也敦促客戶完成 [AEM安全性檢查清單](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) ，並遵循相關准則。

## 需要動作

* AEM部署應開始使用最新的Dispatcher版本。
* 必須按照建議的配置應用分發程式安全規則。
* AEM部 [署的AEM安全性檢查清單](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) ，應已完成。

## 弱點和解決方案

| 問題 | 解析度 | 連結 |
|-------|------------|-------|
| 略過AEM Dispatcher規則 | 安裝最新版Dispatcher(4.3.1)，並遵循建議的Dispatcher設定。 | 請參 [閱「AEM Dispatcher發行說明](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) 」 [和「設定Dispatcher](https://helpx.adobe.com/tw/experience-manager/dispatcher/using/dispatcher-configuration.html)」。 |
| URL篩選略過弱點，此弱點可用來規避分派器規則- CVE-2016-0957 | 這在舊版Dispatcher中已修正，但現在建議您安裝最新版Dispatcher(4.3.1)，並遵循建議的Dispatcher設定。 | 請參 [閱「AEM Dispatcher發行說明](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) 」 [和「設定Dispatcher](https://helpx.adobe.com/tw/experience-manager/dispatcher/using/dispatcher-configuration.html)」。 |
| 與儲存的SWF檔案相關的XSS弱點 | 此問題已透過先前發佈的安全性修正解決。 | 請參閱 [AEM安全性公告APSB18-10](https://helpx.adobe.com/security/products/experience-manager/apsb18-10.html)。 |
| 密碼相關漏洞 | 請依照安全性檢查清單中的建議，取得更強大的密碼。 | 請參閱 [AEM安全性檢查清單](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) |
| 匿名使用者的磁碟使用曝光度 | AEM 6.1和更新版本已解決此問題，對於AEM 6.0，現成權限可修改為限制更嚴格。 | 請參 [閱](https://helpx.adobe.com/experience-manager/aem-previous-versions.html)AEM 6.1及更舊版本的發行說明。 |
| 為匿名使用者開啟Social Proxy的曝光 | 6.0 SP2版本已解決此問題。 | 請參 [閱](https://helpx.adobe.com/experience-manager/aem-previous-versions.html) AEM 6.1及更舊版本的發行說明。 |
| 生產實例上的CRX Explorer訪問 | 安全性檢查清單中已涵蓋管理CRX檔案總管存取權限，CRX檔案總管應從生產作者中移除並發佈，如果未移除，安全性狀況檢查會報告它。 | 請參閱 [AEM安全性檢查清單](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security-checklist.html)。 |
| BGServlets已公開 | 自AEM 6.2以來，這個問題已經解決。 | 請參閱 [AEM 6.2發行說明](https://helpx.adobe.com/tw/experience-manager/6-2/release-notes.html) |

>[!MORELIKETHIS]
>
>* [AEM Dispatcher使用指南](https://helpx.adobe.com/experience-manager/dispatcher/user-guide.html)
>* [AEM Dispatcher 發行說明](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [AEM安全性公告](https://helpx.adobe.com/security.html#experience-manager)

