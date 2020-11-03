---
title: 資產計算開發工具
description: Asset Compute Development Tool是本機網路工具，可讓開發人員在AEM SDK外部，針對Adobe I/O Runtime中的Asset Compute資源，在本機設定並執行Asset Computer工作者。
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 0%

---


# 資產計算開發工具

Asset Compute Development Tool是本機網路工具，可讓開發人員在AEM SDK外部，針對Adobe I/O Runtime中的Asset Compute資源，在本機設定並執行Asset Computer工作者。

## 運行資產計算開發工具

資產計算開發工具可從資產計算項目的根目錄通過終端命令運行：

```
$ aio app run
```

這會在http://localhost:9000啟動開發工具 ____，並在瀏覽器視窗中自動開啟它。 若要執行開發工具， [必須透過查詢參數提供有效、自動產生的devToolToken](#troubleshooting__devtooltoken)。

## 瞭解資產計算開發工具介面{#interface}

![資產計算開發工具](./assets/development-tool/asset-compute-dev-tool.png)

1. __源檔案：__ 源檔案選擇用於：
   + 已選擇資產二進位檔案，該二進位檔案將 `source` 作為傳遞給資產計算工作器的二進位檔案
   + 上傳來源檔案
1. __資產計算配置檔案定義：__ 定義要運行的資產計算工作器，包括參數：包括工作者的URL端點、產生的轉譯名稱，以及任何參數
1. __執行：__ 「運行」按鈕執行資產計算配置檔案，如資產計算配置檔案編輯器中定義的
1. __中止：__ 「中止」按鈕取消從點選「運行」按鈕開始的執行
1. __請求／回應：__ 提供在Adobe I/O Runtime中執行的資產計算工作者的HTTP要求和回應。 這有助於除錯
1. __啟動記錄：__ 描述資產計算工作者執行的日誌以及任何錯誤。 標準版中也提供此 `aio app run` 資訊
1. __轉譯：__ 顯示資產計算工作器執行時生成的所有轉譯
1. __devToolToken查詢參數：__ 資產計算開發工具Token需要有 `devToolToken` 效的查詢參數。 每次衍生新的開發工具時，就會自動產生此Token

### 運行自定義工作器

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_在開發工具中執行資產計算工作的點進（無音訊）_

1. 確保資產計算開發工具是使用命令從您的項目根目錄 `aio app run` 啟動。
1. 在資產計算開發工具中，上傳或選取范 [例影像檔案](../assets/samples/sample-file.jpg)
   + 確保在「來源檔案」下拉式清 __單中選取檔案__ 。
1. 複查「資 __產計算」配置檔案定義__ 「文本」區域
   + 鍵定 `worker` 義到已部署資產計算工作器的URL
   + 鍵 `name` 定義要生成的轉譯的名稱
   + 此JSON物件中可提供其他金鑰／值，而此物件下方的工作器中則提供其他金鑰/ `rendition.instructions` 值
      + （可選）為 `size`和 `contrast` 添加 `brightness`值：

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

1. 點選「執 __行__ 」按鈕
1. 「轉 __譯」區段__ ，將填入轉譯置入持有者
1. 工作者完成後，轉譯預留位置會顯示產生的轉譯

在開發工具運行時對工作程式碼進行代碼更改將「熱部署」更改。 「熱部署」需要幾秒鐘，因此在從開發工具重新運行工作器之前，請允許完成部署。

## 疑難排解

+ [錯誤的YAML縮排](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize限制設定為過低](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [由於缺少private.key，開發工具無法啟動](../troubleshooting.md#missing-private-key)
+ [來源檔案下拉式清單不正確](../troubleshooting.md#source-files-dropdown-incorrect)
+ [遺失或無效的devToolToken查詢參數](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [無法刪除源檔案](../troubleshooting.md#unable-to-remove-source-files)
+ [傳回的轉譯部分繪製／損毀](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
