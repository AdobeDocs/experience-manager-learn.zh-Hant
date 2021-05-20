---
title: 搭配AEM Assets Dynamic Media使用智慧型裁切
description: 智慧型裁切會使用Adobe Sensei來消除為回應式設計而裁切內容所耗時且成本高昂的工作。
sub-product: dynamic-media
feature: 智慧型裁切、影像描述檔
version: 6.4, 6.5
topic: 內容管理
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 2%

---


# 搭配AEM Assets Dynamic Media使用智慧型裁切{#using-smart-crop-with-aem-assets-dynamic-media}

智慧型裁切會使用Adobe Sensei來消除為回應式設計而裁切內容所耗時且成本高昂的工作。

>[!VIDEO](https://video.tv.adobe.com/v/21519/)

>[!NOTE]
>
>視訊假設您的AEM例項在Dynamic Media S7模式下執行。 [若需使用Dynamic Media設定AEM的相關指示，請參閱這裡。](https://helpx.adobe.com/tw/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Adobe Experience Manager的Dynamic Media智慧型裁切功能包括

* AEM資產管理員可根據裝置寬度和高度，輕鬆建立智慧型裁切的影像設定檔。
* 您可以對個別資產執行智慧型裁切，或對資料夾內的所有資產執行智慧型裁切。
* 智慧型裁切編輯版面可調整大小，以獲得更佳的可見度。
* AEM Sites的Dynamic Media元件支援智慧型裁切。
* 智慧型裁切資產的發佈URL可用於接受託管資產的第三方應用程式。

>[!NOTE]
>
>智慧型裁切座標取決於外觀比例。 也就是說，對於影像設定檔中的各種智慧型裁切設定，如果在影像設定檔中新增的維度的長寬比相同，則會將相同的長寬比傳送至動態媒體。 因此，智慧型裁切編輯器會建議相同的裁切區域。 例如，100x100和200x200的裁切設定會導致系統產生相同的智慧型裁切。
