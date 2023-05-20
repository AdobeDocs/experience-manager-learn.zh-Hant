---
title: 測試內AEM容片段控制台擴展
description: 瞭解如何在部署到生AEM產之前test內容片段控制台擴展。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
exl-id: c5c1df23-1c04-4c04-b0cd-e126c31d5acc
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---

# Test擴展

可AEM以針對擴展所屬的Adobe組AEM織中的任何as a Cloud Service環境測試內容片段控制台擴展。

通過精心編製的URL測試擴展，該URL指示AEMContent Fragment控制台載入擴展。

>[!VIDEO](https://video.tv.adobe.com/v/3412877?quality=12&learn=on)

## 內AEM容片段控制台URL

![內AEM容片段控制台URL](./assets/test/content-fragment-console-url.png){align="center"}

要建立將非生產擴展裝載到內容片段控制台AEM的URL，必須收集所AEM需內容片段控制台的URL。 導航到AEMas a Cloud Service環境以test其副檔名，並複製其內容片段控AEM制台的URL。

1. 登錄到所需的AEMas a Cloud Service環境。

   + 使用AEM開發環境 [測試開發](#testing-development-builds)
   + 使用階段AEM或QA開發環境 [測試階段構建](#testing-stage-builds)

1. 選擇 __內容片段__ 表徵圖
1. 等待內容AEM片段控制台在瀏覽器中載入。
1. 從瀏AEM覽器的地址欄複製內容片段控制台的URL，它應類似於：

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

此URL用於生成用於開發和階段測試的URL。

## Test地方發展建設

1. 開啟擴展項目根目錄的命令行。
1. 以本AEM地App Builder應用程式運行內容片段控制台擴展

   ```shell
   $ aio app run
   ...
   No change to package.json was detected. No package manager install will be executed.
   
   To view your local application:
     -> https://localhost:9080
   To view your deployed application in the Experience Cloud shell:
     -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://localhost:9080
   ```

請注意上面所示的本地應用程式URL `-> https://localhost:9080`

1. 將以下兩個查詢參數添加到 [內AEM容片段控制台的URL](#aem-content-fragment-console-url)
   + `&devMode=true`
   + `&ext=<LOCAL APPLICATION URL>`通常 `&ext=https://localhost:9080`。

   添加上述兩個查詢參數(`devMode` 和 `ext`) __第一__ 查詢URL中的參數，因為內容片段控制台使用散列路由(`#/@wknd/aem/...`)，因此在 `#` 不會奏效。

   testURL應該如下所示：

   ```
   https://experience.adobe.com/?devMode=true&ext=https://localhost:9080&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. 將testURL複製並貼上到瀏覽器中。

   + 你可能得先開始，然後定期 [接受HTTPS證書](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users) 本地應用程式的主機(`https://localhost:9080`)。

1. 內容AEM片段控制台載入時，將擴展的本地版本注入到其中以進行測試，並在本地App Builder應用程式運行期間熱重新載入更改。

>[!IMPORTANT]
>
>請記住，使用此方法時，開發中的擴展僅影響您的體驗，而Content Fragment控制台的所有其他用戶都無需注入擴展AEM即可訪問它。


## Test階段生成

1. 開啟擴展項目根目錄的命令行。
1. 確保舞台工作區處於活動狀態（或用於測試的任何工作區）。

   ```shell
   $ aio app use -w Stage
   ```

   將任何更改合併到 `.env` 和 `.aio`。

1. 部署已更新的擴展應用程式生成器應用程式。 如果未登錄，則運行 `aio login` 。

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

1. 將以下兩個查詢參數添加到 [內AEM容片段控制台的URL](#aem-content-fragment-console-url)
   + `&devMode=true`
   + `&ext=<DEPLOYED APPLICATION URL>`

   添加上述兩個查詢參數(`devMode` 和 `ext`) __第一__ 查詢URL中的參數，因為內容片段控制台使用散列路由(`#/@wknd/aem/...`)，因此在 `#` 不會奏效。

   testURL應該如下所示：

   ```
   https://experience.adobe.com/?devMode=true&ext=https://98765-123aquarat.adobeio-static.net/index.html&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. 將testURL複製並貼上到瀏覽器中。
1. 內AEM容片段控制台將部署到中的Stage工作區的擴展的版本插入。 此階段URL可以共用給QA或業務用戶以進行測試。

請記住，使用此方法時，只有在使用工藝階段URLAEM訪問時，才會在「內容片段控制台」上注入階段擴展。

1. 可通過運行來更新已部署的擴展 `aio app deploy` 同樣，使用testURL時，這些更改會自動反映。
1. 要刪除測試擴展，請運行 `aio app undeploy`。
