---
title: 將AEM除錯為雲端服務
description: 自助服務、可擴充的雲端基礎架構，這要求AEM開發人員瞭解如何將AEM視為雲端服務，從建立和部署到取得執行AEM應用程式的詳細資訊，並對其進行除錯。
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5346
translation-type: tm+mt
source-git-commit: debb65a218ae0b9e5105c3f63018358902330b34
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 1%

---


# 將AEM除錯為雲端服務

AEM作為雲端服務是運用AEM應用程式的雲端原生方式。 AEM雲端服務可在自助服務、可擴充的雲端基礎架構上執行，這需要AEM開發人員瞭解如何將AEM視為雲端服務，從建立和部署到取得執行AEM應用程式的詳細資訊，並對其進行除錯。

## 記錄檔

記錄檔提供您應用程式在AEM中如何以雲端服務的方式運作的詳細資訊，以及部署問題的深入資訊。

[使用記錄將AEM除錯為雲端服務](./logs.md)

## 建立和部署

Adobe Cloud Manager管道會透過一系列步驟來部署AEM應用程式，以決定將程式碼部署至AEM做為雲端服務時的程式碼品質和可行性。 每個步驟都可能導致失敗，因此必須瞭解如何對組建進行除錯，以判斷其根本原因，以及如何解決任何失敗。

[將AEM除錯為雲端服務建立和部署](./build-and-deployment.md)

## 開發人員控制台

「開發人員主控台」針對AEM的雲端服務環境提供多種資訊和介紹，這些資訊和介紹對於瞭解AEM中如何識別您的應用程式以及如何在AEM中以雲端服務的方式運作非常有用。

[使用Developer Console將AEM除錯為雲端服務](./developer-console.md)

## CRXDE Lite

CRXDE Lite是一套經典但功能強大的工具，可用來除錯AEM做為雲端服務開發環境。 CRXDE Lite提供一套功能，可協助除錯，從檢查所有資源和屬性、控制JCR的可變部分、調查權限和評估查詢。

[使用CRXDE Lite將AEM除錯為雲端服務](./crxde-lite.md)
