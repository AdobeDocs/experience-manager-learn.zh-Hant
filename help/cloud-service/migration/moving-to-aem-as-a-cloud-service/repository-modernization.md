---
title: 存放庫現代化
description: 了解儲存庫現代化、可變和不可變內容、包結構和儲存庫現代化工具。
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

# 存放庫現代化

了解儲存庫現代化、可變和不可變內容、包結構和儲存庫現代化工具。

>[!VIDEO](https://video.tv.adobe.com/v/336958?quality=12&learn=on)

## Repository Modernizer工具

![存放庫現代化工具](./assets/repository-modernizer.png)

作為重構程式碼基底的一部分，請使用 [Repository Modernizer工具](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/repo-modernizer.html) 將6.x程式碼基底重新建構為更現代的結構。

## 關鍵活動

* 使用 [Adobe I/ORepository Modernizer](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationrepository-modernizer) 工具來重新建構專案，以符合AEMas a Cloud Service專案的預期結構。
* 手動調整及修正更新後程式碼基底中的任何建置錯誤。
* 設定 [本地開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hant) 並部署更新的程式碼庫。 迭代直到項目處於穩定狀態。
* 將更新的程式碼基底部署至AEMas a Cloud Service開發環境，並繼續驗證。
