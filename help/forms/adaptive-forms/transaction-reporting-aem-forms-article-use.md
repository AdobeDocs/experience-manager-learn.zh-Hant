---
title: 在AEM Forms中使用交易報表
seo-title: 在AEM Forms中使用交易報表
description: AEM Forms中的交易報表可讓您保留自AEM Forms部署上指定日期以來發生的所有交易計數。
seo-description: AEM Forms中的交易報表可讓您保留自AEM Forms部署上指定日期以來發生的所有交易計數。
uuid: e6133f7e-c79c-4006-89e7-3bebf7b8229e
feature: 適用性表單
topics: developing
audience: administrator
doc-type: article
activity: setup
version: 6.4.1,6.5
discoiquuid: 1abdf07a-b9f0-4c58-a1c6-08ae57db2014
topic: 開發
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 0%

---


# 在AEM Forms中使用交易報表{#using-transaction-reporting-in-aem-forms}

AEM Forms 6.4.1推出交易報告功能，用於擷取表單提交數量、使用檔案服務轉譯檔案以及轉譯互動式通訊（網頁和列印管道）。此功能主要適用於希望根據表單提交數量和/或檔案轉譯來授權軟體的客戶。 此功能目前僅可在AEM Forms OSGI堆疊上使用。

## 啟用事務報告{#enabling-transaction-reporting}

預設情況下，禁用事務記錄。 要啟用事務記錄，請執行以下步驟：

* [開啟configMgr](http://localhost:4502/system/console/configMgr)
* 搜尋「Forms Transaction Reporting」
* 選中「記錄事務」複選框
* 儲存您的變更

啟用交易報告後，您可以提交Adaptive Forms、使用文檔服務生成文檔或呈現Interactive Communication文檔，以查看交易報告的實際運行。

## 查看事務報表{#viewing-transaction-report}

若要檢視交易報表，請以管理員身分登入AEM Forms。 只有fd-Administrator組的成員才能查看事務報告。

選擇工具 | Forms |查看交易報告

或按一下[此處](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)檢視交易報表

![TransactionReporting](assets/transactionreporting.gif)

在上方的螢幕擷圖中， Document Processed是使用文檔服務生成的文檔數。 所呈現的文檔是所呈現的互動式通信文檔（Web和打印）的數量。 Forms Submitted是適用性表單提交的數量。

事務在指定期間（刷新緩衝時間+反向複製時間）保持在緩衝區中。 依預設，交易計數大約需要90秒才會反映在交易報表中。

提交PDF表單、使用代理UI預覽互動式通訊或使用非標準表單提交方法等動作不會計為交易。 AEM Forms提供API來記錄這類交易。 從您的自訂實作呼叫API以記錄交易。

如果您檢視的是製作例項的交易報表，請確定已在所有發佈例項上設定反向復寫。

要了解有關事務報告[的更多資訊，請按一下這裡](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)

