---
title: 疑難排解籤署多份檔案解決方案
description: 測試和故障拍攝解決方案
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6960
thumbnail: 6960.jpg
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 1%

---


# 測試和疑難排解


## 預覽再融資表單

當客戶服務代理填寫並提交[再融資表單](http://localhost:4502/content/dam/formsanddocuments/formsandsigndemo/refinanceform/jcr:content?wcmmode=disabled)時，會觸發使用案例。

「簽署多份Forms」工作流程會觸發提交此表單的程式，而客戶會收到電子郵件通知，並附上啟動表單填寫和簽署程式的連結。

## 填寫套件中的表格

客戶將會填寫並簽署套件中的第一個表單。 成功簽署表單後，客戶可導覽至套件中的下一個表單。 填寫所有表格並簽署後，客戶就會看到「**AllDone**」表格。

## 疑難排解

### 未產生電子郵件通知

電子郵件通知是由「簽署多個表單」工作流程中的「傳送電子郵件」元件所傳送。 如果此工作流程中的任何步驟失敗，將會傳送電子郵件通知。 請確定工作流中的自定義進程步驟正在MySQL資料庫中建立行。 如果正在建立行，則檢查您的Day CQ Mail Service配置設定

### 電子郵件通知中的連結無法運作

電子郵件通知中的連結會動態產生。 如果您AEM的伺服器未在localhost:4502上運行，請在「簽署多個Forms」工作流的「儲存Forms要簽署」步驟的參數中提供正確的伺服器名稱和埠

### 無法簽署表格

如果表單未從資料來源擷取資料而正確填入，就會發生這種情況。 檢查伺服器的stdout日誌。 fetchformdata.jsp會將一些有用的消息寫入stdout。

### 無法導航到包中的下一個表單

在套件中成功簽署表格時，會觸發「更新簽名狀態」工作流程。 工作流程的第一步會更新資料庫中表單的簽名狀態。 請檢查表單的狀態是否從0更新為1。

### 看不到AllDone表格

當檔案包中沒有其他可登錄的表單時，AllDone表單會顯示給用戶。如果您沒有看到AllDone表單，請檢查GetNextFormToSign.js檔案第33行中使用的URL，該檔案是&#x200B;**getnextform**&#x200B;客戶端庫的一部分。











