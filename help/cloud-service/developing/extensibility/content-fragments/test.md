---
title: 測試AEM內容片段主控台擴充功能
description: 了解如何先測試AEM內容片段主控台擴充功能再部署至生產環境。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: a7b32982b547eb292384d2ebde80ba745091702a
workflow-type: tm+mt
source-wordcount: '514'
ht-degree: 0%

---


# 測試擴充功能

AEM內容片段主控台擴充功能可針對擴充功能所屬Adobe組織中的任何AEMas a Cloud Service環境進行測試。

測試擴充功能需透過精心編製的URL完成，該URL會指示AEM內容片段主控台載入擴充功能。

## AEM內容片段主控台URL

![AEM內容片段主控台URL](./assets/test/content-fragment-console-url.png){align="center"}

若要建立將非生產擴充功能載入至AEM內容片段控制台的URL，必須收集所需AEM內容片段控制台的URL。 導覽至AEMas a Cloud Service環境以測試擴充功能，並複製其AEM內容片段主控台的URL。

1. 登入所需的AEMas a Cloud Service環境。

   + 將AEM開發環境用於 [測試開發組建](#testing-development-builds)
   + 請使用AEM Stage或QA開發環境，以 [測試階段建置](#testing-stage-builds)

1. 選取 __內容片段__ 表徵圖。
1. 等候AEM內容片段主控台在瀏覽器中載入。
1. 從瀏覽器的位址列複製AEM內容片段主控台的URL，它應該類似：

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

在建立用於開發和測試的URL時，以下將使用此URL。

## 測試本機開發組建

1. 開啟命令列至擴充功能專案的根目錄。
1. 以本機應用程式產生器應用程式執行AEM內容片段主控台擴充功能

   ```shell
   $ aio app run
   ...
   No change to package.json was detected. No package manager install will be executed.
   
   To view your local application:
     -> https://localhost:9080
   To view your deployed application in the Experience Cloud shell:
     -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://localhost:9080
   ```

請注意本機應用程式URL，如上所示 `-> https://localhost:9080`

1. 將下列兩個查詢參數新增至 [AEM內容片段主控台的URL](#aem-content-fragment-console-url)
   + `&devMode=true`
   + `&ext=<LOCAL APPLICATION URL>`，通常 `&ext=https://localhost:9080`.

   測試URL應該如下所示：

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin&devMode=true&ext=https://localhost:9080
   ```

1. 將測試URL複製並貼到您的瀏覽器中。

   + 您可能必須開始，然後定期 [接受HTTPS憑證](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users) 本機應用程式的主機(`https://localhost:9080`)。

1. AEM內容片段主控台會載入本機版本的擴充功能，並插入其中以進行測試，而只要本機App Builder應用程式執行，就會熱重新載入變更。

請記住，使用此方法時，開發中的擴充功能只會影響您的體驗，而AEM內容片段主控台的所有其他使用者都不會插入擴充功能來存取它。

## 測試階段組建

1. 開啟命令列至擴充功能專案的根目錄。
1. 確保舞台工作區處於活動狀態（或用於測試的任何工作區）。

   ```shell
   $ aio app use -w Stage
   ```
   將任何變更合併至 `.env` 和 `.aio`.
1. 部署更新的擴充功能App Builder應用程式。 如果未登入，請執行 `aio login` 第一個。

   ```shell
   $ aio app deploy
   ...
   Your deployed actions:
   web actions:
     -> https://98765-123aquarat.adobeio-static.net/api/v1/web/aem-cf-console-admin-1/generic 
   To view your deployed application:
     -> https://98765-123aquarat.adobeio-static.net/index.html
   To view your deployed application in the Experience Cloud shell:
     -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://98765-123aquarat.adobeio-static.net/index.html
   New Extension Point(s) in Workspace 'Production': 'aem/cf-console-admin/1'
   Successful deployment 🏄
   ```

1. 將下列兩個查詢參數新增至 [AEM內容片段主控台的URL](#aem-content-fragment-console-url)
   + `&devMode=true`
   + `&ext=<DEPLOYED APPLICATION URL>`

   測試URL應該如下所示：

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin&devMode=true&ext=https://98765-123aquarat.adobeio-static.net/index.html
   ```

1. 將測試URL複製並貼到您的瀏覽器中。
1. AEM內容片段主控台會插入部署至中Stage Workspace的擴充功能版本。 此階段URL可以與QA或業務使用者共用以進行測試。

請記住，使用此方法時，當使用精巧的階段URL存取時，階段擴充功能只會插入到AEM內容片段主控台。

1. 已部署的擴充功能可透過執行來更新 `aio app deploy` 這些變更會在使用測試URL時自動反映。
1. 若要移除要測試的擴充功能，請執行 `aio app undeploy`.



