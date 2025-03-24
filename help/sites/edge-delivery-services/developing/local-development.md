---
title: 設定Edge Delivery Services的本機開發環境
description: 如何設定Edge Delivery Services的本機開發環境。
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Technical Video
last-substantial-update: 2023-11-15T00:00:00Z
jira: KT-14483
thumbnail: 3425717.jpeg
duration: 169
exl-id: 0f3e50f0-88d8-46be-be8b-0f547c3633a6
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '87'
ht-degree: 1%

---

# 設定本機開發環境

如何設定Edge Delivery Services開發的本機開發環境。

>[!VIDEO](https://video.tv.adobe.com/v/3425717/?learn=on)


## 影片中概述的步驟

1. 安裝AEM CLI

   ```
   $ sudo npm install -g @adobe/aem-cli
   ```

1. 將目錄變更至您的專案目錄，此目錄是從[AEM樣板](https://github.com/adobe/aem-boilerplate)範本建立的Git存放庫。

   ```
   $ git clone git@github.com:my-org/my-project.git
   $ cd my-project
   ```

1. 執行AEM CLI以啟動本機AEM執行個體。

   ```
   $ pwd
     /Users/my-user/my-project
   
   $ aem up
       ___    ________  ___                          __      __ 
      /   |  / ____/  |/  /  _____(_)___ ___  __  __/ /___ _/ /_____  _____
     / /| | / __/ / /|_/ /  / ___/ / __ `__ \/ / / / / __ `/ __/ __ \/ ___/
    / ___ |/ /___/ /  / /  (__  ) / / / / / / /_/ / / /_/ / /_/ /_/ / /
   /_/  |_/_____/_/  /_/  /____/_/_/ /_/ /_/\__,_/_/\__,_/\__/\____/_/
   
   info: Starting AEM dev server vx.x.x
   info: Local AEM dev server up and running: http://localhost:3000/
   info: Enabled reverse proxy to https://main--my-project--my-org.hlx.page
   opening default browser: http://localhost:3000/
   ```

1. 開啟http://localhost:3000/您的網頁瀏覽器，檢視您的AEM網站。
