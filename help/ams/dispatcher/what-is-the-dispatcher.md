---
title: 什麼是「調度員」
description: 瞭解Dispatcher實際是什麼。
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
exl-id: 96c8dd09-e0a7-4abc-b04b-a805aaa67502
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 2%

---

# 什麼是「調度員」

[目錄](./overview.md)

從Dispatcher的基本說明開始AEM。

## Apache Web Server

從在Linux伺服器上安裝基本的Apache Web Server開始。

Apache伺服器所做操作的基本說明：

- 遵循簡單規則，從其靜態文檔目錄(`DocumentRoot`)
- 儲存在預設位置的檔案(`/var/www/html`)在請求客戶端的瀏覽器中匹配並呈現




## AEM特定模組檔案`mod_dispatcher.so`)

然後向名為Dispatcher模組的Apache Web伺服器添加插件

AdobeDispatcher模組執行的AEM基本說明：

- 增加預設檔案處理程式
- 過濾壞請求/保護軟AEM肋/端點
- 如果存在多個呈現器，則負載平衡
- 允許使用活動快取目錄/支援刷新停滯的檔案
- 它是所有AMS安裝的前門，它將網站和資產傳送到客戶端瀏覽器
- 它以遠快於伺服器自行完成的速AEM率快取重新服務請求
- 更多……

## Web通信工作流

瞭解為構建基本的Dispatcher伺服器而安裝了哪些部件，使我們能夠讓您瞭解Adobe管理器服務配置的基本Web通信工作流。
這應該有助於您瞭解它在為內容訪問者提供內容的系統鏈中所扮演的角色AEM。

<b>為已快取的內容提供服務</b>

```
End User's Browser request 
    → Cloud Provider Load Balancer 
        → "The Dispatcher" 
            → Checks for cached request locally if found 
                → return request 
                    → End User
```

<b>從中提供新內AEM容</b>

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

<b>內容發佈/更改</b>

```
AEM Author User activates content 
    → Triggers content to be replicated to Publisher 
        → Publisher gets content and triggers the flush request to Dispatcher 
            → Dispatcher invalidates changed content 
            * Next request for that content will request fresh copy from publisher *
```

[下一步 — >基本檔案佈局](./basic-file-layout.md)
