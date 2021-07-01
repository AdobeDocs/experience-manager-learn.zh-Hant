---
title: AEM Forms CS中的檔案產生微服務
description: 從外部應用程式中使用文檔生成微服務。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
feature: 文件服務
topic: 開發
kt: 7432
thumbnail: 332439.jpg
source-git-commit: 33cb3d18b744d9a8e54a87152079b42ed09212f2
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 4%

---

# 簡介

在本課程中，我們將使用檔案產生微服務，透過將資料與XDP範本合併而產生PDF。 要從外部應用程式使用這些微服務，請執行以下步驟：

1. 產生AEM技術帳戶的服務憑證
1. 從服務憑證建立JSON網站代號(JWT)，並針對存取代號交換相同的
1. 設定AEM中技術帳戶的存取權
1. 使用存取權杖進行HTTP呼叫
