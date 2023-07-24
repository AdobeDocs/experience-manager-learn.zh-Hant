---
title: 將Analytics和Real-time Customer Data Platform與Experience Platform來源聯結器教學課程整合
description: 瞭解如何將Adobe Analytics與Real-time Customer Data Platform整合。
solution: Real-Time Customer Data Platform, Analytics
feature: Integrations
topic: Integrations
role: Leader, Architect, Admin, Developer
level: Beginner
index: true
hidefromtoc: true
kt: null
thumbnail: null
last-substantial-update: 2023-04-11T00:00:00Z
badgeIntegration: label="整合" type="positive"
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 3%

---


# 將Adobe Analytics和Real-time Customer Data Platform與Experience Platform來源聯結器整合

<ol>
    <li><a href="https://experienceleague.adobe.com/?lang=en#dashboard/learning" _target="_blank" rel="noopener noreferrer">建立方案</a> 以擷取資料。</li>
    <li><a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/data-ingestion/create-datasets-and-ingest-data.html" _target="_blank" rel="noopener noreferrer">建立資料集</a> 以擷取資料。</a></li>
    <li><a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/identities/label-ingest-and-verify-identity-data.html?lang=en" _target="_blank" rel="noopener noreferrer">在架構上設定正確的身分識別與身分識別名稱空間</a> 以確定擷取的資料可拼接到統一的設定檔。</li> 
    <li><a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/profiles/bring-data-into-the-real-time-customer-profile.html" _target="_blank" rel="noopener noreferrer">為設定檔啟用結構描述和資料集</a>.</li>
    <li>使用下列其中一種方法將資料內嵌至Experience Platform：</li>
        <ul>
            <li><a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/sources/ingest-data-from-adobe-analytics.html" _target="_blank" rel="noopener noreferrer">Adobe Analytics來源聯結器</a></li>
            <li>Experience PlatformWeb SDK：</li>
                <ul>
                    <li><a href="https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/overview.html?lang=zh-Hant" _target="_blank" rel="noopener noreferrer">教學課程</a></li>
                    <li><a href="https://experienceleague.adobe.com/docs/analytics/implementation/aep-edge/web-sdk/overview.html" _target="_blank" rel="noopener noreferrer">檢查清單</a></li>
                </ul>
            <li>Experience Platform行動SDK：</li>
                <ul>
                    <li><a href="https://experienceleague.adobe.com/docs/platform-learn/data-collection/mobile-sdk/create-mobile-properties.html" _target="_blank" rel="noopener noreferrer">教學課程</a></li>
                    <li><a href="https://experienceleague.adobe.com/docs/analytics/implementation/aep-edge/mobile-sdk/overview.html" _target="_blank" rel="noopener noreferrer">檢查清單</a></li>
                </ul></li>
            <li>Edge Network伺服器API：</li>
                <ul>
                    <li><a href="https://experienceleague.adobe.com/docs/experience-platform/edge-network-server-api/interacting-other-adobe-solutions/interacting-adobe-analytics.html" _target="_blank" rel="noopener noreferrer">教學課程</a></li>
                </ul>
       </ul>
    <li><a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/segments/create-segments.html" _target="_blank" rel="noopener noreferrer">在Experience Platform中建立區段。</a> 系統會自動判斷區段是評估為批次（資料聯結器）還是串流（邊緣網路）。</li>
    <li><a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/destinations/create-destinations-and-activate-data.html" _target="_blank" rel="noopener noreferrer">設定目的地，以將設定檔屬性和對象會籍共用至所需的目的地。</a></li>   
</ol>

>[!NOTE]
>
>Adobe Analytics來源聯結器的標準工作流程步驟會建立結構描述和資料集，用於從Analytics「現況」擷取資料。 因此，前兩個步驟由系統處理。 對應工作流程需要建立自訂屬性；因此，請完全遵循步驟順序。