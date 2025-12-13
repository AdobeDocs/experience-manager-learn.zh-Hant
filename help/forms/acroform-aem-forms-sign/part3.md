---
title: 使用AEM Forms的Acroforms
description: 整合Acroforms與AEM Forms的教學課程的第3部分。 在您的系統上測試工作流程與最適化表單。
feature: Adaptive Forms
doc-type: Tutorial
version: Experience Manager 6.5
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 45
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 3%

---


# 在您的系統上測試此功能

[下載此套件並將其匯入AEM](assets/acro-form-aem-form.zip)
此套件包含範例工作流程和html頁面，可讓您從上傳的Acroform建立結構。

## 設定工作流程

1. [在編輯模式中開啟工作流程模型](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html)。
2. 開啟MergeAcroformData步驟的設定屬性。
3. 按一下「處理」標籤。
4. 請確定您傳送的引數是伺服器上的有效資料夾。
5. 儲存變更。

## 建立自適應表單

1. 使用先前步驟建立的結構描述建立調適型表單。
2. 將一些結構描述元素拖放到最適化表單上。
3. 設定最適化表單的提交動作，以提交至AEM工作流程(MergeAcroformData)。
4. **請確定您指定資料檔路徑為「Data.xml」。 這非常重要，因為範常式式碼會在工作流程裝載中尋找名為Data.xml的檔案。**
5. 預覽最適化表單、填寫表單並提交。
6. 您應該會看到的PDF中，資料已合併儲存至設定工作流程步驟4中指定的資料夾

>[!NOTE]
>
>將資料與Acroform合併而產生的PDF會儲存為pdfdocument.pdf，位於工作流程的裝載資料夾下。 然後，此檔案可用於作為工作流程的一部分進行進一步處理
