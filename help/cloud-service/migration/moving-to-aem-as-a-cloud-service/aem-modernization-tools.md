---
title: 使用AEM現代化工具移至AEMas a Cloud Service
description: 了解如何使用AEM現代化工具將現有的AEM專案和內容升級以as a Cloud Service相容AEM。
version: Cloud Service
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8629
thumbnail: 336965.jpeg
exl-id: 310f492c-0095-4015-81a4-27d76f288138
source-git-commit: 09f6c4b0bec10edd306270a7416fcaff8a584e76
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 5%

---


# AEM 現代化工具

了解如何使用AEM現代化工具將現有的AEM Sites內容升級為與AEMas a Cloud Service相容，並符合最佳實務。

## 多功能一體轉換器

>[!VIDEO](https://video.tv.adobe.com/v/338802/?quality=12&learn=on)

## 頁面轉換

>[!VIDEO](https://video.tv.adobe.com/v/338799/?quality=12&learn=on)

## 元件轉換

>[!VIDEO](https://video.tv.adobe.com/v/338788/?quality=12&learn=on)

## 策略導入

>[!VIDEO](https://video.tv.adobe.com/v/338797/?quality=12&learn=on)

## 使用AEM現代化工具

![AEM現代化工具生命週期](./assets/aem-modernization-tools.png)

AEM現代化工具會自動轉換由舊版靜態範本、基礎元件和parsys組成的現有AEM頁面，以使用可編輯範本、AEM核心WCM元件和版面容器等現代化方法。

## 關鍵活動

+ 複製AEM 6.x生產環境，針對
+ 下載並安裝 [最新AEM現代化工具](https://github.com/adobe/aem-modernize-tools/releases/latest) 在AEM 6.x生產克隆上（透過封裝管理器）

+ [頁面結構轉換器](https://opensource.adobe.com/aem-modernize-tools/pages/structure/about.html) 使用「配置容器」將現有頁面內容從靜態範本更新為已映射的可編輯模板
   + 使用OSGi設定定義轉換規則
   + 對現有頁面執行頁面結構轉換器

+ [元件轉換工具](https://opensource.adobe.com/aem-modernize-tools/pages/component/about.html) 使用「配置容器」將現有頁面內容從靜態範本更新為已映射的可編輯模板
   + 透過JCR節點定義/XML定義轉換規則
   + 對現有頁面執行「元件轉換工具」

+ [政策匯入工具](https://opensource.adobe.com/aem-modernize-tools/pages/policy/about.html) 從設計配置建立策略
   + 使用JCR節點定義/XML定義轉換規則
   + 根據現有設計定義運行策略導入程式
   + 將匯入的原則套用至AEM元件和容器

## 動手練習

通過這個實踐練習來嘗試你學到的東西，來運用你的知識。

在嘗試動手練習之前，請確定您已觀看並了解上述影片，以及下列材料：

+ [對AEMas a Cloud Service有不同的思考](./introduction.md)
+ [存放庫現代化](./repository-modernization.md)
+ [可變和不可變內容](../../developing/basics/mutable-immutable.md)
+ [AEM專案結構](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html)

此外，請確定您已完成先前的實作練習：

+ [BPA和CAM實作](./bpa-and-cam.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session2-migration#bootcamp---session-2-migration-methodology"><img alt="實作練習GitHub存放庫" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">實作AEM現代化</div>
            <p style="margin:1rem 0">
                探索如何使用AEM現代化工具更新舊版WKND網站，以符合AEMas a Cloud Service最佳實務。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session2-migration#bootcamp---session-2-migration-methodology" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">試用AEM現代化工具</span>
            </a>
        </td>
    </tr>
</table>

## 其他資源

+ [下載AEM現代化工具](https://github.com/adobe/aem-modernize-tools/releases/latest)
+ [AEM現代化工具檔案](https://opensource.adobe.com/aem-modernize-tools/)
+ [AEM Gems -AEM現代化套件簡介](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Introducing-the-AEM-Modernization-Suite.html)

1. 在本機AEM SDK上部署新近更新的wknd-legacy網站。 AEM ASK可在以下位置下載：
   + [Software Distribution入口網站](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html).
