---
title: 收件匣自訂
description: '根據工作流程資料新增新欄，借此自訂收件匣 '
feature: 適用性表單
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
kt: 5830
topic: 開發
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 4%

---

# AEM 收件匣

AEM收件匣整合來自各種AEM元件(包括Forms工作流程)的通知和工作。 觸發包含「分配」任務步驟的表單工作流時，關聯的應用程式將作為任務列在受託人的收件箱中。
「收件箱」用戶介面提供清單和日曆視圖以查看任務。 您也可以配置檢視設定。 您可以根據各種參數來篩選任務
您可以自訂Experience Manager收件匣以變更欄的預設標題、重新排序欄的位置，以及根據工作流程的資料顯示其他欄


>[!NOTE]
>
>您必須是管理員或工作流程管理員的成員，才能自訂收件匣欄

## 欄自訂

[啟動AEM收](http://localhost:4502/aem/inbox)
件匣按一下「清單檢視」圖示，然後選取「管 _理控制_ 項」，開啟「管理控制項」，如下方 __ 螢幕擷取畫面所示

![admin-control](assets/open-customization.png)

在欄自訂UI中，您可以執行下列操作

* 刪除欄
* 重新排序欄
* 更名列

## 品牌自訂

在品牌自訂中，您可以執行下列操作

* 新增組織標誌
* 自訂頁首文字
* 自訂說明連結
* 隱藏導航選項

![收件匣品牌化](assets/branding-customization.PNG)
