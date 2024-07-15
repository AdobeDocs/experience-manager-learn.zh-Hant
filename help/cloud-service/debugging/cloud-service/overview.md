---
title: 為AEM as a Cloud Service除錯
description: 在自助式、可擴充的雲端基礎結構上，這要求AEM開發人員瞭解如何理解和偵錯AEM as a Cloud Service的各種面向，從建立和部署到取得執行AEM應用程式的詳細資訊。
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5346
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 8092fbb4-234f-472e-a405-8a45734b7c65
duration: 60
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 1%

---

# 為AEM as a Cloud Service除錯

AEM as a Cloud Service是以雲端原生方式使用AEM應用程式。 AEM as a Cloud Service在自助式、可擴充的雲端基礎結構上執行，這要求AEM開發人員瞭解如何瞭解和偵錯AEM as a Cloud Service的各個層面，從建立和部署到取得執行AEM應用程式的詳細資訊。

## 記錄

記錄檔提供應用程式在AEM as a Cloud Service中運作方式的詳細資訊，以及部署問題的深入分析。

[使用記錄檔對AEM as a Cloud Service除錯](./logs.md)

## 建置和部署

Adobe Cloud Manager管道透過一系列步驟部署AEM應用程式，以判斷部署到AEM as a Cloud Service時的程式碼品質和可行性。 每個步驟都可能導致失敗，因此瞭解如何偵錯組建以確定根本原因，以及如何解決任何失敗很重要。

[偵錯AEM as a Cloud Service建置和部署](./build-and-deployment.md)

## 開發人員主控台

開發人員控制檯提供各種AEM as a Cloud Service環境的資訊和內省，有助於瞭解如何在AEM as a Cloud Service中辨識您的應用程式和發揮其功能。

[使用Developer Console偵錯AEM as a Cloud Service](./developer-console.md)

## 存放庫瀏覽器

存放庫瀏覽器是一款功能強大的工具，可讓您檢視AEM的基本資料存放區，以便更輕鬆地偵錯AEM as a Cloud Service環境。 存放庫瀏覽器支援生產、暫存和開發以及作者、Publish和預覽服務上AEM的資源和屬性的唯讀檢視。

[使用存放庫瀏覽器除錯AEM as a Cloud Service](./repository-browser.md)
