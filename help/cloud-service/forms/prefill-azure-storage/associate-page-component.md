---
title: 將頁面元件與新的自適應表單範本建立關聯
description: 建立新的頁面元件
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
exl-id: 7b2b1e1c-820f-4387-a78b-5d889c31eec0
duration: 25
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 5%

---

# 將頁面元件與範本建立關聯

下一步是將頁面元件與新的調適型表單範本建立關聯。 這可確保在每次根據新範本的自適應表單轉譯時，都會執行頁面元件中的程式碼。 為了這個教學課程的目的，已在&#x200B;**AzurePortalStorage**&#x200B;資料夾中建立名為&#x200B;**StoreAndRestoreFromAzure**的新調適型表單範本。
導覽至/conf/AzurePortalStorage/settings/wcm/templates/storeandrestorefromazure/initial/jcr：content節點，新增下列屬性並儲存變更。

| **屬性名稱** | **屬性型別** | **屬性值** |
|--------------------|-------------------|-------------------------------------------------------|
| sling:resourceType | 字串 | azureportalpagecomponent/component/page/storeandfetch |

導覽至/conf/AzurePortalStorage/settings/wcm/templates/storeandrestorefromazure/structure/jcr：content節點，新增下列屬性並儲存變更。

| **屬性名稱** | **屬性型別** | **屬性值** |
|--------------------|-------------------|-------------------------------------------------------|
| sling:resourceType | 字串 | azureportalpagecomponent/component/page/storeandfetch |


## 後續步驟

[建立與Azure儲存體的整合](./create-fdm.md)
