---
title: 疑難排解籤署多份檔案解決方案
description: 測試和疑難排解
feature: 適用性表單
version: 6.4,6.5
kt: 6960
thumbnail: 6960.jpg
topic: 開發
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '392'
ht-degree: 1%

---


# 測試和疑難排解


## 預覽再融資表單

當客戶服務代理填寫並提交[再融資表單](http://localhost:4502/content/dam/formsanddocuments/formsandsigndemo/refinanceform/jcr:content?wcmmode=disabled)時，就會觸發使用案例。

「簽署多個Forms」工作流程會在提交此表單時觸發，而客戶會收到包含連結的電子郵件通知，以開始表單填寫和簽署程式。

## 在套件中填寫表單

系統會呈現客戶以填寫並簽署套件中的第一個表單。 成功簽署表單時，客戶可導覽至套件中的下一個表單。 填寫並簽署所有表單後，客戶將看到「**AllDone**」表單。

## 疑難排解

### 未生成電子郵件通知

電子郵件通知由「簽署多個表單」工作流程中的「傳送電子郵件」元件發送。 如果此工作流程中的任何步驟失敗，則會傳送電子郵件通知。 請確保工作流中的自定義進程步驟正在MySQL資料庫中建立行。 如果正在建立列，請檢查您的Day CQ Mail Service組態設定

### 電子郵件通知中的連結無法運作

電子郵件通知中的連結會動態產生。 如果您的AEM伺服器未在localhost:4502上運行，請在「登錄多個Forms」工作流的「儲存Forms以簽名」步驟的參數中提供正確的伺服器名稱和埠

### 無法簽署表單

如果從資料來源擷取資料而未正確填入表單，就會發生此情況。 檢查伺服器的狀態日誌。 fetchformdata.jsp將一些有用的消息寫入stdout。

### 無法導航到包中的下一個表單

成功簽署套件中的表單時，會觸發更新簽名狀態工作流程。 工作流程的第一步會更新資料庫中表單的簽名狀態。 請檢查表單的狀態是否從0更新為1。

### 未看到AllDone表單

當包中沒有其他要登錄的表單時，AllDone表單將顯示給用戶。如果您沒有看到AllDone表單，請檢查作為&#x200B;**getnextform**&#x200B;客戶端庫一部分的GetNextFormToSign.js檔案第33行中使用的URL。











