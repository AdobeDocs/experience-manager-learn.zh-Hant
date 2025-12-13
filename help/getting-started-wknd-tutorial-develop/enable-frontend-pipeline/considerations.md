---
title: 開發考量事項
description: 請考慮在啟用前端管道之後，對於前端和後端開發流程的影響。
version: Experience Manager as a Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: a3b27d5b-b167-4c60-af49-8f2e8d814c86
duration: 79
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 100%

---

# 開發考量事項

啟用前端管道以便僅把前端資源部署到 AEM as a Cloud Service 環境之後，對於本機 AEM 開發產生一些影響，而您必須修改 Git 分支模型。

## 目標

* 如何實現順暢無礙的前端與後端開發流程
* 檢視全堆疊和前端管道之間的相依性


## 本機開發考量事項

>[!VIDEO](https://video.tv.adobe.com/v/3409421?quality=12&learn=on)


## 已調整的開發方法

* 對於使用 AEM SDK 進行本機開發，後端開發團隊仍需要透過 `ui.frontend` 模組產生 clientlib，但是在 Cloud Manager 部署到 AEM as a Cloud Service 環境期間，您必須跳過此步驟。這樣便出現一項難題，要如何分隔[更新專案](update-project.md)章節所述的專案設定變更。

一種&#x200B;__解決方案__&#x200B;可能是調整您的 git 分支模型，並確保 AEM 專案設定變更永遠不會傳回 AEM 後端開發人員使用的&#x200B;__本機開發__&#x200B;分支中。


* 作為 AEM 專案持續增強的一部分，如果您引進新元件或更新現有元件，而該元件在 `ui.app` 和 `ui.frontend` 模組中都有變更，則您必須同時執行全堆疊和前端管道。
