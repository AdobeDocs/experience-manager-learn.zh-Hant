---
title: AEM as a Cloud Service 偵錯
description: 在自助服務、可擴展的雲端基礎結構上執行，要求 AEM 開發人員了解如何理解 AEM as a Cloud Service 的各個面向並進行偵錯，包括建置和部署以及獲取正在執行的 AEM 應用程式的詳細資訊。
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5346
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 8092fbb4-234f-472e-a405-8a45734b7c65
duration: 60
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '314'
ht-degree: 100%

---

# AEM as a Cloud Service 偵錯

AEM as a Cloud Service 是善用 AEM 應用程式的雲端原生方法。AEM as a Cloud Service 在自助服務、可擴展的雲端基礎結構上執行，要求 AEM 開發人員了解如何理解 AEM as a Cloud Service 的各個面向並進行偵錯，包括建置和部署以及獲取正在執行的 AEM 應用程式的詳細資訊。

## 記錄

記錄會提供關於您的應用程式在 AEM as a Cloud Service 中如何運作的詳細資訊，以及對部署問題的深入分析。

[使用記錄對 AEM as a Cloud Service 進行偵錯](./logs.md)

## 建置和部署

Adobe Cloud Manager 管道會透過一系列步驟部署 AEM 應用程式，以判定部署至 AEM as a Cloud Service 時的程式碼品質和可行性。每個步驟都可能導致失敗，因此了解如何對建置過程進行偵錯來判斷根本原因，以及了解如何解決任何失敗是非常重要的。

[針對 AEM as a Cloud Service 的建置及部署進行偵錯](./build-and-deployment.md)

## Developer Console

Developer Console 會提供關於 AEM as a Cloud Service 環境的各種資訊和深入檢查的功能，有助於了解您的應用程式如何被 AEM as a Cloud Service 識別，以及其如何在 AEM as a Cloud Service 中運作。

[使用 Developer Console 對 AEM as a Cloud Service 進行偵錯](./developer-console.md)

## 存放庫瀏覽器

存放庫瀏覽器是功能強大的工具，可以了解 AEM 基礎資料儲存區的情形，因而能輕鬆對 AEM as a Cloud Service 進行偵錯。存放庫瀏覽器支援顯示在生產、中繼和開發以及製作、發佈和預覽服務中 AEM 所有資源和屬性的唯讀視圖。

[使用存放庫瀏覽器對 AEM as a Cloud Service 進行偵錯](./repository-browser.md)
