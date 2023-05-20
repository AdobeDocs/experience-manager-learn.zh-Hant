---
title: 在AEM Forms使用事務報告
description: 在AEM Forms，事務報表允許您保留自指定日期以來在AEM Forms部署中發生的所有事務的計數。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 36c38cb6-6f6a-4328-abf5-7a30059b66ce
last-substantial-update: 2019-03-20T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 0%

---

# 在AEM Forms使用事務報告{#using-transaction-reporting-in-aem-forms}

AEM Forms已與6.4.1一道提出交易報告，以捕獲提交表格的數量、使用檔案服務提供檔案和提供互動式通信（Web和打印渠道）。此功能主要針對希望根據提交的表單和/或提供的文檔數來許可軟體的客戶。 此功能目前僅在AEM FormsOSGI堆棧上可用。

## 啟用事務報告 {#enabling-transaction-reporting}

預設情況下禁用事務記錄。 要啟用事務記錄，請執行以下步驟：

* [開啟configMgr](http://localhost:4502/system/console/configMgr)
* 搜索「Forms事務報告」
* 選中「記錄事務」複選框
* 保存更改

啟用事務處理報告後，您可以提交自適應Forms、使用文檔服務生成文檔或呈現Interactive Communication文檔，以查看正在執行的事務處理報告。

## 查看事務處理報表 {#viewing-transaction-report}

要查看事務報告，請以管理員身份登錄AEM Forms。 只有fd-Administrator組的成員才能查看事務報表。

選擇工具 |Forms |查看事務報表

或通過按一下 [這裡](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)

![事務報告](assets/transactionreporting.gif)

在上面的「文檔處理」螢幕快照中是使用文檔服務生成的文檔數。 Documents reled是Interactive Communication文檔（Web和Print）呈現的數量。 Forms提交是自適應表單提交的數量。

某個事務在指定期間（刷新緩衝區時間+反向複製時間）內保留在緩衝區中。 預設情況下，事務計數在事務報表中反映大約需要90秒。

提交PDF表單、使用代理UI預覽互動式通信或使用非標準表單提交方法等操作不會作為事務處理入賬。 AEM Forms提供API來記錄此類交易。 從自定義實現調用API以記錄事務。

如果正在查看作者實例的事務報表，請確保在所有發佈實例上配置了反向複製。

要瞭解有關事務處理報告的詳細資訊，請 [請按一下這裡](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)
