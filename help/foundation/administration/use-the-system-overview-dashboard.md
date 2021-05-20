---
title: 使用AEM中的「系統概述」控制面板
description: 在舊版AEM管理員中，需要查看數個位置，才能完整了解AEM例項。 「系統概述」旨在透過從單一控制面板提供AEM執行個體的設定、硬體和健全狀態的概觀，來解決此問題。
version: 6.4, 6.5
topics: administration, operations, monitoring
activity: use
audience: administrator, architect, developer, implementer
doc-type: technical video
contentOwner: dgordon
topic: 管理
role: Administrator
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '143'
ht-degree: 0%

---


# 使用系統概述控制面板

Adobe Experience Manager的(AEM)[!UICONTROL 系統概觀]可讓您從單一控制面板概略檢視AEM執行個體的設定、硬體和運作狀況。

>[!VIDEO](https://video.tv.adobe.com/v/21340?quality=12&learn=on)

1. 可從以下位置訪問「系統概述」：**AEM開始** > **[!UICONTROL 工具]** > **[!UICONTROL 操作]** > **[!UICONTROL 系統概述]**

   直接在&#x200B;**`<server-host>/libs/granite/operations/content/systemoverview.html`**

1. 按一下[!UICONTROL Download]按鈕可導出[!UICONTROL System Overview]中的資訊。 此資訊也會透過下列[!DNL REST]端點公開：
1. 以下是從[!UICONTROL 系統概述]匯出的JSON輸出範例：

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
