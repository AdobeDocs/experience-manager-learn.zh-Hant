---
title: AEM Assets的封閉使用者群組
description: 「封閉使用者群組」(CUG)是一種功能，可用來限制對發佈網站上特定使用者群組的內容存取。 此影片顯示「關閉的使用者群組」如何與「Adobe Experience Manager資產」搭配使用，以限制對特定資產資料夾的存取。
version: 6.3, 6.4, 6.5, cloud-service
topic: 管理、安全性
feature: 使用者和群組
role: 管理員
level: 中級
kt: 649
thumbnail: 22155.jpg
translation-type: tm+mt
source-git-commit: 407840a0e0c90c4f004390a052d036f9b69fa8df
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 1%

---


# 已關閉的用戶組{#using-closed-user-groups-with-aem-assets}

「封閉使用者群組」(CUG)是一種功能，可用來限制對發佈網站上特定使用者群組的內容存取。 此影片顯示「關閉的使用者群組」如何與「Adobe Experience Manager資產」搭配使用，以限制對特定資產資料夾的存取。 在6.4中首次對AEM Assets的封閉用戶AEM組提供支援。

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## Closed User Group(CUG)與AEM Assets

* 設計為限制AEM Publish例項上資產的存取權。
* 授予一組使用者／群組的讀取存取權。
* CUG只能在資料夾級別配置。 CUG無法設定在個別資產上。
* 任何子資料夾和套用的資產會自動繼承CUG原則。
* 通過設定新的CUG策略，子資料夾可以覆蓋CUG策略。 應謹慎使用，不視為最佳實務。

## 關閉的用戶組與訪問控制清單{#closed-user-groups-vs-access-control-lists}

封閉的使用者群組(CUG)和存取控制清單(ACL)都可用來控制對安全性使用者和群組中內AEM容的存AEM取權。 但是這些功能的應用和實現卻大不相同。 下表總結了兩個特徵之間的區別。

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| 預期用途 | 在&#x200B;**current**&#x200B;例項上設定並套用內容的權AEM限。 | 在&#x200B;**author**&#x200B;例AEM項上為內容配置CUG策略。 在&#x200B;**publish**&#x200B;例AEM項上套用內容的CUG原則。 |
| 權限層級 | 定義所有層級的使用者／群組已授與／拒絕權限：讀取、修改、建立、刪除、讀取ACL、編輯ACL、複製。 | 授予一組使用者／群組的讀取存取權。 拒絕對&#x200B;*所有其他*&#x200B;用戶／組的讀取訪問。 |
| 出版物 | ACL是&#x200B;*not*&#x200B;隨內容發佈的。 | CUG策略&#x200B;*與內容一起發佈。* |

## 支援連結{#supporting-links}

* [管理資產和已關閉的使用者群組](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=en#closed-user-group)
* [建立已關閉的使用者群組](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html)
* [Oak Closed使用者群組檔案](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
