---
title: 使用內容傳輸工具進行內容遷移
description: 瞭解內容傳輸工具如何幫助您將內容從AEM6遷移到AEMas a Cloud Service。
version: Cloud Service
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8919
thumbnail: 336970.jpeg
exl-id: c51ce8e3-e83c-4f8b-a835-70335ed3a5b9
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '359'
ht-degree: 2%

---


# 內容轉移工具

瞭解內容傳輸工具如何幫助您將內容AEM從6.AEM3+遷移到as a Cloud Service。

>[!VIDEO](https://video.tv.adobe.com/v/336970?quality=12&learn=on)

## 使用內容傳輸工具

![內容傳輸工具生命週期](../assets/content-transfer-tool.png)

內容傳輸工具安裝在AEM6.3+上，並將內容傳輸AEM到as a Cloud Service。

## 關鍵活動

+ 下載 [最新內容傳輸工具](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Content*+Transfer*+Tool*&amp;1_group.propertyvalues.property=。%2Fjcr%3內容%2Fmetadata%2Fdc%3SoftwareType&amp;1_group.propertyvalues.operation=等於&amp;1_group.propertyvalues.0_values=軟體類型%3Atoling&amp;orderby=%40jcr%3Acont%2Fjcr%3AlastModified&amp;orderby.sort.st&amp;rded&amp;lay=list&amp;p.offset=0&amp;p.limit=2)。
+ 將AEM Author 6.3+最終內容傳輸到AEMas a Cloud Service作者服務。
   + 在6.3+ Author上安AEM裝內容傳輸工具，其中包含要傳輸的最終內容。
   + 分批運行內容傳輸工具，傳輸內容集。
+ 將AEM Publish 6.3+最終內容傳輸到AEMas a Cloud Service發佈服務。
   + 在6.3+發佈上安AEM裝內容傳輸工具，其中包含要傳輸的最終內容。
   + 分批運行內容傳輸工具，傳輸內容集。
+ （可選）在as a Cloud Service上「補充」內AEM容，方法是自上次內容傳輸後傳輸新內容

## 動手練習

通過嘗試通過實際操作所學到的知識來應用您的知識。

在嘗試動手練習之前，請確保您已觀看並瞭解上面的視頻，以及以下資料：

+ [AEM 現代化工具](../aem-modernization-tools.md)
+ [上線](../onboarding.md)
+ [Cloud Manager](../cloud-manager.md)

另外，確保您已完成以前的動手練習：

+ [Dispatcher親力親為練習](../dispatcher.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session6-transfercontent#cloud-acceleration-bootcamp---session-6-content"><img alt="實際練習GitHub儲存庫" src="../assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">內容傳輸工具的操作</div>
            <p style="margin:1rem 0">
                瞭解內容傳輸工具如何自動將內容從6移AEM動到AEMas a Cloud Service。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session6-transfercontent#cloud-acceleration-bootcamp---session-6-content" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">試用內容傳輸工具</span>
            </a>
        </td>
    </tr>
</table>

## 其他資源

+ [下載內容傳輸工具](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Content*+Transfer*+Tool*&amp;1_group.propertyvalues.property=。%2Fjcr%3內容%2Fmetadata%2Fdc%3SoftwareType&amp;1_group.propertyvalues.operation=等於&amp;1_group.propertyvalues.0_values=軟體類型%3Atoling&amp;orderby=%40jcr%3Acont%2Fjcr%3AlastModified&amp;orderby.sort.st&amp;rded&amp;lay=list&amp;p.offset=0&amp;p.limit=2)
+ [批量導入服務How-to視頻](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/bulk-import.html)

