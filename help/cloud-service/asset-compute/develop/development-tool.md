---
title: asset compute開發工具
description: asset compute開發工具是一種本地Web工具，允許開發人員在SDK上下文之外，針對Adobe I/O Runtime的Asset compute資源在本地配置和執行資產電腦工作AEM人員。
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

asset compute開發工具是一種本地Web工具，允許開發人員在SDK上下文之外，針對Adobe I/O Runtime的Asset compute資源在本地配置和執行資產電腦工作AEM人員。

## 運行Asset compute開發工具

asset compute開發工具可通過終端命令從Asset compute項目的根部運行：

```
$ aio app run
```

這將啟動開發工具，時間： __http://localhost:9000__，並在瀏覽器窗口中自動開啟。 要運行開發工具， [必須通過查詢參數提供有效的自動生成的devToolToken](#troubleshooting__devtooltoken)。

## 瞭解Asset compute開發工具介面{#interface}

![asset compute開發工具](./assets/development-tool/asset-compute-dev-tool.png)

1. __源檔案：__ 源檔案選擇用於：
   + 已選擇用作 `source` 傳遞給Asset compute工作進程的二進位檔案
   + 上載源檔案
1. __asset compute配置檔案定義：__ 定義要運行的Asset compute工作進程，包括參數：包括工作人員的URL端點、結果格式副本名稱和任何參數
1. __運行：__ 「運行」按鈕執行Asset compute配置檔案編輯器中定義的Asset compute配置檔案
1. __中止：__ 「中止」按鈕取消從點擊「運行」按鈕開始的執行
1. __請求/響應：__ 提供在Adobe I/O Runtime運行的Asset compute工作進程的HTTP請求和響應。 這對調試很有幫助
1. __激活日誌：__ 描述Asset compute工作人員執行情況的日誌以及任何錯誤。 此資訊也可在 `aio app run` 標準
1. __格式副本：__ 顯示執行Asset compute工作程式生成的所有格式副本
1. __devToolToken查詢參數：__ asset compute開發工具令牌需要有效 `devToolToken` 查詢參數。 每次生成新開發工具時都自動生成此令牌

### 運行自定義工作程式

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_在開發工具中運行Asset compute工作的點擊（無音頻）_

1. 確保Asset compute開發工具是使用 `aio app run` 的子菜單。
1. 在Asset compute開發工具中，上載或選擇 [示例影像檔案](../assets/samples/sample-file.jpg)
   + 確保在 __源檔案__ 下拉清單
1. 查看 __asset compute配置檔案定義__ 文本區域
   + 的 `worker` 鍵定義到已部署的Asset compute工作程式的URL
   + 的 `name` 鍵定義要生成的格式副本的名稱
   + 可以在此JSON對象中提供其他鍵/值，並且可以在工作程式中 `rendition.instructions` 對象
      + （可選）為 `size`。 `contrast` 和 `brightness`:

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

1. 點擊 __運行__ 按鈕
1. 的 __格式副本部分__ 將填充格式副本佔位符
1. 工作人員完成後，格式副本佔位符將顯示生成的格式副本

在開發工具運行時對工作代碼進行代碼更改將「熱部署」更改。 「熱部署」需要幾秒鐘，因此，在從開發工具重新運行工作程式之前，請允許完成部署。

## 疑難排解

+ [YAML縮進不正確](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize限制設定得過低](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [由於缺少private.key，開發工具無法啟動](../troubleshooting.md#missing-private-key)
+ [源檔案下拉清單不正確](../troubleshooting.md#source-files-dropdown-incorrect)
+ [缺少或無效的devToolToken查詢參數](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [無法刪除源檔案](../troubleshooting.md#unable-to-remove-source-files)
+ [返回的格式副本部分已繪製/損壞](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
