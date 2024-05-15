---
title: 如何在AEM中使用主要專案
description: 主要專案透過AEM專案大幅簡化使用者和團隊管理。
version: 6.4, 6.5, Cloud Service
topic: Content Management, Collaboration
feature: Projects
level: Intermediate
role: User
jira: KT-256
thumbnail: 17740.jpg
doc-type: Feature Video
exl-id: 78ff62ad-1017-4a02-80e9-81228f9e01eb
duration: 260
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 0%

---

# 使用主要專案

主要專案透過以下功能，大幅簡化使用者和團隊管理 [!DNL AEM Projects].

>[!VIDEO](https://video.tv.adobe.com/v/17740?quality=12&learn=on)

管理員現在可以建立 **[!DNL Master Project]** 並將使用者指派給作為專案團隊一部分的角色/許可權。 專案可以從主要專案建立，並自動繼承團隊成員資格。 這提供下列幾項優點：

* 在多個專案中重複使用現有團隊
* 加速專案建立，因為團隊不需要手動重新建立
* 從中央位置管理團隊成員資格，專案會自動繼承團隊的任何更新
* 避免建立重複的ACL，以免造成效能問題

[!DNL Master Projects] 可建立於 [!UICONTROL 主版] 資料夾位於 [!UICONTROL AEM專案]. 建立主要專案後，當建立新專案時，它會在精靈中顯示為可用範本旁的選項。

[!DNL Project Masters] URL (本機AEM作者執行個體)： [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## 刪除 [!DNL Project Masters]

刪除主要專案會導致衍生的專案無法使用。

在刪除主要專案之前，請確定所有衍生的專案都已完成並從AEM中移除。 在移除衍生的專案之前，請務必儲存所有必要的專案資料。 從AEM移除所有衍生專案後，就可以安全地刪除主專案。

## 標籤 [!DNL Project Masters] 為非使用中

透過將主要專案的狀態變更為非使用中專案屬性，非使用中主要專案會從主要專案清單中消失。

若要顯示非使用中的主專案，請切換頂端列中的「顯示使用中」篩選按鈕（在清單顯示切換旁邊）。 若要讓非使用中專案再次啟用，只要選取非使用中主要專案、編輯專案屬性，然後再次將其設定為啟用。

## 瞭解 [!DNL Project Masters]

![主要專案技術檢視](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] 透過定義一組AEM使用者群組（擁有者、編輯者和觀察者）來工作，並允許衍生的「專案」參照和重複使用這些集中定義的使用者群組。

這會減少AEM中所需的使用者群組總數。 早於 [!DNL Project Masters]，每個專案都會建立3個使用者群組及隨附的ACE，以強制進行許可權管理，因此100個專案產生的使用者群組達300個。 主要專案允許任意數量的專案重複使用相同的三個群組，前提是共用的成員資格符合整個專案的業務需求。
