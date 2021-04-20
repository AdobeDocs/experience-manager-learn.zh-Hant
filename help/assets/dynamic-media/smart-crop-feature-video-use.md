---
title: 與AEM Assets·Dynamic Media一起使用Smart Crop
description: Smart Crop使用Adobe Sensei來消除裁切內容以進行自適應設計所需的耗時且昂貴的工作。
sub-product: 動態媒體
feature: Smart Crop, Image Profiles
version: 6.4, 6.5
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 2%

---


# 搭配AEM Assets·Dynamic Media使用智慧型裁切{#using-smart-crop-with-aem-assets-dynamic-media}

Smart Crop使用Adobe Sensei來消除裁切內容以進行自適應設計所需的耗時且昂貴的工作。

>[!VIDEO](https://video.tv.adobe.com/v/21519/)

>[!NOTE]
>
>視訊假設您AEM的例項在Dynamic MediaS7模式下執行。 [有關與Dynamic MediaAEM一起設定的說明，請參閱這裡。](https://helpx.adobe.com/tw/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Adobe Experience Manager的Dynamic Media智慧作物功能包括

* 資AEM產管理員可以根據裝置寬度和高度，輕鬆建立智慧型裁切的影像設定檔。
* 智慧型裁切可針對個別資產執行，或可針對資料夾內的所有資產執行。
* 智慧型裁切編輯版面可調整大小，以提高可見度。
* AEM Sites的Dynamic Media元件支援Smart Crop。
* 「智慧裁切」資產的發佈URL可用於接受託管資產的第三方應用程式。

>[!NOTE]
>
>智慧型裁切座標視外觀比例而定。 也就是說，對於影像描述檔中的各種智慧型裁切設定，如果影像描述檔中新增尺寸的長寬比相同，則會傳送相同的長寬比給動態媒體。 因此，智慧型裁切編輯器中會建議相同的裁切區域。 例如，裁切設定為100x100和200x200會導致系統產生相同的智慧型裁切。
