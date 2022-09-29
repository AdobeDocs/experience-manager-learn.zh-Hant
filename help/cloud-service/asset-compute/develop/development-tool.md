---
title: asset compute開發工具
description: 「Asset compute開發工具」是本機Web工具，可讓開發人員在AEM SDK外部的本機位置，對Adobe I/O Runtime中的Asset compute資源設定並執行資產電腦背景工作。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: cbe08570-e353-4daf-94d1-a91a8d63406d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '535'
ht-degree: 0%

---

# asset compute開發工具

「Asset compute開發工具」是本機Web工具，可讓開發人員在AEM SDK外部的本機位置，對Adobe I/O Runtime中的Asset compute資源設定並執行資產電腦背景工作。

## 執行Asset compute開發工具

您可以透過終端命令，從Asset compute專案的根目錄執行Asset compute開發工具：

```
$ aio app run
```

這將啟動開發工具，網址為 __http://localhost:9000__，並在瀏覽器視窗中自動開啟。 要運行開發工具， [必須透過查詢參數提供有效且自動產生的devToolToken](#troubleshooting__devtooltoken).

## 了解Asset compute開發工具介面{#interface}

![asset compute開發工具](./assets/development-tool/asset-compute-dev-tool.png)

1. __源檔案：__ 源檔案選擇用於：
   + 已選取用作 `source` 二進位傳遞給Asset compute背景工作
   + 上傳來源檔案
1. __asset compute設定檔定義：__ 定義要運行的Asset compute工作器，包括參數：包括工作人員的URL端點、產生的轉譯名稱以及任何參數
1. __執行：__ 「運行」按鈕執行Asset compute配置檔案編輯器中定義的Asset compute配置檔案
1. __中止：__ 「中止」按鈕取消從點選「運行」按鈕開始的執行
1. __請求/回應：__ 提供在Adobe I/O Runtime中執行之Asset compute背景工作的HTTP要求和回應。 這對除錯很有幫助
1. __激活日誌：__ 描述Asset compute工作者執行的日誌，以及任何錯誤。 此資訊也可在 `aio app run` 標準輸出
1. __轉譯：__ 顯示由執行Asset compute工作器產生的所有轉譯
1. __devToolToken查詢參數：__ asset compute開發工具代號需要有效 `devToolToken` 要存在的查詢參數。 每次衍生新開發工具時，都會自動產生此代號

### 運行自定義工作器

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_在開發工具中執行Asset compute工作的點進（無音訊）_

1. 確認Asset compute開發工具是從專案根目錄中使用 `aio app run` 命令。
1. 在「Asset compute開發工具」中，上傳或選取 [範例影像檔案](../assets/samples/sample-file.jpg)
   + 確認已在 __源檔案__ 下拉式清單
1. 檢閱 __asset compute設定檔定義__ 文字區域
   + 此 `worker` 索引鍵會定義已部署Asset compute背景工作的URL
   + 此 `name` key定義了要生成的格式副本的名稱
   + 此JSON物件中可提供其他索引鍵/值，並可在背景工作中使用 `rendition.instructions` 物件
      + （可選）為 `size`, `contrast` 和 `brightness`:

         ```json
         {
             "renditions": [
                 {
                     "worker": "...",
                     "name": "rendition.png",
                     "size":"800",
                     "contrast": "0.30",
                     "brightness": "-0.15"
                 }
             ]
         }
         ```

1. 點選 __執行__ 按鈕
1. 此 __轉譯區段__ 將填充轉譯佔位符
1. 工作程式完成後，轉譯預留位置將顯示產生的轉譯

在開發工具執行期間對背景程式碼進行程式碼變更，將會「熱部署」變更。 「熱部署」需要幾秒的時間，因此，在從開發工具重新運行工作器之前，請允許完成部署。

## 疑難排解

+ [YAML縮進錯誤](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize限制設定得太低](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [由於缺少private.key，開發工具無法啟動](../troubleshooting.md#missing-private-key)
+ [源檔案下拉清單不正確](../troubleshooting.md#source-files-dropdown-incorrect)
+ [遺失或無效的devToolToken查詢參數](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [無法刪除源檔案](../troubleshooting.md#unable-to-remove-source-files)
+ [傳回的格式副本已部分繪製/損壞](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
