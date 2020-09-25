---
title: 在AEM中使用專案主版
description: Project Masters透過AEM Projects大幅簡化使用者和團隊管理。
version: 6.4, 6.5
feature: projects, users-and-groups
topics: administration, collaboration, performance
activity: use
audience: administrator, implementer, architect
doc-type: article
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 0%

---


# 使用專案主版

Project Masters可大幅簡化使用者和團隊管理 [!DNL AEM Projects]。

>[!VIDEO](https://video.tv.adobe.com/v/17740/?quality=9&learn=on)

管理員現在可以建立 **[!DNL Master Project]** 並指派使用者至專案團隊中的角色／權限。 專案可從主專案建立，並自動繼承團隊會籍。 這提供了幾項優點：

* 跨多個專案重新使用現有團隊
* 加速專案建立，因為團隊不需要手動重新建立
* Projects會自動從中央位置管理團隊會籍，並繼承任何對團隊的更新
* 避免建立可能導致效能問題的重複ACL

[!DNL Master Projects] 可在「AEM專案」下 [!UICONTROL 的] 「主檔案夾」 [!UICONTROL 下建立]。 建立 [!DNL Master Project] 新的「專案」後，它會在精靈中顯示為可用範本旁的選項。

[!DNL Project Masters] URL（本機AEM作者例項）: [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## 刪除 [!DNL Project Masters]

刪除主項目會導致衍生的項目無法使用。

在刪除主專案之前，請確定所有衍生專案都已完成並從AEM移除。 請務必先儲存任何必要的專案資料，再移除衍生的專案。 從AEM移除所有衍生專案後，即可安全地刪除主專案。

## 標籤為 [!DNL Project Masters] 非活動

通過將主項目狀態更改為項目屬性中的非活動狀態，非活動的主項目將從主項目清單中消失。

若要顯示非作用中的主專案，請切換頂端列中的「顯示作用中」篩選按鈕（清單顯示旁）。 要使非活動項目再次激活，只需選擇非活動的主項目、編輯項目屬性，然後再次將其設定為活動。

## 瞭解 [!DNL Project Masters]

![專案主修人員技術檢視](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] 工作方式：定義一組AEM使用者群組（擁有者、編輯者和觀察者），並允許衍生的「專案」參考和重複使用這些集中定義的使用者群組。

如此可減少AEM中所需的使用者群組總數。 之前， [!DNL Project Masters]每個項目都建立了3個用戶組和隨附的ACE，以強制許可，因此100個項目產生了300個用戶組。 專案主管允許任意數量的專案重複使用相同的3個群組，前提是共用會籍符合專案的業務需求。
