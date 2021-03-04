---
title: 如何使用Project Masters AEM
description: Project Masters可大幅簡化使用者和團隊的專案AEM管理。
version: 6.4, 6.5, cloud-service
topic: 內容管理
feature: 專案
level: 中級
role: 業務從業人員
kt: 256
thumbnail: 17740.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 0%

---


# 使用專案主版

專案主管使用[!DNL AEM Projects]大幅簡化使用者和團隊管理。

>[!VIDEO](https://video.tv.adobe.com/v/17740/?quality=12&learn=on)

管理員現在可以建立&#x200B;**[!DNL Master Project]**，並將使用者指派給專案團隊中的角色／權限。 專案可從主專案建立，並自動繼承團隊會籍。 這提供了幾項優點：

* 跨多個專案重複使用現有團隊
* 加速專案建立，因為團隊不需要手動重新建立
* Projects會自動從中央位置管理團隊會籍，並繼承任何對團隊的更新
* 避免建立可能導致效能問題的重複ACL

[!DNL Master Projects] 可在「專案」下的  Mastersfolder下 AEM建立。建立主項目後，在建立新項目時，它將作為嚮導中可用模板的選項一起顯示。

[!DNL Project Masters] URL（本機AEM作者例項）: [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## 刪除 [!DNL Project Masters]

刪除主項目會導致衍生的項目無法使用。

在刪除主項目之前，請確保已完成並從中刪除所有派生項AEM目。 請務必先儲存任何必要的專案資料，再移除衍生的專案。 從中刪除所有派生項AEM目後，可以安全地刪除主項目。

## 將[!DNL Project Masters]標籤為非活動

通過將主項目狀態更改為項目屬性中的非活動狀態，非活動的主項目將從主項目清單中消失。

若要顯示非作用中的主專案，請切換頂端列中的「顯示作用中」篩選按鈕（清單顯示旁）。 要使非活動項目再次激活，只需選擇非活動的主項目、編輯項目屬性，然後再次將其設定為活動。

## 瞭解[!DNL Project Masters]

![專案主修人員技術檢視](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] 定義一組使用者群組(擁AEM有者、編輯者和觀察者)，並允許衍生的專案參考和重複使用這些集中定義的使用者群組。

如此可減少中所需的使用者群組總數AEM。 在[!DNL Project Masters]之前，每個項目都建立了3個用戶組，並附帶了ACE以強制執行權限，因此，100個項目產生了300個用戶組。 專案主管允許任意數量的專案重複使用相同的三個群組，前提是共用會籍符合專案中的業務需求。
