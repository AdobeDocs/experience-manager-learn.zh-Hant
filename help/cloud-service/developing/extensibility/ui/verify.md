---
title: 驗證AEM UI擴充功能
description: 瞭解在部署到生產環境之前，如何預覽、測試和驗證AEM UI擴充功能。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603, KT-13382
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: c5c1df23-1c04-4c04-b0cd-e126c31d5acc
duration: 637
source-git-commit: f48fb02887d909a102718dc5a0c4d1ecd2b1ef34
workflow-type: tm+mt
source-wordcount: '739'
ht-degree: 0%

---

# 驗證擴充功能

AEM UI擴充功能可依據擴充功能所屬的Adobe組織中的任何AEMas a Cloud Service環境進行驗證。

擴充功能測試是透過巧盡心思構建的URL來完成，它會指示AEM載入擴充功能，但僅限於該要求。

>[!VIDEO](https://video.tv.adobe.com/v/3412877?quality=12&learn=on)

>[!IMPORTANT]
>
> 上述影片會示範如何使用內容片段主控台擴充功能，以說明App Builder擴充功能應用程式預覽和驗證作業。 不過，請務必注意，涵蓋的概念可套用至所有AEM UI擴充功能。

## AEM UI URL

![AEM內容片段主控台URL](./assets/verify/content-fragment-console-url.png){align="center"}

若要建立將非生產擴充功能掛載至AEM的URL，必須取得插入擴充功能的AEM UI的URL。 導覽至AEMas a Cloud Service環境以驗證擴充功能，然後開啟要預覽擴充功能的UI。

例如，若要預覽內容片段主控台的擴充功能：

1. 登入所需的AEMas a Cloud Service環境。
1. 選取 __內容片段__ 圖示。
1. 等候在瀏覽器中載入AEM內容片段主控台。
1. 從瀏覽器的位址列複製AEM內容片段主控台的URL，看起來應該類似於：

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

在精心設計用於開發和階段驗證的URL時，會使用此URL。 如果對照其他AEM UI驗證擴充功能，請取得這些URL並套用以下相同步驟。

## 驗證本機開發組建

1. 開啟擴充功能專案根目錄的命令列。
1. 以本機App Builder應用程式身分執行AEM UI擴充功能

   ```shell
   $ aio app run
   ...
   No change to package.json was detected. No package manager install will be executed.
   
   To view your local application:
     -> https://localhost:9080
   To view your deployed application in the Experience Cloud shell:
     -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://localhost:9080
   ```

記下本機應用程式URL，如下所示 `-> https://localhost:9080`

1. 最初（以及當您看到連線錯誤時）開啟 `https://localhost:9080` （或是您的本機應用程式URL為何），並手動接受 [HTTPS憑證](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users).
1. 將下列兩個查詢引數新增至 [AEM UI的URL](#aem-ui-url)
   + `&devMode=true`
   + `&ext=<LOCAL APPLICATION URL>`，通常 `&ext=https://localhost:9080`.

   新增上述兩個查詢引數(`devMode` 和 `ext`)作為 __第一__ url中的查詢引數。 AEM可擴充UI的使用雜湊路由(`#/@wknd/aem/...`)，因此在 `#` 無法運作。

   預覽URL應如下所示：

   ```
   https://experience.adobe.com/?devMode=true&ext=https://localhost:9080&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. 將預覽URL複製並貼到瀏覽器中。

   + 您可能必須先開始，然後定期進行， [接受HTTPS憑證](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users) 本機應用程式的主機(`https://localhost:9080`)。

1. AEM UI會載入並插入其本機版本的擴充功能以進行驗證。

>[!IMPORTANT]
>
>請記住，使用此方法時，開發中的擴充功能只會影響您的體驗，而AEM UI的所有其他使用者體驗UI，而不會插入擴充功能。

## 驗證階段組建

1. 開啟擴充功能專案根目錄的命令列。
1. 請確定Stage工作區為作用中（或使用於驗證的任何工作區）。

   ```shell
   $ aio app use -w Stage
   ```

   將任何變更合併至 `.env` 和 `.aio`.

1. 部署更新的擴充功能App Builder應用程式。 如果未登入，請執行 `aio login` 第一。

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

1. 將下列兩個查詢引數新增至 [AEM UI的URL](#aem-ui-url)
   + `&devMode=true`
   + `&ext=<DEPLOYED APPLICATION URL>`

   新增上述兩個查詢引數(`devMode` 和 `ext`)作為 __第一__ URL中的查詢引數，因為可擴充的AEM UI使用雜湊路由(`#/@wknd/aem/...`)，因此在 `#` 無法運作。

   預覽URL應如下所示：

   ```
   https://experience.adobe.com/?devMode=true&ext=https://98765-123aquarat.adobeio-static.net/index.html&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. 將預覽URL複製並貼到瀏覽器中。
1. AEM內容片段主控台會插入在中部署至中繼工作區的擴充功能版本。 此Stage URL可與QA或企業使用者共用，以進行驗證。

請記住，使用此方法時，只有在透過製作階段URL存取時，階段擴充功能才會插入到AEM內容片段主控台的。

1. 部署後的擴充功能可透過執行 `aio app deploy` 使用預覽URL時，這些變更會自動反映。
1. 若要移除擴充功能以進行驗證，請執行 `aio app undeploy`.

## 預覽小書籤

如要輕鬆建立上述的預覽和預覽URL，可建立載入擴充功能的JavaScript書籤小程式。

下方的書籤小程式會預覽 [本機開發組建](#verify-local-development-builds) 擴充功能於 `https://localhost:9080`. 預覽 [中繼組建](#verify-stage-builds)，使用建立書籤小程式 `previewApp` 變數設為App Builder已部署應用程式的URL。

1. 在瀏覽器中建立書籤。
1. 編輯書籤。
1. 為書籤提供有意義的名稱，例如 `AEM UI Extension Preview (localhost:9080)`.
1. 將書籤的URL設定為下列程式碼：

   ```javascript
   javascript: (() => {
       /* Change this to the URL of the local App Builder app if not using https://localhost:9080 */
       const previewApp = 'https://localhost:9080';
   
       const repo = new URL(window.location.href).searchParams.get('repo');
   
       if (window.location.href.match(/https:\/\/experience\.adobe\.com\/.*\/aem\/cf\/(editor|admin)\/.*/i)) {
           window.location = `https://experience.adobe.com/?devMode=true&ext=${previewApp}&repo=${repo}${window.location.hash}`;
       } 
   })();
   ```

1. 導覽至可擴充的AEM UI，以載入預覽擴充功能，然後按一下書籤小程式。

>[!TIP]
>
> 如果App Builder擴充功能未載入，使用時， `&ext=https://localhost:9080`，直接在瀏覽器索引標籤中開啟該主機和連線埠，並接受自我簽署憑證。 然後再次嘗試小書籤。
