---
title: 在AEM FormsCS中使用批處理API生成文檔
description: 配置和觸發批處理操作以生成文檔。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
source-git-commit: 228da29e7ac0d61359c2b94131495b5b433a09dc
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# 簡介

批請求是指一次生成數以十計、數以百計或數以千計的類似文檔。 示例：財務公司可生成信用卡對帳單以發送給其所有客戶。
批處理API（非同步API）適用於計畫的高吞吐量多文檔生成使用情形。 這些API以批次形式生成文檔。 例如，每月生成電話單、信用卡對帳單和福利對帳單。

要使用AEM FormsCS批處理操作API，需要以下配置

1. 配置Azure儲存帳戶
1. 建立Azure儲存支援的雲配置
1. 建立批資料儲存配置
1. 執行批處理API

建議您熟悉 [API文檔](https://experienceleague.corp.adobe.com/docs/experience-manager-cloud-service/assets/batch-api.yaml?lang=en) 開始使用本教程。




