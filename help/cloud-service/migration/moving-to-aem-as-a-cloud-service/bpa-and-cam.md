---
title: 設定您的BPA和CAM專案
description: 瞭解Best Practices Analyzer和Cloud Acceleration Manager如何提供移轉至AEM as a Cloud Service的自訂指南。
version: Experience Manager as a Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8627
thumbnail: 336957.jpeg
exl-id: f8289dd4-b293-4b8f-b14d-daec091728c0
duration: 680
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 2%

---

# Best Practices Analyzer與Cloud Acceleration Manager

瞭解Best Practices Analyzer (BPA)和Cloud Acceleration Manager (CAM)如何提供遷移至AEM as a Cloud Service的自訂指南。 

>[!VIDEO](https://video.tv.adobe.com/v/336957?quality=12&learn=on)

## 使用BPA和CAM

![BPA與CAM高階圖表](assets/bpa-cam-diagram.png)

BPA套件應安裝在複製的AEM 6.x生產環境中。 BPA會產生一份報表，然後可將其上傳至CAM，為移至AEM as a Cloud Service所需進行的關鍵活動提供指引。

## 重要活動

+ 仿製生產6.x環境。 當您移轉內容並重構程式碼時，複製生產環境對於測試各種工具和變更非常有用。
+ 從[軟體發佈入口網站](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)下載最新的BPA工具，並安裝在您的AEM 6.x複製環境中。
+ 使用BPA工具產生可上傳至Cloud Acceleration Manager (CAM)的報告。 可透過[https://experience.adobe.com/](https://experience.adobe.com/) > **Experience Manager** > **Cloud Acceleration Manager**&#x200B;存取CAM。
+ 使用CAM提供需對目前程式碼庫和環境進行哪些更新才能移至AEM as a Cloud Service的指引。

## 實作練習

透過嘗試您透過此實作練習學到的內容，運用您的知識。

在嘗試實作練習之前，請確定您已觀看並瞭解上述影片和下列資料：

+ [以不同方式思考AEM as a Cloud Service](./introduction.md)
+ [什麼是AEM as a Cloud Service？](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/what-is-aem-as-a-cloud-service.html?lang=zh-Hant)
+ [Assets as a Cloud Service 架構](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/architecture.html?lang=zh-Hant)
+ [可變和不可變的內容](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/mutable-immutable.html?lang=zh-Hant)
+ [為AEM as a Cloud Service和AEM 6.x開發時的差異](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=zh-Hant#developing)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session1-differently#bootcamp---session-1-introduction-and-thinking-differently"><img alt="實作練習GitHub存放庫" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">使用Best Practices Analyzer實作</div>
            <p style="margin:1rem 0">
                探索Best Practices Analyzer (BPA)，並透過對包含範例違規的舊版WKND程式碼庫執行它來檢閱結果。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session1-differently#bootcamp---session-1-introduction-and-thinking-differently" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">請試用最佳做法分析工具</span>
            </a>
        </td>
    </tr>
</table>


## 其他資源

+ [下載最佳做法分析工具](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Best*+Practices*+Analyzer*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)