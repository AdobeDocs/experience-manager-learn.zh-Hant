---
title: 設定BPA和CAM項目
description: 瞭解最佳做法分析器和雲加速管理器如何為遷移到AEMas a Cloud Service提供自定義指南。
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

# 最佳做法分析器和雲加速管理器

瞭解最佳實踐分析器(BPA)和雲加速管理器(CAM)如何為遷移到AEMas a Cloud Service提供自定義指南。 

>[!VIDEO](https://video.tv.adobe.com/v/336957?quality=12&learn=on)

## 使用BPA和CAM

![BPA和CAM高級圖](assets/bpa-cam-diagram.png)

BPA包應安裝在生產6.x環境AEM的克隆上。 雙酚A將生成一份報告，然後可以上傳到CAM中，這將為需要進行的關鍵活動提供指導，以便轉到AEMas a Cloud Service。

## 關鍵活動

+ 製作生產6.x環境的克隆。 在遷移內容和重構代碼時，擁有生產環境的克隆對於test各種工具和更改非常有價值。
+ 從 [軟體分發門戶](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html) 並安裝到AEM6.x克隆環境中。
+ 使用BPA工具生成可上載到雲加速管理器(CAM)的報告。 CAM通過 [https://experience.adobe.com/](https://experience.adobe.com/) > **Experience Manager** > **雲加速管理器**。
+ 使用CAM提供對當前代碼庫和環境進行哪些更新以便移動到AEMas a Cloud Service。

## 動手練習

通過嘗試通過實際操作所學到的知識來應用您的知識。

在嘗試動手練習之前，請確保您已觀看並瞭解上面的視頻，以及以下資料：

+ [對as a Cloud Service的不AEM同思考](./introduction.md)
+ [什麼是AEMas a Cloud Service?](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/what-is-aem-as-a-cloud-service.html?lang=en)
+ [Assets as a Cloud Service 架構](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/architecture.html?lang=en)
+ [可變和不可變內容](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/mutable-immutable.html?lang=en)
+ [開發AEMas a Cloud Service和AEM6.x](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html#developing)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session1-differently#bootcamp---session-1-introduction-and-thinking-differently"><img alt="實際練習GitHub儲存庫" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">最佳實踐分析器操作</div>
            <p style="margin:1rem 0">
                瀏覽最佳做法分析器(BPA)並查看結果，方法是針對包含示例違規的舊版WKND代碼庫運行它。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session1-differently#bootcamp---session-1-introduction-and-thinking-differently" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">試用最佳實踐分析器</span>
            </a>
        </td>
    </tr>
</table>


## 其他資源

+ [下載最佳做法分析工具](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Best*+Practices*+Analyzer*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)