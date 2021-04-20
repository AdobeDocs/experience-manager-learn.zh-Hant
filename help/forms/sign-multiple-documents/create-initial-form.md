---
title: 建立初始表單以觸發流程
description: 建立初始表單以觸發電子郵件通知，以開始簽署程式。
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6892
thumbnail: 6892.jpg
topic: Development
role: Business Practitioner
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 5%

---


# 建立初始表單

初始表單（再融資表單）用於通過觸發&#x200B;**「簽署多個Forms」工作流程簽署多個表AEM單。**&#x200B;您可以輸入您選擇的值，但請確定表單中已新增下列欄位。



| 欄位類型 | 名稱 | 目的 | 隱藏 | 預設值 |
------------------------|---------------------------------------|--------------------|--------|-----------------
| TextField | 簽署 | 若要指出簽署狀態 | Y | N |
| TextField | guid | 要唯一識別表單 | Y | 郵編：3889 |
| TextField | customerName | 若要擷取客戶名稱 | N |
| TextField | customerEmail | 客戶以電子郵件傳送通知 | N |
| 核取方塊 | formsToSign | 項目標識包中的表單 | N |



初始表單必須進行設定，以觸AEM發名為&#x200B;**signmultipleforms**的工作流程
請確定「資料檔案路徑」已設為**Data.xml**。 當范常式式碼在表單提交的裝載程式中尋找名為Data.xml的檔案時，這一點非常重要。

## 資產

初始表單（再融資表單）可從此處下載[](assets/refinance-form.zip)





