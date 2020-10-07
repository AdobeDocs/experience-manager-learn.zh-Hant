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
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '703'
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

### 來源檔案下拉式清單不正確{#troubleshooting__dev-tool-application-cache}

資產計算開發工具可能會進入其提取過時資料的狀態，在顯示錯誤項目的「來源檔案」下拉式清單 __中__ ，這個狀態最為明顯。

+ __錯誤：__ 來源檔案下拉式清單顯示不正確的項目。
+ __原因：__ 過期的快取瀏覽器狀態會導致
+ __解析度：__ 在您的瀏覽器中，完全清除瀏覽器標籤的「應用程式狀態」、瀏覽器快取、本機儲存空間和服務工作者。

### 遺失或無效的devToolToken查詢參數{#troubleshooting__devtooltoken}

+ __錯誤：__ 資產計算開發工具中的「未授權」通知
+ __原因：__`devToolToken` 遺失或無效
+ __解析度：__ 關閉「資產計算開發工具」瀏覽器窗口，終止任何通過命令啟動的運行中的「開發工具」 `aio app run` 進程，然後重新啟動「開發工具」(使用 `aio app run`)。

### 無法刪除源檔案{#troubleshooting__remove-source-files}

+ __錯誤：__ 無法從開發工具UI移除新增的來源檔案
+ __原因：__ 此功能尚未實作
+ __解析度：__ 使用中定義的憑證登入您的雲端儲存供應商 `.env`。 找出「開發工具」(也在中指定 `.env`)所使用的容器，導覽至 __來源__ ，並刪除任何來源影像。 如果刪除的來源檔案仍繼續顯示在下拉式清單中 [](#troubleshooting__dev-tool-application-cache) ,YOu可能需要執行「來源檔案」下拉式清單中概述的步驟，因為這些檔案可能會在開發工具的「應用程式狀態」中快取在本機。

   ![Microsoft Azure Blob儲存空間](./assets/development-tool/troubleshooting__remove-source-files.png)
