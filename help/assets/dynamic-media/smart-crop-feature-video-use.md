---
title: 將Smart Crop與AEM AssetsDynamic Media
description: Smart Crop使用Adobe Sensei來消除為響應性設計而裁剪內容的耗時且成本高昂的任務。
feature: Smart Crop, Image Profiles
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 295bbfb6-241f-41c0-972d-d9688863cea1
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 2%

---

# 將Smart Crop與AEM AssetsDynamic Media{#using-smart-crop-with-aem-assets-dynamic-media}

Smart Crop使用Adobe Sensei來消除為響應性設計而裁剪內容的耗時且成本高昂的任務。

>[!VIDEO](https://video.tv.adobe.com/v/21519?quality=12&learn=on)

>[!NOTE]
>
>視頻假設AEM您的實例在Dynamic MediaS7模式下運行。 [有關與Dynamic MediaAEM建立關係的說明，請參閱。](https://helpx.adobe.com/tw/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Adobe Experience Manager的Dynamic Media智慧作物能力包括

* 資產管AEM理員可以根據設備寬度和高度輕鬆地為智慧裁剪建立映像配置檔案。
* 可以對單個資產執行智慧裁剪，也可以對資料夾內的所有資產執行智慧裁剪。
* 可以調整智慧裁剪編輯佈局的大小，以便獲得更好的可見性。
* AEM站點的Dynamic Media元件支援Smart Crop。
* 智慧裁剪資產的已發佈URL可用於接受託管資產的第三方應用程式。

>[!NOTE]
>
>智慧裁剪坐標與縱橫比相關。 即，對於影像配置檔案中的各種智慧裁剪設定，如果對於影像配置檔案中添加的尺寸而言縱橫比相同，則將相同縱橫比發送到動態媒體。 因此，智慧裁剪編輯器中建議使用相同的裁剪區域。 例如，如果裁剪設定為100x100和200x200，則系統將生成相同的智慧裁剪。
