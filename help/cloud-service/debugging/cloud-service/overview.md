---
title: 將AEM作為Cloud Service除錯
description: 在自助式、可擴充的雲端基礎架構上，這要求AEM開發人員了解如何了解AEM as aCloud Service的各個層面，從建立和部署到取得執行AEM應用程式的詳細資訊，並對其進行除錯。
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5346
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 8092fbb4-234f-472e-a405-8a45734b7c65
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 1%

---

# 將AEM作為Cloud Service除錯

AEM as aCloud Service是運用AEM應用程式的雲端原生方式。 AEM as aCloud Service在自助服務、可擴充的雲端基礎架構上執行，這要求AEM開發人員了解如何了解AEM as aCloud Service的各個層面，從建立和部署，到取得執行AEM應用程式的詳細資訊。

## 記錄檔

記錄檔提供應用程式在AEM as aCloud Service中運作方式的詳細資訊，以及部署問題的深入分析。

[使用記錄檔將AEM作為Cloud Service除錯](./logs.md)

## 建置和部署

AdobeCloud Manager管道會透過一系列步驟來部署AEM應用程式，以決定部署至AEM作為Cloud Service時的程式碼品質和可行性。 每個步驟都可能導致失敗，因此請務必了解如何對組建進行除錯，以判斷根本原因，以及如何解決任何失敗。

[將AEM作為Cloud Service建置和部署進行偵錯](./build-and-deployment.md)

## 開發人員控制台

開發人員控制台提供AEM作為Cloud Service環境的各種資訊和介紹，這些資訊和介紹對於了解AEM如何識別應用程式以及在中作為Cloud Service的作用非常有用。

[使用開發人員主控台除錯AEM作為Cloud Service](./developer-console.md)

## CRXDE Lite

CRXDE Lite是傳統但功能強大的工具，可作為Cloud Service開發環境除錯AEM。 CRXDE Lite提供一套功能，有助於調試以檢查所有資源和屬性、操作JCR的可變部分、調查權限和評估查詢。

[將AEM作為Cloud Service進行CRXDE Lite](./crxde-lite.md)
