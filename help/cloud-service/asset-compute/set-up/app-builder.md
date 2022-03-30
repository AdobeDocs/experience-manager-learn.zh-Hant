---
title: 設定App Builder以實現Asset compute擴展
description: asset compute項目是特別定義的App Builder項目，因此需要訪問Adobe開發者控制台中的App Builder才能設定和部署這些項目。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 2b1d8786-592e-41f2-80cc-bc0b1c7e1b49
source-git-commit: eb6a7ef343a43000855f8d5cc69bde0fae81d3e6
workflow-type: tm+mt
source-wordcount: '539'
ht-degree: 0%

---

# 設定應用生成器

asset compute項目是特別定義的App Builder項目，因此需要訪問Adobe開發者控制台中的App Builder才能設定和部署這些項目。

## 在Adobe開發人員控制台中建立和設定App Builder{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)

_按一下以設定App Builder（無音頻）_

1. 登錄到 [Adobe開發人員控制台](https://console.adobe.io) 使用與預配 [帳戶和服務](./accounts-and-services.md)。 確保您是 __系統管理員__ 或 __開發人員角色__ 的下界。
1. 通過點擊建立App Builder項目 __建立新項目>從模板建立項目> App Builder__

   _如果__&#x200B;建立新項目&#x200B;__按鈕__&#x200B;應用程式生成器&#x200B;__類型不可用，這意味著您的Adobe組織不可用 [已使用App Builder設定](#request-adobe-project-app-builder)。_

   + __項目標題__: `WKND AEM Asset Compute`
   + __應用名稱__: `wkndAemAssetCompute<YourName>`
      + 的 __應用名稱__ 必須在所有FApp Builderifly項目中都是唯一的，以後不可修改。 在公司或組織名稱前加上前置詞，並在尾碼上加上有意義的尾碼是一種很好的方法，例如： `wkndAemAssetCompute`。
      + 為了實現自我啟用，通常最好將您的名稱 __應用名稱__，例如 `wkndAemAssetComputeJaneDoe` 以避免與其他App Builder項目發生衝突。
   + 下 __工作區__ 添加名為 `Development`
   + 下 __Adobe I/O Runtime__ 確保 __包含每個工作區的運行時__ 已選中
   + 點擊 __保存__ 保存項目
1. 在App Builder項目中，選擇 `Development` 從工作區選擇器
1. 點擊 __+添加服務> API__ 開啟 __添加API__ 嚮導，使用此方法添加以下API:

   + __Experience Cloud>Asset compute__
      + 選擇 __生成密鑰對__ 點擊 __生成鍵對__ 按鈕，並保存下載的內容 `config.zip` 到安全的地點 [以後使用](#private-key)
      + 點擊 __下一個__
      + 選擇產品配置檔案 __整合 — Cloud Service__ 點擊 __保存已配置的API__
   + __Adobe Services > I/O事件__ 點擊 __保存已配置的API__
   + __Adobe Services > I/O管理API__ 點擊 __保存已配置的API__

## 訪問private.key{#private-key}

設定 [asset computeAPI整合](#set-up) 生成了新的密鑰對，並且 `config.zip` 檔案已自動下載。 此 `config.zip` 包含生成的公共證書和匹配 `private.key` 的子菜單。

1. 解壓縮 `config.zip` 將檔案系統上的安全位置 `private.key` 是 [以後](../develop/environment-variables.md)
   + 機密和私鑰永遠不應作為安全問題添加到Git中。

## 查看服務帳戶(JWT)憑據

此Adobe I/O項目的憑據由本地 [asset compute開發工具](../develop/development-tool.md) 與Adobe I/O Runtime互動，需要納入Asset compute項目。 熟悉服務帳戶(JWT)憑據。

![Adobe開發人員服務帳戶憑據](./assets/app-builder/service-account.png)

1. 在Adobe I/OProject App Builder項目中，確保 `Development` 工作區被選中
1. 點擊 __服務帳戶(JWT)__ 在 __憑據__
1. 查看顯示的Adobe I/O憑據
   + 的 __公鑰__ 在底部列出 __私有密鑰__ 對應 `config.zip` 下載時間 __asset computeAPI__ 已添加到此項目。
      + 如果私鑰丟失或被洩露，則可以刪除匹配的公鑰，並使用此介面在Adobe I/O中生成新的密鑰對或將其上載到該密鑰中。
