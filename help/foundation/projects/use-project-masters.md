---
title: 如何在AEM中使用專案主版
description: Project Masters可大幅簡化AEM Projects的使用者和團隊管理。
version: 6.4, 6.5, Cloud Service
topic: Content Management, Collaboration
feature: Projects
level: Intermediate
role: User
kt: 256
thumbnail: 17740.jpg
exl-id: 78ff62ad-1017-4a02-80e9-81228f9e01eb
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 0%

---

# 使用專案主版

Project Masters通過 [!DNL AEM Projects].

>[!VIDEO](https://video.tv.adobe.com/v/17740?quality=12&learn=on)

管理員現在可以建立 **[!DNL Master Project]** 並將使用者指派給專案團隊中的角色/權限。 可以從主項目建立項目並自動繼承團隊成員資格。 這提供幾項優點：

* 跨多個專案重複使用現有團隊
* 無需手動重新建立團隊，加快專案建立速度
* 從中央位置管理團隊成員資格，且項目會自動繼承對團隊的任何更新
* 避免建立可能導致效能問題的重複ACL

[!DNL Master Projects] 可在 [!UICONTROL 大師] 檔案夾 [!UICONTROL AEM專案]. 建立主項目後，當建立新項目時，它將作為選項與嚮導中可用的模板一起顯示。

[!DNL Project Masters] URL（本機AEM製作例項）: [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## 刪除 [!DNL Project Masters]

刪除主項目會導致派生項目無法使用。

刪除主專案前，請確定所有衍生專案均已完成並從AEM中移除。 移除衍生專案前，請務必儲存任何必要的專案資料。 從AEM中移除所有衍生專案後，即可安全地刪除主專案。

## 標籤 [!DNL Project Masters] 非作用中

通過將主項目的狀態更改為項目屬性中的非活動狀態，非活動主項目將從主項目清單中消失。

若要顯示非使用中主專案，請切換頂端列的「顯示使用中」篩選按鈕（清單顯示切換按鈕旁）。 要使非活動項目再次活動，只需選擇非活動主項目、編輯項目屬性，然後將其再次設定為活動。

## 了解 [!DNL Project Masters]

![項目主技術視圖](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] 工作方式是定義一組AEM使用者群組（擁有者、編輯者和觀察者），並允許衍生專案參照和重複使用這些集中定義的使用者群組。

這可減少AEM中需要的使用者群組總數。 之前 [!DNL Project Masters]，每個專案都建立了3個使用者群組，並隨附ACE以強制執行權限，因此100個專案產生了300個使用者群組。 如果共用成員資格符合項目中的業務要求，則項目主體允許任意數量的項目重複使用相同的三個組。
