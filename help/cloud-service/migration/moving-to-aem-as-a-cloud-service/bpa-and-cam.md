---
title: 設定BPA和CAM項目
description: 了解Best Practices Analyzer和Cloud Acceleration Manager如何提供自訂指南，以便移轉至AEMas a Cloud Service。
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8627
thumbnail: 336957.jpeg
exl-id: f8289dd4-b293-4b8f-b14d-daec091728c0
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 4%

---

# Best Practices Analyzer和Cloud Acceleration Manager

了解Best Practices Analyzer(BPA)和Cloud Acceleration Manager(CAM)如何提供自訂的指南，以便移轉至AEMas a Cloud Service。 

>[!VIDEO](https://video.tv.adobe.com/v/336957?quality=12&learn=on)

## 使用BPA和CAM

![BPA和CAM高級圖](assets/bpa-cam-diagram.png)

應將BPA套件安裝在原地複製的生產AEM 6.x環境上。 BPA將生成一個報告，然後可以上載到CAM中，該報告將指導需要進行的關鍵活動，以便移動到AEMas a Cloud Service。

## 關鍵活動

+ 複製生產6.x環境。 當您移轉內容和重構程式碼時，複製生產環境對於測試各種工具和變更非常有價值。
+ 從下載最新的BPA工具 [Software Distribution入口網站](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html) 並安裝在AEM 6.x複製環境中。
+ 使用BPA工具產生可上傳至Cloud Acceleration Manager(CAM)的報表。 CAM可透過 [https://experience.adobe.com/](https://experience.adobe.com/) > **Experience Manager** > **Cloud Acceleration Manager**.
+ 使用CAM提供對當前代碼庫和環境進行哪些更新的指導，以便轉到AEMas a Cloud Service。

## 動手練習

通過這個實踐練習來嘗試你學到的東西，來運用你的知識。

在嘗試動手練習之前，請確定您已觀看並了解上述影片，以及下列材料：

+ [對AEMas a Cloud Service有不同的思考](./introduction.md)
+ [什麼是AEMas a Cloud Service?](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/what-is-aem-as-a-cloud-service.html?lang=en)
+ [Assets as a Cloud Service 架構](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/architecture.html?lang=en)
+ [可變和不可變內容](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/mutable-immutable.html?lang=en)
+ [針對AEM as a Cloud Service和AEM 6.x開發的差異](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html#developing)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session1-differently#bootcamp---session-1-introduction-and-thinking-differently"><img alt="實作練習GitHub存放庫" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">使用Best Practices Analyzer進行操作</div>
            <p style="margin:1rem 0">
                探索Best Practices Analyzer(BPA)，並針對包含範例違規的舊版WKND代碼庫運行它以檢查結果。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session1-differently#bootcamp---session-1-introduction-and-thinking-differently" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">試用Best Practices Analyzer</span>
            </a>
        </td>
    </tr>
</table>


## 其他資源

+ [下載最佳做法分析工具](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Best*+Practices*+Analyzer*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)