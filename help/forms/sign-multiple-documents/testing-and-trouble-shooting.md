---
title: 對簽名多個文檔解決方案進行故障排除
description: Test和麻煩解決方案
feature: Adaptive Forms
version: 6.4,6.5
kt: 6960
thumbnail: 6960.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 99cba29e-4ae3-4160-a4c7-a5b6579618c0
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 0%

---

# Test和故障排除


## 預覽再融資窗體

當客戶服務代理填寫和提交時，將觸發用例 [再融資](http://localhost:4502/content/dam/formsanddocuments/formsandsigndemo/refinanceform/jcr:content?wcmmode=disabled)。

「簽名多個Forms」工作流將獲取此表單提交的觸發器，客戶將獲取帶連結的電子郵件通知，以啟動表單填寫和簽名過程。

## 填寫包中的表單

客戶將填寫並簽署包中的第一張表單。 成功簽署表單後，客戶可以導航到包中的下一個表單。 填寫所有表單並簽名後，客戶將顯示「 」**全部完成**&#x200B;的子菜單。

## 疑難排解

### 未生成電子郵件通知

電子郵件通知由「簽名多表單」工作流中的「發送電子郵件」元件發送。 如果此工作流中的任何步驟失敗，則會發送電子郵件通知。 確保工作流中的自定義進程步驟正在MySQL資料庫中建立行。 如果正在建立行，則檢查您的Day CQ Mail Service配置設定

### 電子郵件通知中的連結無效

動態生成電子郵件通知中的連結。 如果服AEM務器未在localhost:4502上運行，請在「對多個Forms進行簽名」工作流的「儲存Forms要簽名」步驟的參數中提供正確的伺服器名稱和埠

### 無法對表單簽名

如果從資料源讀取資料而未正確填充表單，則會發生這種情況。 檢查伺服器的停止日誌。 fetchformdata.jsp將一些有用的消息寫入stdout。

### 無法導航到包中的下一個表單

在成功簽署包中的表單時，將觸發「更新簽名狀態」工作流。 工作流中的第一步更新資料庫中表單的簽名狀態。 請檢查表單的狀態是否從0更新為1。

### 未查看AllDone窗體

當包中沒有其他可登錄的表單時，AllDone表單將呈現給用戶。如果您沒有看到AllDone表單，請檢查GetNextFormToSign.js檔案(該檔案是 **getnexform** 客戶端庫。
