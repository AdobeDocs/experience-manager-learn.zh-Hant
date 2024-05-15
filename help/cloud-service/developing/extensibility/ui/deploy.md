---
title: 部署AEM UI擴充功能
description: 瞭解如何部署AEM UI擴充功能。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: 2e37165d-c003-4206-8133-54e37ca35b8e
duration: 166
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '765'
ht-degree: 0%

---

# 部署擴充功能

若要在AEMas a Cloud Service環境中使用，必須部署和核准擴充功能App Builder應用程式。

部署擴充功能App Builder應用程式時，應注意以下幾點：

+ 擴充功能會部署至Adobe Developer Console專案工作區。 預設工作區為：
   + __生產__ 工作區包含擴充功能部署，可在所有AEMas a Cloud Service使用。
   + __階段__ 工作區就像開發人員工作區。 部署至中繼工作區的擴充功能在AEMas a Cloud Service中無法使用。
Adobe Developer Console工作區與AEMas a Cloud Service的環境型別沒有任何直接關聯。
+ 部署至生產工作區的擴充功能會顯示在該擴充功能所在的Adobe組織的所有AEMas a Cloud Service環境中。
擴充功能不可僅限於透過新增以註冊的環境 [檢查AEMas a Cloud Service主機名稱的條件式邏輯](https://developer.adobe.com/uix/docs/guides/publication/#enabling-extension-only-on-specific-aem-environments).
+ AEMas a Cloud Service可使用多個擴充功能。 Adobe建議使用每個擴充功能App Builder應用程式來解決單一業務目標。 也就是說，單一擴充功能App Builder應用程式可實作支援共同業務目標的多個擴充點。

## 初始部署

若要讓擴充功能在AEMas a Cloud Service環境中可用，必須將其部署至Adobe Developer主控台。

部署程式分為兩個邏輯步驟：

1. 開發人員將擴充功能App Builder應用程式部署至Adobe Developer主控台。
1. 部署管理員或企業所有者核准擴充功能。

### 部署擴充功能

將擴充功能部署至生產工作區。 部署至生產工作區的擴充功能會自動新增至部署該擴充功能的Adobe組織中的所有AEMas a Cloud Service作者服務。

1. 開啟命令列，指向已更新擴充功能App Builder應用程式的根目錄。
1. 確認生產工作區為作用中

   ```shell
   $ aio app use -w Production
   ```

   將任何變更合併至 `.env` 和 `.aio`.

1. 部署更新的擴充功能App Builder應用程式。

   ```shell
   $ aio app deploy
   ```

#### 要求部署核准

![提交擴充功能以供核准](./assets/deploy/submit-for-approval.png){align="center"}

1. 登入 [Adobe Developer Console](https://developer.adobe.com)
1. 選取 __主控台__
1. 瀏覽至 __專案__
1. 選取與擴充功能相關聯的專案
1. 選取 __生產__ 工作區
1. 選取 __提交以進行核准__
1. 完成並提交表單，視需要更新欄位。

### 部署核准

![擴充功能核准](./assets/deploy/adobe-exchange.png){align="center"}

1. 登入 [Adobe Exchange](https://exchange.adobe.com/)
1. 瀏覽至 __管理__ > __擱置檢閱的應用程式__
1. __檢閱__ 擴充功能App Builder應用程式
1. 如果擴充功能變更是可接受的 __Accept__ 評論。 這會立即在Adobe組織內的所有AEMas a Cloud Service作者服務上插入擴充功能。

擴充功能要求通過核准後，擴充功能會在AEMas a Cloud Service製作服務中立即啟用。

## 更新擴充功能

更新及擴充功能App Builder應用程式的程式與 [初始部署](#initial-deployment)，其中包含必須首先撤銷現有擴充功能部署的偏差。

### 撤銷擴充功能

若要部署新版本的擴充功能，必須先撤銷（或移除）。 雖然擴充功能已撤銷，但在AEM主控台中無法使用。

1. 登入 [Adobe Exchange](https://exchange.adobe.com/)
1. 瀏覽至 __管理__ > __應用程式產生器應用程式__
1. __撤銷__ 要更新的擴充功能

### 部署擴充功能

將擴充功能部署至生產工作區。 部署至生產工作區的擴充功能會自動新增至部署該擴充功能的Adobe組織中的所有AEMas a Cloud Service作者服務。

1. 開啟命令列，指向已更新擴充功能App Builder應用程式的根目錄。
1. 確認生產工作區為作用中

   ```shell
   $ aio app use -w Production
   ```

   將任何變更合併至 `.env` 和 `.aio`.

1. 部署更新的擴充功能App Builder應用程式。

   ```shell
   $ aio app deploy
   ```

#### 要求部署核准

![提交擴充功能以供核准](./assets/deploy/submit-for-approval.png){align="center"}

1. 登入 [Adobe Developer Console](https://developer.adobe.com)
1. 選取 __主控台__
1. 瀏覽至 __專案__
1. 選取與擴充功能相關聯的專案
1. 選取 __生產__ 工作區
1. 選取 __提交以進行核准__
1. 完成並提交表單，視需要更新欄位。

#### 核准部署請求

![擴充功能核准](./assets/deploy/adobe-exchange.png){align="center"}

1. 登入 [Adobe Exchange](https://exchange.adobe.com/)
1. 瀏覽至 __管理__ > __擱置檢閱的應用程式__
1. __檢閱__ 擴充功能App Builder應用程式
1. 如果擴充功能變更是可接受的 __Accept__ 評論。 這會立即在Adobe組織內的所有AEMas a Cloud Service作者服務上插入擴充功能。

擴充功能要求通過核准後，擴充功能會在AEMas a Cloud Service製作服務中立即啟用。

## 移除擴充功能

![移除擴充功能](./assets/deploy/revoke.png)

若要移除擴充功能，請將其從Adobe Exchange中撤銷（或移除）。 擴充功能撤銷時，會從所有AEMas a Cloud Service作者服務中移除。

1. 登入 [Adobe Exchange](https://exchange.adobe.com/)
1. 瀏覽至 __管理__ > __應用程式產生器應用程式__
1. __撤銷__ 要移除的擴充功能
