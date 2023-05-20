---
title: 在AEMas a Cloud Service中搜索和索引
description: 瞭解AEMas a Cloud Service的搜索索引、如何轉換AEM6個索引定義以及如何部署索引。
version: Cloud Service
feature: Search
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8634
thumbnail: 336963.jpeg
exl-id: f752df86-27d4-4dbf-a3cb-ee97b7d9a17e
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '309'
ht-degree: 2%

---

# 搜索和索引

瞭解AEMas a Cloud Service的搜索索引、如何將AEM6個索引定義轉換為AEMas a Cloud Service相容，以及如何將索引部署到AEMas a Cloud Service。

>[!VIDEO](https://video.tv.adobe.com/v/336963?quality=12&learn=on)

## 索引轉換工具

![索引轉換工具](./assets/index-converter.png)

作為重構代碼庫的一部分，使用 [索引轉換工具](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) 將自定義Oak索引定義轉換為AEMas a Cloud Service相容的索引定義。

## 關鍵活動

+ 使用 [Adobe I/O工作流遷移程式](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) 工具，用於遷移資產處理工作流以使用Asset compute微服務。
+ 設定 [地方開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hant) 部署自定義索引。 確保更新的索引是最新的。
+ 將更新的代碼庫部署到AEMas a Cloud Service開發環境並繼續驗證。
+ 如果修改出廠設定索引 **始終** 從最新版本上運行的AEMas a Cloud Service環境中複製最新索引定義。 修改複製的索引定義以滿足您的需要。

## 動手練習

通過嘗試通過實際操作所學到的知識來應用您的知識。

在嘗試動手練習之前，請確保您已觀看並瞭解上面的視頻，以及以下資料：

+ [對as a Cloud Service的不AEM同思考](./introduction.md)
+ [儲存庫現代化](./repository-modernization.md)

另外，確保您已完成以前的動手練習：

+ [內容傳輸工具動手練習](./content-migration/content-transfer-tool.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing"><img alt="實際練習GitHub儲存庫" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">使用索引進行操作</div>
            <p style="margin:1rem 0">
                探索定義Oak索引並將其部署到AEMas a Cloud Service。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">嘗試編製索引</span>
            </a>
        </td>
    </tr>
</table>
