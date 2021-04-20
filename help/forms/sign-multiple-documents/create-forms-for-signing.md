---
title: 為簽署建立Forms
description: 建立需要包含在簽署套件中的表單。
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6893
thumbnail: 6893.jpg
topic: Development
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '332'
ht-degree: 1%

---


# 建立表單以進行簽署

下一步是建立要包含在軟體包中的自適應表單。 在建立簽署表格時，請務必遵守下列要求：

* 請確定表單是以&#x200B;**SignMultipleForms**&#x200B;範本為基礎。 這可確保表單預先填入從資料庫擷取的資料。

* 表單必須設定為使用Adobe Sign，簽署者1欄位必須與「客戶電子郵件」欄位關聯
* 這些表單還需要與名為&#x200B;**getnextform**&#x200B;的clientLib關聯
* 表單必須使用「簽名步驟」元件。
* 表單也必須使用自訂&#x200B;**簽署多個表單**&#x200B;元件。 此元件可讓您導覽至下一個要登入套件的表單。
* 表單的提交必須配置為觸AEM發工作流&#x200B;**更新簽名狀態**
* 請確定「資料檔案路徑」已設為&#x200B;**Data.xml**。 當范常式式碼在表單提交的裝載程式中尋找名為Data.xml的檔案時，這一點非常重要。

製作表單後，請在表單中加入&#x200B;**commenfields**&#x200B;最適化表單片段。 片段將標籤為隱藏。 此片段包含下列欄位。

* **signed**  —— 用於保存簽名狀態的欄位
* **guid**  —— 用於標識包中表單的唯一標識符
* **customerEmail**  —— 此欄位將保存客戶的電子郵件



>[!NOTE]
>如果您想要將資料從某個表單傳送至套件中的另一個表單，請確定所有表單中的表單欄位名稱都相同。

## 完整表單

在封裝中的所有表格填入並簽署後，我們需要顯示適當的訊息。 此消息在Alldone表單的幫助下顯示。 Alldone表單包含在範例表單中。

## 資產

本教學課程中使用的範例表單包括[從此處下載](assets/forms-for-signing.zip)
