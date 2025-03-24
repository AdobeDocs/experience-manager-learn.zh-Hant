---
title: AEM Assets中的封閉使用者群組
description: 封閉使用者群組(CUG)是一項功能，用來將內容的存取許可權製為已發佈網站上的選定使用者群組。 此影片說明如何將封閉式使用者群組搭配Adobe Experience Manager Assets使用，以限制對資產的特定資料夾的存取權。
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
jira: KT-649
thumbnail: 22155.jpg
last-substantial-update: 2022-06-06T00:00:00Z
doc-type: Feature Video
exl-id: a2bf8a82-15ee-478c-b7c3-de8a991dfeb8
duration: 321
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 0%

---

# 封閉使用者群組{#using-closed-user-groups-with-aem-assets}

封閉使用者群組(CUG)是一項功能，用來將內容的存取許可權製為已發佈網站上的選定使用者群組。 此影片說明如何將封閉式使用者群組搭配Adobe Experience Manager Assets使用，以限制對資產的特定資料夾的存取權。 AEM 6.4中首次引入對AEM Assets的封閉使用者群組的支援。

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## 具AEM Assets的封閉式使用者群組(CUG)

* 旨在限制對AEM Publish執行個體上資產的存取。
* 授予一組使用者/群組的讀取存取權。
* CUG只能在資料夾層級設定。 無法在個別資產上設定CUG。
* 任何子資料夾和套用的資產都會自動繼承CUG原則。
* 藉由設定新的CUG原則，子資料夾可以覆寫CUG原則。 應謹慎使用，且不應視為最佳實務。

## 已關閉的使用者群組與存取控制清單 {#closed-user-groups-vs-access-control-lists}

封閉使用者群組(CUG)和存取控制清單(ACL)皆可根據AEM Security使用者和群組，用來控制AEM中內容的存取。 不過，這些功能的應用程式和實施方式差異極大。 下表摘要這兩個功能之間的差異。

|                   | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| 預期用途 | 針對&#x200B;**目前** AEM執行個體上的內容設定並套用許可權。 | 在AEM **作者**&#x200B;執行個體上設定內容的CUG原則。 針對AEM **發佈**&#x200B;執行個體上的內容套用CUG原則。 |
| 許可權層級 | 定義所有層級之使用者/群組的授予/拒絕許可權：讀取、修改、建立、刪除、讀取ACL、編輯ACL、復寫。 | 授予一組使用者/群組的讀取存取權。 拒絕對&#x200B;*所有其他*&#x200B;使用者/群組的讀取存取權。 |
| 發佈內容 | ACL *不是*&#x200B;已發佈內容。 | CUG原則&#x200B;*是隨內容發佈的*。 |

## 支援連結 {#supporting-links}

* [管理Assets和已關閉的使用者群組](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=en#closed-user-group)
* [正在建立已關閉的使用者群組](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html)
* [Oak已關閉的使用者群組檔案](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
