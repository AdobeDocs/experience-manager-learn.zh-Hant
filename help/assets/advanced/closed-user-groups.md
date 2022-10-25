---
title: AEM Assets中的封閉使用者群組
description: 封閉使用者群組(CUG)是一項功能，用來限制特定使用者群組在發佈網站上存取內容。 此影片顯示如何搭配Adobe Experience Manager Assets使用封閉使用者群組，以限制存取特定資產資料夾。
version: 6.4, 6.5, Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
kt: 649
thumbnail: 22155.jpg
last-substantial-update: 2022-06-06T00:00:00Z
exl-id: a2bf8a82-15ee-478c-b7c3-de8a991dfeb8
source-git-commit: f37483f90f2a707c906e1e206795fdebb5f698e9
workflow-type: tm+mt
source-wordcount: '377'
ht-degree: 0%

---

# 已關閉的用戶組{#using-closed-user-groups-with-aem-assets}

封閉使用者群組(CUG)是一項功能，用來限制特定使用者群組在發佈網站上存取內容。 此影片顯示如何搭配Adobe Experience Manager Assets使用封閉使用者群組，以限制存取特定資產資料夾。 AEM 6.4中首次推出支援使用AEM Assets的封閉式使用者群組。

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## 封閉使用者群組(CUG)，含AEM Assets

* 設計來限制AEM Publish例項上資產的存取權。
* 授予一組使用者/群組的讀取存取權。
* CUG只能在資料夾級別配置。 無法對個別資產設定CUG。
* CUG策略由任何子資料夾和應用的資產自動繼承。
* 通過設定新的CUG策略，子資料夾可以覆蓋CUG策略。 這應謹慎使用，不視為最佳作法。

## 封閉用戶組與訪問控制清單 {#closed-user-groups-vs-access-control-lists}

封閉用戶組(CUG)和訪問控制清單(ACL)都用於控制對AEM中內容的訪問，並基於AEM安全用戶和組。 然而，這些功能的應用和實現卻大不相同。 下表概述了這兩種特徵之間的差異。

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| 預期用途 | 設定並套用上內容的權限 **電流** AEM例項。 | 為AEM上的內容配置CUG原則 **作者** 例項。 在AEM上對內容套用CUG原則 **發佈** 例項。 |
| 權限層級 | 定義所有級別的用戶/組的已授予/拒絕權限：讀取、修改、建立、刪除、讀取ACL、編輯ACL、複製。 | 授予一組使用者/群組的讀取存取權。 拒絕對的讀取訪問 *其他* 使用者/群組。 |
| 出版物 | ACL為 *not* 隨內容發佈。 | CUG策略 *為* 隨內容發佈。 |

## 支援連結 {#supporting-links}

* [管理資產和封閉的使用者群組](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=en#closed-user-group)
* [建立封閉的使用者群組](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html)
* [Oak封閉使用者群組檔案](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
