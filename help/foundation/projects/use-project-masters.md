---
title: 如何在中使用Project Master AEM
description: Project Masters通過Projects大大簡化了用戶和團隊AEM管理。
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

# 使用項目首頁

Project Masters通過 [!DNL AEM Projects]。

>[!VIDEO](https://video.tv.adobe.com/v/17740?quality=12&learn=on)

管理員現在可以建立 **[!DNL Master Project]** 並將用戶分配到角色/權限作為項目團隊的一部分。 可以從主項目建立項目並自動繼承團隊成員資格。 這提供了以下幾個優點：

* 跨多個項目重複使用現有團隊
* 加快項目建立，因為團隊不必手動重新建立
* 從中心位置管理團隊成員資格，項目會自動繼承對團隊的任何更新
* 避免建立可導致效能問題的重複ACL

[!DNL Master Projects] 可在 [!UICONTROL 馬斯特] 資料夾 [!UICONTROL 項AEM目]。 建立主項目後，在建立新項目時，它將作為選項與嚮導中可用模板一起顯示。

[!DNL Project Masters] URL（本地AEM作者實例）: [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## 刪除 [!DNL Project Masters]

刪除主項目將導致無法使用的派生項目。

在刪除主項目之前，請確保完成並從中刪除所有派生項AEM目。 在刪除派生的項目之前，請確保保存任何必需的項目資料。 從中刪除所有派生項AEM目後，可以安全地刪除主項目。

## 標籤 [!DNL Project Masters] 不活動

通過將主項目的狀態更改為項目屬性中的非活動狀態，非活動主項目將從主項目清單中消失。

要顯示非活動的主項目，請切換頂部欄中的「顯示活動」篩選器按鈕（清單顯示切換旁）。 要使非活動項目再次處於活動狀態，只需選擇非活動的主項目、編輯項目屬性，並將其再次設定為活動。

## 瞭解 [!DNL Project Masters]

![項目主控程式技術視圖](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] 通過定義一組用戶組(AEM所有者、編輯器和觀察器)並允許派生的「項目」引用和重新使用這些集中定義的用戶組來工作。

這將減少中所需的用戶組總數AEM。 之前 [!DNL Project Masters]每個項目都建立了3個用戶組，隨附的ACE強制執行權限，因此100個項目產生了300個用戶組。 「項目主管」允許任意數量的項目重複使用同三個組，前提是共用成員資格與整個項目的業務要求相一致。
