---
title: 將頁面元件與新的自適應表單範本建立關聯
description: 建立新的頁面元件
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 5%

---

# 將頁面元件與範本建立關聯

下一步是將頁面元件與新的最適化表單範本建立關聯。 這可確保每次根據新範本呈現最適化表單時，都會執行頁面元件中的程式碼。 在本教學課程中，將新增一個最適化表單範本，名為 **StoreAndRestoreFromAzure** 建立於 **AzurePortalStorage** 資料夾。
導覽至/conf/AzurePortalStorage/settings/wcm/templates/storeandrestorefromazure/initial/jcr：content節點，新增下列屬性並儲存變更。

| **屬性名稱** | **屬性型別** | **屬性值** |
|--------------------|-------------------|-------------------------------------------------------|
| sling:resourceType | 字串 | azureportalpagecomponent/component/page/storeandfetch |

導覽至/conf/AzurePortalStorage/settings/wcm/templates/storeandrestorefromazure/structure/jcr：content節點，新增下列屬性並儲存變更。
| **屬性名稱**  | **屬性型別** | **屬性值**                                    | |--------------------|-------------------|-------------------------------------------------------| | sling：resourceType |字串 | azureportalpagecomponent/component/page/storeandfetch |


## 後續步驟

[建立與Azure儲存體的整合](./create-fdm.md)
