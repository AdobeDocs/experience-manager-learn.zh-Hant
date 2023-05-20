---
title: 在中使用「系統概述」控AEM制板
description: 在以前版本AEM中，管理員需要查看多個位置，才能獲得實例的完整AEM資訊。 「系統概述」旨在通過從單個儀表板中提供實例的配置、硬體和運行狀況的高AEM級視圖來解決此問題。
version: 6.4, 6.5
topics: administration, operations, monitoring
activity: use
audience: administrator, architect, developer, implementer
doc-type: technical video
contentOwner: dgordon
topic: Administration
role: Admin
level: Beginner
exl-id: af8f499c-4955-44b5-8f21-085263ca31a3
source-git-commit: 4b47daf82e27f6bea4be30e3cdd132f497f4c609
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 0%

---

# 使用系統概述儀表板

Adobe Experience ManagerAEM [!UICONTROL 系統概述] 提供了從單個儀表板查看實例的配置、硬AEM件和運行狀況的高級視圖。

>[!VIDEO](https://video.tv.adobe.com/v/21340?quality=12&learn=on)

1. 可從以下位置訪問「System Overview（系統概述）」： **開AEM始** > **[!UICONTROL 工具]** > **[!UICONTROL 操作]** > **[!UICONTROL 系統概述]**

   直接在 **`<server-host>/libs/granite/operations/content/systemoverview.html`**

1. 來自 [!UICONTROL 系統概述] 可通過按一下 [!UICONTROL 下載] 按鈕 資訊也通過以下方式公開 [!DNL REST] 終結點：
1. 下面是從中導出的JSON的示例輸出 [!UICONTROL 系統概述]:

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
