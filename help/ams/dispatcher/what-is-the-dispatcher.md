---
title: 什麼是「排程程式」
description: 瞭解什麼是Dispatcher。
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 96c8dd09-e0a7-4abc-b04b-a805aaa67502
duration: 109
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 2%

---

# 什麼是「排程程式」

[目錄](./overview.md)

從要求AEM Dispatcher的基本說明開始。

## Apache Web Server

從Linux伺服器上的基本Apache Web Server安裝開始。

Apache伺服器功能的基本說明：

- 遵循簡單規則，從其靜態檔案目錄(`DocumentRoot`)
- 儲存在預設位置的檔案(`/var/www/html`)會依請求進行比對，並在請求使用者端的瀏覽器中轉譯




## AEM特定模組檔案(`mod_dispatcher.so`)

然後，將稱為Dispatcher模組的外掛程式新增到Apache Web Server

AdobeAEM Dispatcher模組功能的基本說明：

- 增加預設檔案處理常式
- 篩選出不良請求/保護AEM軟腹部/端點
- 如果存在多個轉譯器，則為負載平衡
- 允許使用動態快取目錄/支援排清停滯的檔案
- 它是所有AMS安裝的前門，並將網站和資產提供給使用者端的瀏覽器
- 它會快取要求，以比AEM伺服器自行完成快得多的速度重新服務
- 更多內容……

## 網站流量工作流程

瞭解哪些片段會一起安裝在一起以建置基本Dispatcher伺服器，可讓您瞭解Adobe管理員服務設定的基本Web流量工作流程。
這應該有助於您瞭解它在為您AEM內容的訪客提供內容的系統鏈中扮演什麼角色。

<b>提供已快取的內容</b>

```
End User's Browser request 
    → Cloud Provider Load Balancer 
        → "The Dispatcher" 
            → Checks for cached request locally if found 
                → return request 
                    → End User
```

<b>從AEM提供最新內容</b>

```
End User's Browser request 
    → Cloud Provider Load Balancer 
        → "The Dispatcher" 
            → Checks for cached request locally if NOT found 
                → requests content from publisher 
                    → publisher sends content 
                        → Dispatcher adds content to cache and replies 
                            → End User
```

<b>內容發佈/變更</b>

```
AEM Author User activates content 
    → Triggers content to be replicated to Publisher 
        → Publisher gets content and triggers the flush request to Dispatcher 
            → Dispatcher invalidates changed content 
            * Next request for that content will request fresh copy from publisher *
```

[下一個 — >基本檔案配置](./basic-file-layout.md)
