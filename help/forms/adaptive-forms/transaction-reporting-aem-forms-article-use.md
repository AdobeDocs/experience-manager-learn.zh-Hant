---
title: 在AEM Forms中使用交易報告
description: AEM Forms中的交易報告可讓您保留自AEM Forms部署上的指定日期以來發生的所有交易的計數。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 36c38cb6-6f6a-4328-abf5-7a30059b66ce
last-substantial-update: 2019-03-20T00:00:00Z
duration: 81
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '331'
ht-degree: 0%

---

# 在AEM Forms中使用交易報告{#using-transaction-reporting-in-aem-forms}

AEM Forms 6.4.1已引入交易報告功能，以擷取表單提交次數、使用檔案服務轉譯檔案以及轉譯互動式通訊（網頁和列印管道）。此功能目前僅適用於AEM Forms OSGI棧疊。

## 啟用交易報告 {#enabling-transaction-reporting}

依預設，會停用交易記錄。 若要啟用交易記錄，請遵循下列步驟：

* [開啟configMgr](http://localhost:4502/system/console/configMgr)
* 搜尋「Forms交易報告」
* 選取「記錄交易」核取方塊
* 儲存您的變更

啟用交易報告後，您可以提交Adaptive Forms、使用檔案服務產生檔案或轉譯Interactive Communication檔案以檢視交易報告的運作情況。

## 檢視交易報表 {#viewing-transaction-report}

若要檢視交易報告，請以管理員身分登入AEM Forms。 只有fd-Administrator群組的成員可以檢視交易報告。

選取工具 | Forms | 檢視交易報告

或按一下以檢視交易報告 [此處](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)

![交易報告](assets/transactionreporting.gif)

在上方熒幕擷圖中，Document Processed是使用檔案服務產生的檔案數。 Documents rended是已轉譯的互動式通訊檔案（網頁與列印）數目。 Forms已提交為最適化表單提交數。

交易在緩衝區中保留指定的期間（排清緩衝區時間+反向復寫時間）。 依預設，交易計數大約需要90秒才會反映在交易報表中。

提交PDF表單、使用代理程式UI預覽互動式通訊或使用非標準表單提交方法等動作不會計為交易。 AEM Forms提供API來記錄這類交易。 從您的自訂實作中呼叫API以記錄交易。

如果您在製作執行個體上檢視交易報告，請確定已在所有發佈執行個體上設定反向復寫。

若要進一步瞭解交易報告 [請按這裡](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)
