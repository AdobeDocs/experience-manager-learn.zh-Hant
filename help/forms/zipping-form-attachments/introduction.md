---
title: 發送自適應表單附件
description: 使用發送電子郵件元件發送自適應表單附件
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: bd9e1fc1-2fc7-452c-9a4a-2e16f6821760
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 2%

---

# 簡介



常見用例是使用工作流中的發送電子郵件元件來發送自適應表單AEM附件。
客戶通常會使用發送電子郵件元件將表單附件壓縮或將附件作為單個檔案發送。

## 在zip檔案中發送表單附件

為了完成使用情形，編寫了自定義工作流進程步驟。 在此自定義流程步驟中，在有效負載資料夾下建立並儲存了表單附件的zip檔案，該檔案名為 *zipped_attachments.zip*

![發送表單附件](assets/send-form-attachments.JPG)

## 單獨發送表單附件

為完成此使用情形，編寫了自定義工作流進程步驟。 在此自定義流程步驟中，我們將填充ArrayList of Documents和ArrayList of Strings類型的工作流變數。

![文檔發送清單](assets/send-list-of-documents.JPG)

## 後續步驟

[Zip表單附件](./custom-process-step.md)
