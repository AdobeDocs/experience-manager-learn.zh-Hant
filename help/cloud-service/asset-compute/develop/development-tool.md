---
title: Asset Compute開發工具
description: Asset Compute開發工具是本機Web工具，可讓開發人員在AEM SDK環境之外，對Adobe I/O Runtime中的Asset Compute資源在本機設定並執行資產電腦背景工作。
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6283
thumbnail: 40241.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: cbe08570-e353-4daf-94d1-a91a8d63406d
duration: 171
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 0%

---

# Asset Compute開發工具

Asset Compute開發工具是本機Web工具，可讓開發人員在AEM SDK環境之外，對Adobe I/O Runtime中的Asset Compute資源在本機設定並執行資產電腦背景工作。

## 執行Asset Compute開發工具

Asset Compute開發工具可以透過終端機命令，從Asset Compute專案的根目錄執行：

```
$ aio app run
```

這會在&#x200B;__http://localhost:9000__&#x200B;啟動開發工具，並在瀏覽器視窗中自動開啟。 若要執行開發工具，[必須透過查詢引數](#troubleshooting__devtooltoken)提供有效、自動產生的devToolToken。

## 瞭解Asset Compute開發工具介面{#interface}

![Asset Compute開發工具](./assets/development-tool/asset-compute-dev-tool.png)

1. __Source檔案：__&#x200B;來源檔案選取專案用於：
   + 選取資產二進位檔，做為傳遞給Asset Compute背景工作程式的`source`二進位檔
   + 上傳來源檔案
1. __Asset Compute設定檔定義：__&#x200B;定義要執行的Asset Compute背景工作，包括引數：包括背景工作的URL端點、產生的轉譯名稱，以及任何引數
1. __執行：__ 「執行」按鈕會執行Asset Compute設定檔編輯器中定義的Asset Compute設定檔
1. __中止：__&#x200B;中止按鈕會取消點選[執行]按鈕所起始的執行
1. __要求/回應：__&#x200B;提供HTTP要求以及對/來自在Adobe I/O Runtime中執行的Asset Compute背景工作程式的回應。 這有助於進行除錯
1. __啟動記錄：__&#x200B;說明Asset Compute背景工作執行以及任何錯誤的記錄。 此資訊也可在`aio app run`標準輸出中取得
1. __轉譯：__&#x200B;顯示執行Asset Compute背景工作產生的所有轉譯
1. __devToolToken查詢引數：__ Asset Compute開發工具權杖需要有效的`devToolToken`查詢引數才能存在。 每次產生新的開發工具時，都會自動產生此代號

### 執行自訂背景工作

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_在開發工具中執行Asset Compute工作的點進（無音訊）_

1. 確定已使用`aio app run`命令從您的專案根目錄啟動Asset Compute開發工具。
1. 在Asset Compute開發工具中，上傳或選取[範例影像檔](../assets/samples/sample-file.jpg)
   + 確定已在&#x200B;__Source檔案__&#x200B;下拉式清單中選取檔案
1. 檢閱&#x200B;__Asset Compute設定檔定義__&#x200B;文字區域
   + `worker`索引鍵會定義已部署Asset Compute背景工作程式的URL
   + `name`索引鍵會定義要產生的轉譯名稱
   + 其他索引鍵/值可在此JSON物件中提供，並可在`rendition.instructions`物件下的背景工作中使用
      + 選擇性地新增`size`、`contrast`和`brightness`的值：

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
1. __轉譯區段__&#x200B;將填入轉譯預留位置
1. 背景工作完成後，轉譯預留位置會顯示產生的轉譯

在開發工具執行時對背景工作程式碼進行程式碼變更將「熱部署」變更。 「熱部署」需要幾秒鐘的時間，所以從開發工具重新執行工作者之前，請允許完成部署。

## 疑難排解

+ [不正確的YAML縮排](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize限制設定得太低](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [開發工具無法啟動，因為遺失private.key](../troubleshooting.md#missing-private-key)
+ [Source檔案下拉式清單不正確](../troubleshooting.md#source-files-dropdown-incorrect)
+ [devToolToken查詢引數遺失或無效](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [無法移除來源檔案](../troubleshooting.md#unable-to-remove-source-files)
+ [轉譯傳回部分繪製/損毀](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
