---
title: 從AEM工作流程中的檔案清單擷取檔案
description: 從檔案清單中擷取特定檔案的自訂工作流程元件
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-13918
last-substantial-update: 2023-09-12T00:00:00Z
exl-id: b0baac71-3074-49d5-9686-c9955b096abb
duration: 63
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 0%

---

# 從檔案清單擷取檔案

常見的使用案例是使用AEM工作流程中的叫用表單資料模型步驟，將表單資料和表單附件提交至外部系統。 例如，在ServiceNow中建立案例時，您會想要使用支援檔案提交案例詳細資訊。 新增至最適化表單的附件會儲存在檔案陣列清單型別的變數中，若要從此陣列清單擷取特定檔案，您必須編寫自訂程式碼。

本文將逐步引導您使用自訂工作流程元件，以擷取檔案並將其儲存在檔案變數中。

## 建立工作流程

需要建立工作流程來處理表單提交。 工作流程需要定義下列變數

* 型別為ArrayList of Document的變數（此變數會儲存使用者新增的表單附件）
* 型別為「檔案」的變數。（此變數會保留從ArrayList擷取的檔案）

* 新增自訂元件至您的工作流程並設定其屬性
  ![extract-item-workflow](assets/extract-document-array-list.png)

## 設定最適化表單

* 設定最適化表單的提交動作以觸發AEM工作流程
  ![submit-action](assets/store-attachments.png)

## 測試解決方案

[使用OSGi Web主控台部署自訂套件組合](assets/ExtractItemsFromArray.core-1.0.0-SNAPSHOT.jar)

[使用封裝管理員匯入工作流程元件](assets/Extract-item-from-documents-list.zip)

[匯入範例工作流程](assets/extract-item-sample-workflow.zip)

[匯入最適化表單](assets/test-attachment-extractions-adaptive-form.zip)

[預覽表單](http://localhost:4502/content/dam/formsanddocuments/testattachmentsextractions/jcr:content?wcmmode=disabled)

將附件新增至表單並提交它。

>[!NOTE]
>
>提取的檔案隨後可用於任何其他工作流程步驟，例如傳送電子郵件或叫用FDM步驟
