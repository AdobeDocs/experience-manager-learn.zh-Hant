---
title: 在AEM Forms CS中使用批次API產生檔案
description: 設定並觸發批次作業以產生檔案。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
exl-id: 165e2884-4399-4970-81ff-1f2f8b041a10
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
duration: 26
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 14%

---

# 簡介

批次請求是指一次產生數十、數百或數千份類似檔案。 範例：財務公司可能會產生信用卡對帳單，以傳送給其所有客戶。
批次API （非同步API）適合用於排程的高輸送量多檔案產生使用案例。 這些 API 批次產生文件。例如，每月產生的電話帳單、信用卡報表和福利報表。

若要使用AEM Forms CS批次操作API，需要以下設定

1. 設定Azure儲存體帳戶
1. 建立Azure儲存備份雲端設定
1. 建立批次資料存放區設定
1. 執行批次API

建議您先熟悉[API檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/batch-api.yaml?lang=zh-Hant)，再繼續使用此教學課程。
