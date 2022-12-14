---
title: 部署AEM內容片段主控台擴充功能
description: 了解如何部署AEM內容片段主控台擴充功能。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: f19cdc7d551f20b35550e7d25bd168a2eaa43b6a
workflow-type: tm+mt
source-wordcount: '804'
ht-degree: 0%

---


# 部署擴充功能

若要在AEMas a Cloud Service環境中使用，必須部署並核准擴充功能App Builder應用程式。

部署擴充功能App Builder應用程式時，請注意以下幾點：

+ 擴充功能部署至Adobe Developer Console專案工作區。 預設工作區為：
   + __生產__ 工作區包含可在所有AEMas a Cloud Service中使用的擴充功能部署。
   + __階段__ 工作區可作為開發人員工作區。 部署至Stage工作區的擴充功能無法在AEMas a Cloud Service中使用。
Adobe Developer主控台工作區與AEMas a Cloud Service環境類型沒有任何直接關聯。
+ 部署至生產工作區的擴充功能會顯示在擴充功能所在之Adobe組織的所有AEMas a Cloud Service環境中。
擴充功能不能限制為透過新增 [檢查AEMas a Cloud Service主機名稱的條件式邏輯](https://developer.adobe.com/uix/docs/guides/publication/#enabling-extension-only-on-specific-aem-environments).
+ 可在AEM as a Cloud Service上使用多個擴充功能。 Adobe建議使用每個擴充功能App Builder應用程式來解決單一業務目標。 也就是說，單一擴充功能App Builder應用程式可實作多個擴充點，以支援共同的商業目標。

## 初始部署

若要讓擴充功能可在AEMas a Cloud Service環境中使用，必須將其部署至Adobe Developer Console。

部署過程分為兩個邏輯步驟：

1. 開發人員將擴充功能App Builder應用程式部署至Adobe Developer Console。
1. 由部署管理員或業務擁有者核准擴充功能。

### 部署擴充功能App Builder應用程式

將擴充功能部署至生產工作區。 部署至生產工作區的擴充功能會自動新增至部署擴充功能的Adobe組織中的所有AEMas a Cloud Service製作服務。

1. 開啟命令列，前往更新後擴充功能App Builder應用程式的根目錄。
1. 確保生產工作區處於作用中狀態

   ```shell
   $ aio app use -w Production
   ```

   將任何變更合併至 `.env` 和 `.aio`.

1. 部署更新的擴充功能App Builder應用程式。

   ```shell
   $ aio app deploy
   ```

#### 請求部署批准

![提交擴充功能以進行核准](./assets/deploy/submit-for-approval.png){align="center"}

1. 登入 [Adobe Developer Console](https://developer.adobe.com)
1. 選擇 __主控台__
1. 導覽至 __專案__
1. 選取與擴充功能相關聯的專案
1. 選取 __生產__ 工作區
1. 選擇 __提交以進行核准__
1. 填妥並提交表單，視需要更新欄位。

請注意，需要圖示。 如果您沒有圖示，則可使用 [此表徵圖](./assets/deploy/icon.png).

### 核准部署請求

![擴充功能核准](./assets/deploy/adobe-exchange.png){align="center"}

1. 登入 [AdobeExchange](https://exchange.adobe.com/)
1. 導覽至 __管理__ > __待審應用__
1. __檢閱__ 擴充功能App Builder應用程式
1. 如果擴充功能變更是可接受的 __接受__ 評論。 這會立即將擴充功能插入Adobe組織內的所有AEMas a Cloud Service作者服務。

擴充功能要求一經核准，擴充功能就會立即在AEMas a Cloud Service製作服務中生效。

## 更新擴充功能

更新和擴充App Builder應用程式的程式與 [初始部署](#initial-deployment)，且必須先撤銷現有擴充功能部署的偏差。

### 撤銷擴充功能

若要部署新版本的擴充功能，必須先撤銷（或移除）擴充功能。 雖然擴充功能已撤銷，但在AEM主控台中無法使用。

1. 登入 [AdobeExchange](https://exchange.adobe.com/)
1. 導覽至 __管理__ > __App Builder應用程式__
1. __撤銷__ 要更新的擴充功能

### 部署擴充功能

將擴充功能部署至生產工作區。 部署至生產工作區的擴充功能會自動新增至部署擴充功能的Adobe組織中的所有AEMas a Cloud Service製作服務。

1. 開啟命令列，前往更新後擴充功能App Builder應用程式的根目錄。
1. 確保生產工作區處於作用中狀態

   ```shell
   $ aio app use -w Production
   ```

   將任何變更合併至 `.env` 和 `.aio`.

1. 部署更新的擴充功能App Builder應用程式。

   ```shell
   $ aio app deploy
   ```

#### 請求部署批准

![提交擴充功能以進行核准](./assets/deploy/submit-for-approval.png){align="center"}

1. 登入 [Adobe Developer Console](https://developer.adobe.com)
1. 選擇 __主控台__
1. 導覽至 __專案__
1. 選取與擴充功能相關聯的專案
1. 選取 __生產__ 工作區
1. 選擇 __提交以進行核准__
1. 填妥並提交表單，視需要更新欄位。

#### 核准部署請求

![擴充功能核准](./assets/deploy/adobe-exchange.png){align="center"}

1. 登入 [AdobeExchange](https://exchange.adobe.com/)
1. 導覽至 __管理__ > __待審應用__
1. __檢閱__ 擴充功能App Builder應用程式
1. 如果擴充功能變更是可接受的 __接受__ 評論。 這會立即將擴充功能插入Adobe組織內的所有AEMas a Cloud Service作者服務。

擴充功能要求一經核准，擴充功能就會立即在AEMas a Cloud Service製作服務中生效。

## 移除擴充功能

![移除擴充功能](./assets/deploy/revoke.png)

若要移除擴充功能，請從Exchange中撤銷（或移除）該擴充功能。 撤銷擴充功能時，會從所有AEMas a Cloud Service製作服務中移除。

1. 登入 [AdobeExchange](https://exchange.adobe.com/)
1. 導覽至 __管理__ > __App Builder應用程式__
1. __撤銷__ 要移除的擴充功能
