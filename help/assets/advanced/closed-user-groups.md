---
title: 關閉的AEM Assets用戶組
description: 關閉的用戶組(CUG)是一種功能，用於限制對發佈站點上選定用戶組的內容的訪問。 此視頻顯示了關閉的用戶組如何與Adobe Experience Manager資產一起使用，以限制對特定資產資料夾的訪問。
version: 6.4, 6.5, Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
kt: 649
thumbnail: 22155.jpg
exl-id: a2bf8a82-15ee-478c-b7c3-de8a991dfeb8
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '377'
ht-degree: 0%

---

# 關閉的用戶組{#using-closed-user-groups-with-aem-assets}

關閉的用戶組(CUG)是一種功能，用於限制對發佈站點上選定用戶組的內容的訪問。 此視頻顯示了關閉的用戶組如何與Adobe Experience Manager資產一起使用，以限制對特定資產資料夾的訪問。 在6.4中首次對AEM Assets的封閉用戶AEM組提供支援。

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## 關閉的用戶組(CUG)與AEM Assets

* 旨在限制對AEM發佈實例上的資產的訪問。
* 授予對一組用戶/組的讀取訪問權限。
* CUG只能在資料夾級別配置。 無法對單個資產設定CUG。
* CUG策略由任何子資料夾和應用的資產自動繼承。
* 通過設定新的CUG策略，子資料夾可以覆蓋CUG策略。 這應少用，不應被視為最佳做法。

## 關閉的用戶組與訪問控制清單 {#closed-user-groups-vs-access-control-lists}

關閉的用戶組(CUG)和訪問控制清單(ACL)都用於控制對安全用戶和組中的內AEM容的訪AEM問，並基於這些用戶和組。 然而這些特徵的應用和實現卻有很大不同。 下表概述了兩種特徵之間的區別。

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| 預期用途 | 配置和應用上內容的權限 **當前** 實AEM例。 | 為上的內容配置CUG策AEM略 **作者** 實例。 將CUG策略應用於上的內AEM容 **發佈** 實例。 |
| 權限級別 | 定義所有級別的用戶/組的已授予/拒絕權限：讀取、修改、建立、刪除、讀取ACL、編輯ACL、複製。 | 授予對一組用戶/組的讀取訪問權限。 拒絕對 *其他* 用戶/組。 |
| 出版物 | ACL是 *不* 與內容一起發佈。 | CUG策略 *是* 與內容一起發佈。 |

## 支援連結 {#supporting-links}

* [管理資產和關閉的用戶組](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=en#closed-user-group)
* [建立關閉的用戶組](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html)
* [Oak關閉的用戶組文檔](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
