---
title: 傳送最適化表單附件
description: 使用傳送電子郵件元件傳送最適化表單附件
feature: 適用性表單
version: 6.5
topic: 開發
role: Developer
level: Beginner
kt: kt-8049
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 2%

---


# 簡介



常見的使用案例是在AEM工作流程中使用傳送電子郵件元件來傳送最適化表單附件。
客戶通常會使用傳送電子郵件元件，將表單附件壓縮或以個別檔案形式傳送附件。

## 以zip檔案傳送表單附件

為完成使用案例，編寫了自訂工作流程處理步驟。 在此自訂程式步驟中，會建立一個zip檔案，其中包含表單附件，並儲存在&#x200B;*zipped_attachments.zip*&#x200B;檔案的裝載資料夾下

![send-form-attachments](assets/send-form-attachments.JPG)

## 個別傳送表單附件

為了完成此使用案例，編寫了自訂工作流程處理步驟。 在此自訂處理步驟中，我們會填入「檔案的ArrayList 」類型和「字串的ArrayList 」類型的工作流變數。

![send-list-of-documents](assets/send-list-of-documents.JPG)



