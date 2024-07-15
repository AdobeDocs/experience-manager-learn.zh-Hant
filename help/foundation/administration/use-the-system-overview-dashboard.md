---
title: 使用AEM中的系統總覽儀表板
description: 在舊版AEM中，管理員需要檢視多個位置，才能取得AEM執行個體的完整資訊。 「系統概覽」旨在透過單一儀表板提供AEM執行個體的組態、硬體和執行狀況的高層級檢視，以解決此問題。
version: 6.4, 6.5
feature: Operations
doc-type: Technical Video
topic: Administration
role: Admin
level: Beginner
exl-id: af8f499c-4955-44b5-8f21-085263ca31a3
duration: 357
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 0%

---

# 使用系統概觀儀表板

Adobe Experience Manager (AEM) [!UICONTROL 系統概覽]提供從單一儀表板檢視AEM執行個體的組態、硬體及健康情況的高階檢視。

>[!VIDEO](https://video.tv.adobe.com/v/21340?quality=12&learn=on)

1. 系統概觀可從下列位置存取： **AEM開始** > **[!UICONTROL 工具]** > **[!UICONTROL 作業]** > **[!UICONTROL 系統概觀]**

   直接在&#x200B;**`<server-host>/libs/granite/operations/content/systemoverview.html`**

1. 按一下[!UICONTROL 下載]按鈕，即可匯出[!UICONTROL 系統總覽]中的資訊。 此資訊也會透過下列[!DNL REST]端點公開：
1. 以下是從[!UICONTROL 系統概覽]匯出的JSON輸出範例：

   ```json
   {
       "Health Checks": {
           "1 Critical": "System Maintenance",
           "3 Warn": "Replication Queue, Log Errors, Sling/Granite Content Access Check"
       },
       "Instance": {
           "Adobe Experience Manager": "6.4.0",
           "Run Modes": "s7connect, crx3, non-composite, author, samplecontent, crx3tar",
           "Instance Up Since": "2018-01-22 10:50:37"
       },
       "Repository": {
           "Apache Jackrabbit Oak": "1.8.0",
           "Node Store": "Segment Tar",
           "Repository Size": "0.26 GB",
           "File Data Store": "crx-quickstart/repository/datastore"
       },
       "Maintenance Tasks": {
           "Failed": "AuditLog Maintenance Task, Project Purge, Workflow Purge",
           "Succeeded": "Data Store Garbage Collection, Lucene Binaries Cleanup, Revision Clean Up, Version Purge, Purge of ad-hoc tasks"
       },
       "System Information": {
           "Mac OS X": "10.12.6",
           "System Load Average": "2.29",
           "Usable Disk Space": "89.44 GB",
           "Maximum Heap": "0.97 GB"
       },
       "Estimated Node Counts": {
           "Total": "232448",
           "Tags": "62",
           "Assets": "267",
           "Authorizables": "210",
           "Pages": "1592"
       },
       "Replication Agents": {
           "Blocked": "publish, publish2",
           "Idle": "offloading_b3deb296-9b28-4f60-8587-c06afa2e632c, offloading_outbox, offloading_reverse_b3deb296-9b28-4f60-8587-c06afa2e632c, publish_reverse, scene7, screens, screens2, test_and_target"
       },
       "Distribution Agents": {
           "Blocked": "publish"
       },
       "Workflows": {
           "Running Workflows": "15",
           "Failed Workflows": "15",
           "Failed Jobs": "150"
       },
       "Sling Jobs": {
           "Failed": "305"
       }
   }
   ```
