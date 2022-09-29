---
title: 在AEM Forms CS中使用批次API產生檔案
description: 配置並觸發批操作以生成文檔。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
exl-id: 165e2884-4399-4970-81ff-1f2f8b041a10
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# 簡介

批次請求是一次產生數以萬計、數以百計或千計類似檔案的位置。 範例：財務公司可以生成信用卡報表，發送給所有客戶。
批次API（非同步API）適用於排程的高吞吐量多檔案產生使用案例。 這些API會以批次產生檔案。 例如，每月生成電話賬單、信用卡對帳單和福利對帳單。

若要使用AEM Forms CS批次作業API，需進行下列設定

1. 配置Azure儲存帳戶
1. 建立Azure儲存備份雲配置
1. 建立批資料儲存配置
1. 執行批次API

建議您先熟悉 [API檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/batch-api.yaml?lang=en) 開始使用本教學課程之前。
