---
title: 建立可點按的影像元件
description: 在AEM FormsCloud Service中建立可點按的影像元件
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15968
badgeVersions: label="AEM Formsas a Cloud Service" before-title="false"
source-git-commit: 426020f59c7103829b7b7b74acb0ddb7159b39fa
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---

# 簡介

在Forms中使用可點按的影像，可建立更吸引人、直覺且美觀的使用者體驗。 為了撰寫本文章的目的，我們將SVG用於可點按的影像，因為它提供幾項優點，尤其是在設計彈性、效能和使用者體驗方面。
SVG可以使用Adobe Illustrator或任何免費線上工具來建立。 我已使用來自](https://simplemaps.com/resources/svg-us)簡易地圖的[USA地圖來展示使用案例。

## 使用可點選的「美國地圖」的使用案例

美國的可點按地圖可讓使用者探索特定狀態的表單提交。 當使用者按一下狀態時，將列出該狀態的提交內容，並提供開啟特定提交內容的選項。