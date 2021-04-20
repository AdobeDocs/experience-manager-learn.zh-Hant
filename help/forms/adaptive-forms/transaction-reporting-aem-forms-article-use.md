---
title: 在AEM Forms使用事務報告
seo-title: 在AEM Forms使用事務報告
description: 在AEM Forms的事務報表允許您記錄自指定日期以來在AEM Forms部署中發生的所有事務。
seo-description: 在AEM Forms的事務報表允許您記錄自指定日期以來在AEM Forms部署中發生的所有事務。
uuid: e6133f7e-c79c-4006-89e7-3bebf7b8229e
feature: Adaptive Forms
topics: developing
audience: administrator
doc-type: article
activity: setup
version: 6.4.1,6.5
discoiquuid: 1abdf07a-b9f0-4c58-a1c6-08ae57db2014
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '403'
ht-degree: 0%

---


# 在AEM Forms使用事務報告{#using-transaction-reporting-in-aem-forms}

AEM Forms6.4.1推出交易報告功能，以擷取表單提交數量、使用檔案服務轉換檔案以及互動式通訊（網路和印刷通道）。這項功能主要適用於想要根據提交表單和／或所提供檔案數量來授權軟體的客戶。 此功能目前僅在AEM FormsOSGI堆棧上提供。

## 啟用事務報告{#enabling-transaction-reporting}

預設情況下會禁用事務記錄。 若要啟用交易記錄，請遵循下列步驟：

* [開啟configMgr](http://localhost:4502/system/console/configMgr)
* 搜索「Forms交易報告」
* 選中「記錄事務」複選框
* 儲存變更

在啟用交易報表後，您就可以提交AdaptiveForms、使用檔案服務產生檔案，或轉譯Interactive Communication檔案，以檢視交易報表的實際運作。

## 查看事務報表{#viewing-transaction-report}

若要檢視交易報表，請以管理員身分登入AEM Forms。 只有fd-Administrator組的成員才能查看事務報告。

選擇工具 |Forms |檢視交易報表

或按一下[此處](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)檢視交易報表

![TransactionReporting](assets/transactionreporting.gif)

在上述的「檔案處理」螢幕擷取中，是使用檔案服務產生的檔案數。 轉譯的檔案是轉譯的互動式通訊檔案（網頁和列印）數目。 Forms提交的是最適化表單提交的數量。

事務在指定期間（刷新緩衝時間+反向複製時間）內保留在緩衝區中。 依預設，交易計數大約需要90秒才能反映在交易報表中。

提交PDF表單、使用代理UI預覽互動式通訊或使用非標準表單提交方法等動作不會視為交易。 AEM Forms提供API來記錄此類交易。 從您的自訂實作呼叫API以記錄交易。

如果您正在查看作者實例的事務報告，請確保在所有發佈實例上配置了反向複製。

若要進一步瞭解交易報表[，請按一下此處](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)

