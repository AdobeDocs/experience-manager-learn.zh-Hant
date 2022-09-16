---
title: 為標準AEM專案原型啟用前端管道
description: 轉換標準AEM專案，使用前端管道更快速部署靜態資源，例如CSS、JavaScript、字型、圖示。 以及將前端開發與AEM上的全堆棧後端開發分開。
sub-product: sites
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
mini-toc-levels: 1
index: y
recommendations: disable
source-git-commit: 96e1c95b7cd672aa5d4f79707735abc86dae7b8a
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# 為標準AEM專案原型啟用前端管道{#enable-front-end-pipeline-standard-aem-project}

了解如何啟用以 [AEM專案原型](https://github.com/adobe/aem-project-archetype) 使用前端管道部署靜態資源（如CSS、JavaScript、字型、圖示），以加快開發到部署的週期。 以及將前端開發與AEM上的全堆棧後端開發分開。 您也可以了解這些靜態資源 __not__ 由AEM存放庫提供，但由CDN提供，這改變了傳遞范式。

新的前端管道會在Analytics Cloud Manager中建立，僅供建置和部署Adobe `ui.frontend` 對象至CDN，並通知AEM其位置。 在網頁的HTML產生期間， `<link>` 和 `<script>` 標籤會在 `href` 屬性值。

在標準AEM專案轉換後，前端開發人員可與AEM上任何完整堆疊的後端開發（有自己的部署管道）分開工作，並與之平行。

>[!VIDEO](https://video.tv.adobe.com/v/3409268)

>[!NOTE]
>
>這僅適用於AEMas a Cloud Service，不適用於AMS型AdobeCloud Manager部署。

