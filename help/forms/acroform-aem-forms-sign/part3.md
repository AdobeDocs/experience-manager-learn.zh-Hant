---
title: Acroforms與AEM Forms
seo-title: 將最適化表單資料與Acroform合併
description: 整合Acroforms與AEM Forms的教學課程第3部分。 在您的系統上測試工作流程和最適化表單。
feature: 適用性表單
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 1%

---


# 在您的系統上測試此功能

[將此套件下載並匯入](assets/acro-form-aem-form.zip)
AEMT此套件包含範例工作流程和html頁面，可讓您從上傳的Acroform建立架構。

## 設定工作流程

1. [在編輯模式中開啟「工作流模型」](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html)。
2. 開啟MergeAcroformData步驟的配置屬性。
3. 按一下「進程」(Process)頁簽。
4. 請確定您傳遞的引數是伺服器上的有效資料夾。
5. 儲存變更。

## 建立最適化表單

1. 使用先前步驟中建立的結構建立適用性表單。
2. 將幾個結構元素拖放至最適化表單。
3. 設定最適化表單的提交動作，以提交至AEM工作流程(MergeAcroformData)。
4. **請務必將資料檔案路徑指定為「Data.xml」。這非常重要，因為示例代碼在工作流裝載中查找名為Data.xml的檔案。**
5. 預覽最適化表單、填寫表單並提交。
6. 您應該會在設定工作流程中看到資料合併至步驟4中指定之資料夾的PDF

>[!NOTE]
>
>將資料與Acroform合併後產生的PDF會儲存為工作流程裝載資料夾下的pdfdocument.pdf。 然後，此檔案便可作為工作流程的一部分，用於進一步處理
