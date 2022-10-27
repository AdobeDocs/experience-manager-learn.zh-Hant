---
title: 傳送最適化表單附件
description: 使用傳送電子郵件元件傳送最適化表單附件
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: bd9e1fc1-2fc7-452c-9a4a-2e16f6821760
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---

# 簡介



常見的使用案例是在AEM工作流程中使用傳送電子郵件元件來傳送最適化表單附件。
客戶通常會使用傳送電子郵件元件，將表單附件壓縮或以個別檔案形式傳送附件。

## 以zip檔案傳送表單附件

為完成使用案例，編寫了自訂工作流程處理步驟。 在此自訂程式中，會將ZIP檔案（其中已建立表單附件，並儲存在名為的檔案的裝載資料夾下） *zipped_attachments.zip*

![send-form-attachments](assets/send-form-attachments.JPG)

## 個別傳送表單附件

為了完成此使用案例，編寫了自訂工作流程處理步驟。 在此自訂處理步驟中，我們會填入「檔案的ArrayList 」類型和「字串的ArrayList 」類型的工作流變數。

![send-list-of-documents](assets/send-list-of-documents.JPG)
