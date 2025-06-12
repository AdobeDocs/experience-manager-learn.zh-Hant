---
title: 設定 Edge Delivery Services 的本機開發環境
description: 如何設定 Edge Delivery Services 的本機開發環境。
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
workflow-type: ht
source-wordcount: '87'
ht-degree: 100%

---

# 設定本機開發環境

如何設定用於開發 Edge Delivery Services 的本機開發環境。

>[!VIDEO](https://video.tv.adobe.com/v/3425717/?learn=on)


## 影片中概述的步驟

1. 安裝 AEM CLI

   ```
   $ sudo npm install -g @adobe/aem-cli
   ```

1. 將目錄變更為您的專案目錄，該目錄是使用 [AEM 樣板專案](https://github.com/adobe/aem-boilerplate)範本所建立的 git 存放庫。

   ```
   $ git clone git@github.com:my-org/my-project.git
   $ cd my-project
   ```

1. 執行 AEM CLI 來啟動本機 AEM 實例。

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

1. 在您的網頁瀏覽器開啟 http://localhost:3000/，即可查看您的 AEM 網站。
