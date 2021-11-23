---
title: 在AEM as a Cloud Service中搜尋和建立索引
description: 了解AEM as a Cloud Service的搜尋索引、如何轉換AEM 6索引定義，以及如何部署索引。
version: Cloud Service
feature: Search
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8634
thumbnail: 336963.jpeg
exl-id: f752df86-27d4-4dbf-a3cb-ee97b7d9a17e
source-git-commit: 1dcb66bc3535231c89f3e7fc127688fcf96f2b61
workflow-type: tm+mt
source-wordcount: '309'
ht-degree: 0%

---

# 搜索和索引

了解AEM as a Cloud Service的搜尋索引、如何將AEM 6索引定義轉換為與AEMas a Cloud Service相容，以及如何將索引部署至AEM as a Cloud Service。

>[!VIDEO](https://video.tv.adobe.com/v/336963/?quality=12&learn=on)

## 索引轉換工具

![索引轉換工具](./assets/index-converter.png)

作為重構程式碼基底的一部分，請使用 [索引轉換工具](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) 將自訂Oak索引定義轉換為AEMas a Cloud Service相容的索引定義。

## 關鍵活動

+ 使用 [Adobe I/O工作流遷移程式](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) 工具來移轉資產處理工作流程，以使用Asset compute微服務。
+ 設定 [本地開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) 並部署自定義索引。 確保更新的索引為最新。
+ 將更新的程式碼基底部署至AEMas a Cloud Service開發環境，並繼續驗證。
+ 如果修改現成可用的索引 **一律** 從最新發行版本上執行的AEMas a Cloud Service環境複製最新索引定義。 修改複製的索引定義以符合您的需求。

## 動手練習

通過這個實踐練習來嘗試你學到的東西，來運用你的知識。

在嘗試動手練習之前，請確定您已觀看並了解上述影片，以及下列材料：

+ [對AEMas a Cloud Service有不同的思考](./introduction.md)
+ [存放庫現代化](./repository-modernization.md)

此外，請確定您已完成先前的實作練習：

+ [內容轉移工具實作練習](./content-migration/content-transfer-tool.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing"><img alt="實作練習GitHub存放庫" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">使用索引進行操作</div>
            <p style="margin:1rem 0">
                探索如何定義Oak索引並將其部署至AEMas a Cloud Service。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">嘗試建立索引</span>
            </a>
        </td>
    </tr>
</table>
