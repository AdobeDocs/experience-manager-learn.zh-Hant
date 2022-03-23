---
title: AEM 收件匣
description: 通過根據工作流資料添加新列來自定義收件箱
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
kt: 5830
topic: Development
role: Developer
level: Experienced
exl-id: 3e1d86ab-e0c4-45d4-b998-75a44a7e4a3f
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# AEM 收件匣

收件AEM箱整合來自各個元件(AEM包括Forms工作流)的通知和任務。 當觸發包含「分配」任務步驟的表單工作流時，關聯的應用程式將作為任務列在受分配人的收件箱中。

「收件箱」用戶介面提供清單和日曆視圖以查看任務。 您還可以配置視圖設定。 您可以根據各種參數篩選任務。

您可以自定義Experience Manager收件箱以更改列的預設標題、重新排序列的位置，並根據工作流的資料顯示附加列。

>[!NOTE]
>
>您必須是管理員或工作流管理員的成員才能自定義收件箱列

## 列自定義

[啟動收件箱AEM](http://localhost:4502/aem/inbox)
按一下 _清單視圖_ 表徵圖，然後選擇 _管理控制_ 如下面螢幕抓圖所示

![管理控制](assets/open-customization.png)

在列自定義UI中，可以執行以下操作

* 刪除列
* 重新排序列
* 更名列

## 品牌自訂

在品牌定制中，您可以執行以下操作

* 添加組織徽標
* 自訂頁首文字
* 自定義幫助連結
* 隱藏導航選項

![收件箱標籤](assets/branding-customization.PNG)
