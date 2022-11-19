---
title: 什麼是「The Dispatcher」
description: 了解Dispatcher的實質。
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 829ad9733b4326c79b9b574b13b1d4c691abf877
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 2%

---


# 什麼是「The Dispatcher」

[目錄](./overview.md)

從AEM Dispatcher的基本說明開始。

## Apache Web Server

從在Linux伺服器上安裝基本的Apache Web Server開始。

Apache伺服器作用的基本說明：

- 遵循簡單規則，從其靜態文檔目錄(`DocumentRoot`)
- 儲存在預設位置(`/var/www/html`)符合要求，並在要求用戶端的瀏覽器中轉譯




## AEM特定模組檔案(`mod_dispatcher.so`)

然後，將外掛程式新增至名為Dispatcher模組的Apache Web Server

AdobeAEM Dispatcher模組作用的基本說明：

- 擴大預設檔案處理程式
- 篩除不當請求/保護AEM軟體/端點
- 如果存在多個轉譯器，則負載平衡
- 允許有效快取目錄/支援對停滯的檔案進行刷新
- 它是所有AMS安裝的前門，並將網站和資產提供給用戶端的瀏覽器
- 它會以比AEM伺服器自行完成更快的速度快取請求，以重新提供
- 更多……

## 網路流量工作流程

了解為了建立基本Dispatcher伺服器而將哪些片段安裝在一起，可讓您了解Adobe管理員服務設定的基本Web流量工作流程。
這應可協助您了解其在系統鏈中所扮演的角色，這些系統會將內容提供給AEM內容的訪客。

<b>提供已快取的內容</b>

```
End User's Browser request 
    → Cloud Provider Load Balancer 
        → "The Dispatcher" 
            → Checks for cached request locally if found 
                → return request 
                    → End User
```

<b>從AEM提供新內容</b>

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

[下一步 — >基本檔案佈局](./basic-file-layout.md)