---
title: 建立countries元件的結構
description: 在crx中建立元件結構
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
exl-id: 87e790c9-6ef6-4337-90b8-687ca576b21a
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 2%

---

# 建立countries元件的結構

登入您的AEM Forms執行個體，並依照以下步驟根據現成的下拉式元件建立新元件：

1. 導覽至CRXDE Lite中的「/apps/&lt;yourproject>/components/adaptiveForm/dropdown」。
2. 複製下拉式元件，並將其貼到相同的目錄層級。
3. 將複製的元件重新命名為國家/地區。
4. 將cq：template節點的jcr：title屬性更新為Countries。
5. 儲存變更。

您現在有名為「國家/地區」的新元件，這是現成可用下拉式元件的完整復本。 這是進一步自訂的基礎。

## 建立HTL檔案

若要建立Countries元件的HTL檔案：

1. 導覽至crx存放庫中的countries資料夾
2. 建立名為countries.html的新檔案。
3. 開啟crx存放庫中的/apps/core/fd/components/form/dropdown/v1/dropdown/dropdown.html檔案並複製其內容。
4. 將複製的內容貼入國家/地區.html。
5. 變更程式碼以使用新的Sling模型，如熒幕擷取畫面所示
6. 儲存您的變更。

![sling-model](assets/countriesdropdown.png)

最後，請將這些更新同步處理您的專案，以確保CRX存放庫中的變更會反映在您的AEM專案中。


## 後續步驟

[建立cq-dialog](./dialog.md)
