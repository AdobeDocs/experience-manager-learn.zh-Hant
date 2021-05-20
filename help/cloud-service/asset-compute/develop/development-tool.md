---
title: asset compute開發工具
description: 「Asset compute開發工具」是本機Web工具，可讓開發人員在AEM SDK外部的本機位置，對Adobe I/O Runtime中的Asset compute資源設定並執行資產電腦背景工作。
feature: asset compute微服務
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
topic: 整合，開發
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '541'
ht-degree: 0%

---


# asset compute開發工具

「Asset compute開發工具」是本機Web工具，可讓開發人員在AEM SDK外部的本機位置，對Adobe I/O Runtime中的Asset compute資源設定並執行資產電腦背景工作。

## 執行Asset compute開發工具

您可以透過終端命令，從Asset compute專案的根目錄執行Asset compute開發工具：

```
$ aio app run
```

這會在&#x200B;__http://localhost:9000__&#x200B;啟動開發工具，並在瀏覽器視窗中自動開啟。 若要執行開發工具，必須透過查詢參數](#troubleshooting__devtooltoken)提供有效且自動產生的devToolToken。[

## 了解Asset compute開發工具介面{#interface}

![asset compute開發工具](./assets/development-tool/asset-compute-dev-tool.png)

1. __源檔案：__ 源檔案選擇用於：
   + 已選取將作為`source`二進位檔傳遞至Asset compute背景工作的資產二進位檔
   + 上傳來源檔案
1. __asset compute設定檔定義：__ 定義要執行的Asset compute背景工作，包括參數：包括工作人員的URL端點、產生的轉譯名稱以及任何參數
1. __執行：__ 「執行」按鈕會執行Asset compute設定檔編輯器中定義的Asset compute設定檔
1. __中止：__ 中止按鈕取消從點選運行按鈕開始的執行
1. __請求/回應：__ 提供在Adobe I/O Runtime中執行之Asset compute背景工作的HTTP請求與回應。這對除錯很有幫助
1. __啟用記錄：__ 說明Asset compute背景工作執行的記錄，以及任何錯誤。`aio app run`標準輸出中也提供此資訊
1. __轉譯：__ 顯示執行Asset compute背景工作產生的所有轉譯
1. __devToolToken查詢參數：__ Asset compute開發工具代號需要有 `devToolToken` 效的查詢參數存在。每次衍生新開發工具時，都會自動產生此代號

### 運行自定義工作器

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_在開發工具中執行Asset compute工作的點進（無音訊）_

1. 確保Asset compute開發工具是使用`aio app run`命令從項目根目錄啟動的。
1. 在「Asset compute開發工具」中，上傳或選取[範例影像檔案](../assets/samples/sample-file.jpg)
   + 確保在&#x200B;__源檔案__&#x200B;下拉清單中選擇了該檔案
1. 檢閱&#x200B;__Asset compute設定檔定義__&#x200B;文字區域
   + `worker`鍵定義了部署的Asset compute工作器的URL
   + `name`鍵定義要生成的格式副本的名稱
   + 此JSON物件中可提供其他索引鍵/值，且將可在`rendition.instructions`物件下的工作器中使用
      + （可選）為`size`、`contrast`和`brightness`添加值：

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

1. 點選&#x200B;__執行__&#x200B;按鈕
1. __轉譯區段__&#x200B;將填入轉譯放置器
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
