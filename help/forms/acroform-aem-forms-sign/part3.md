---
title: Acroforms與AEM Forms
seo-title: 將最適化表單資料與Acroform合併
description: 整合Acroforms與AEM Forms的教學課程第3部分。 在您的系統上測試工作流程和最適化表單。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '236'
ht-degree: 1%

---


# 在您的系統上測試此功能

[下載此套件並將它匯入](assets/acro-form-aem-form.zip)
AEMThis套件包含範例工作流程和html頁面，可讓您從上傳的Acroform建立架構。

## 設定工作流程

1. [在編輯模式下開啟「工作流模型」](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html)。
2. 開啟MergeAcroformData步驟的設定屬性。
3. 按一下「流程」(Process)頁籤。
4. 請確定您所傳遞的引數是伺服器上的有效資料夾。
5. 儲存變更。

## 建立最適化表單

1. 使用在前面步驟中建立的模式建立最適化表單。
2. 將一些架構元素拖放到最適化表單。
3. 設定最適化表單的提交動作，以提交至AEM工作流程(MergeAcroformData)。
4. **請確定您將資料檔案路徑指定為「Data.xml」。當范常式式碼在工作流程裝載中尋找名為Data.xml的檔案時，這一點非常重要。**
5. 預覽最適化表單、填寫表單並送出。
6. 您應看到PDF，並將資料合併儲存至設定工作流程中步驟4中指定的檔案夾

>[!NOTE]
>
>將資料與acroform合併所產生的pdf會儲存為工作流程的裝載資料夾下的pdfdocument.pdf。 然後，本檔案可做為工作流程的一部分，用於進一步處理
