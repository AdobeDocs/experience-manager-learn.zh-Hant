---
title: 使用「系統概述控制面板」(位AEM於
description: 在舊版管理AEM員中，為了取得實例的完整資訊，需要檢視數個位置AEM。 「系統概述」旨在透過單一儀表板提供執行個體的組態、硬體和健AEM康狀況的高階檢視，來解決此問題。
version: 6.4, 6.5
topics: administration, operations, monitoring
activity: use
audience: administrator, architect, developer, implementer
doc-type: technical video
contentOwner: dgordon
topic: 管理
role: 管理員
level: 初學者
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 1%

---


# 使用系統概述控制面板

Adobe Experience Manager(AEM)[!UICONTROL 系統概述]提供從單個儀表板查看實例配置、硬體和AEM運行狀況的高級視圖。

>[!VIDEO](https://video.tv.adobe.com/v/21340?quality=12&learn=on)

1. 可從以下位置訪問系統概述：**AEM開始** > **[!UICONTROL 工具]** > **[!UICONTROL 操作]** > **[!UICONTROL 系統概述]**

   直接在&#x200B;**`<server-host>/libs/granite/operations/content/systemoverview.html`**

1. 按一下[!UICONTROL 下載]按鈕可導出[!UICONTROL 系統概述]中的資訊。 這些資訊也通過以下[!DNL REST]端點公開：
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
