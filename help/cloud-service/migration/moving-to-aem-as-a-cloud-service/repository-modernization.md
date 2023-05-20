---
title: 資料庫現代化
description: 瞭解儲存庫現代化、可變和不可變的內容、包結構和儲存庫現代化器CLI工具。
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8630
thumbnail: 336958.jpeg
exl-id: e9bd9035-1f2d-4a34-a581-9c1ec2c7bc04
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 5%

---

# 資料庫現代化

瞭解儲存庫現代化、可變和不可變的內容、包結構和儲存庫現代化器CLI工具。

>[!VIDEO](https://video.tv.adobe.com/v/336958?quality=12&learn=on)

## 儲存庫現代化工具

![存放庫現代化工具](./assets/repository-modernizer.png)

作為重構代碼庫的一部分，使用 [儲存庫現代化工具](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/repo-modernizer.html) 將6.x代碼庫重組為更現代的結構。

## 關鍵活動

* 使用 [Adobe I/O儲存庫現代化器](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationrepository-modernizer) 工具，用於重新構建項目以匹配as a Cloud Service項目的AEM預期結構。
* 手動調整和修復更新的代碼庫中的任何生成錯誤。
* 設定 [地方開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hant) 並部署更新的代碼庫。 迭代直到項目處於穩定狀態。
* 將更新的代碼庫部署到AEMas a Cloud Service開發環境並繼續驗證。
