---
title: 建立Forms以進行簽署
description: 建立需要包含在簽署套件中的表單。
feature: 適用性表單
version: 6.4,6.5
kt: 6893
thumbnail: 6893.jpg
topic: 開發
role: User
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '329'
ht-degree: 1%

---


# 建立簽名表單

下一步是建立您要納入套件中的最適化表單。 建立簽署表單時，請務必遵循下列要點：

* 確認表單是以&#x200B;**SignMultipleForms**&#x200B;範本為基礎。 這可確保表單已預先填入從資料庫擷取的資料。

* 表單必須設定為使用Adobe Sign，且signer1欄位必須與客戶電子郵件欄位相關聯
* 表單還需要與名為&#x200B;**getnextform**&#x200B;的clientLib相關聯
* 表單需要使用簽名步驟元件。
* 表單也必須使用自訂&#x200B;**簽署多個表單**&#x200B;元件。 此元件可讓您導覽至下一個表單以登入套件。
* 表單的提交必須設定為觸發AEM工作流程&#x200B;**更新簽名狀態**
* 請確定「資料檔案路徑」已設為&#x200B;**Data.xml**。 當范常式式碼在表單提交程式的裝載中尋找名為Data.xml的檔案時，這是很重要的。

製作表單後，請在表單中加入&#x200B;**commonfields**&#x200B;最適化表單片段。 片段會標示為隱藏。 此片段包含下列欄位。

* **已簽名**  — 保存簽名狀態的欄位
* **guid**  — 識別套件中表單的唯一識別碼
* **customerEmail**  — 此欄位將保留客戶的電子郵件



>[!NOTE]
>如果您想要將資料從一個表單傳輸到套件中的另一個表單，請確定所有表單中的表單欄位名稱相同。

## 全部完成表單

封裝中的所有表單填妥並簽署後，就需要顯示適當的訊息。 此消息在Alldone窗體的幫助下顯示。 範例表單中包含Alldone表單。

## 資產

範例表單（包括本教學課程中使用的）可從此處[下載](assets/forms-for-signing.zip)
