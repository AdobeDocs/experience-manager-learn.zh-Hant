---
title: 部署內AEM容片段控制台擴展
description: 瞭解如何部署內AEM容片段控制台擴展。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
exl-id: 2e37165d-c003-4206-8133-54e37ca35b8e
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '804'
ht-degree: 1%

---

# 部署擴展

要在as a Cloud Service環AEM境中使用，必須部署和批准擴展App Builder應用。

部署擴展App Builder應用時需要注意以下幾個注意事項：

+ 擴展部署到Adobe Developer控制台項目工作區。 預設工作區包括：
   + __生產__ workspace包含所有as a Cloud Service中都可用的擴展部AEM署。
   + __舞台__ 工作區用作開發人員工作區。 部署到舞台工作區的擴展在AEMas a Cloud Service中不可用。
Adobe Developer控制台工作區與as a Cloud Service的環境類AEM型沒有任何直接關聯。
+ 部署到生產工作區的擴展將在Adobe組AEM織中的所有as a Cloud Service環境中顯示，擴展位於其中。
擴展不能通過添加限制到其註冊的環境 [檢查as a Cloud Service主機名AEM的條件邏輯](https://developer.adobe.com/uix/docs/guides/publication/#enabling-extension-only-on-specific-aem-environments)。
+ 可在as a Cloud Service上使用多個擴展AEM。 Adobe建議使用每個擴展應用程式生成器應用程式來解決單個業務目標。 儘管如此，單個擴展App Builder應用可以實現多個擴展點，以支援一個共同的業務目標。

## 初始部署

要使擴展在as a Cloud Service環AEM境中可用，必須將其部署到Adobe Developer控制台。

部署過程分為兩個邏輯步驟：

1. 由開發人員將擴展App Builder應用部署到Adobe Developer控制台。
1. 由部署經理或業務所有者批准擴展。

### 部署擴展應用程式生成器應用程式

將擴展部署到生產工作區。 部署到生產工作區的擴展將自動添加到Adobe組AEM織中部署到的所有as a Cloud Service作者服務中。

1. 開啟更新的擴展App Builder應用的根目錄的命令行。
1. 確保「生產」工作區處於活動狀態

   ```shell
   $ aio app use -w Production
   ```

   將任何更改合併到 `.env` 和 `.aio`。

1. 部署已更新的擴展應用程式生成器應用程式。

   ```shell
   $ aio app deploy
   ```

#### 請求部署審批

![提交延期以供審批](./assets/deploy/submit-for-approval.png){align="center"}

1. 登錄到 [Adobe Developer控制台](https://developer.adobe.com)
1. 選擇 __控制台__
1. 導航到 __項目__
1. 選擇與擴展關聯的項目
1. 選擇 __生產__ 工作區
1. 選擇 __提交以供審批__
1. 填寫並提交表單，根據需要更新欄位。

請注意，需要表徵圖。 如果沒有表徵圖，可使用 [此表徵圖](./assets/deploy/icon.png)。

### 批准部署請求

![延期審批](./assets/deploy/adobe-exchange.png){align="center"}

1. 登錄到 [AdobeExchange](https://exchange.adobe.com/)
1. 導航到 __管理__ > __待審應用程式__
1. __審閱__ 擴展應用程式生成器應用
1. 如果擴展更改是可接受的 __接受__ 評論。 這會立即在Adobe組織AEM內的所有as a Cloud Service作者服務上插入擴展。

一旦擴展請求獲得批准，該擴展將立即在「as a Cloud Service作者」服AEM務中處於活動狀態。

## 更新擴展

更新和擴展App Builder應用與 [初始部署](#initial-deployment)，其偏差是必須首先撤消現有擴展部署。

### 撤消擴展

要部署擴展的新版本，必須先撤銷（或刪除）該擴展。 擴展被吊銷時，在控制台中不AEM可用。

1. 登錄到 [AdobeExchange](https://exchange.adobe.com/)
1. 導航到 __管理__ > __應用生成器應用__
1. __撤消__ 要更新的擴展

### 部署擴展

將擴展部署到生產工作區。 部署到生產工作區的擴展將自動添加到Adobe組AEM織中部署到的所有as a Cloud Service作者服務中。

1. 開啟更新的擴展App Builder應用的根目錄的命令行。
1. 確保「生產」工作區處於活動狀態

   ```shell
   $ aio app use -w Production
   ```

   將任何更改合併到 `.env` 和 `.aio`。

1. 部署已更新的擴展應用程式生成器應用程式。

   ```shell
   $ aio app deploy
   ```

#### 請求部署審批

![提交延期以供審批](./assets/deploy/submit-for-approval.png){align="center"}

1. 登錄到 [Adobe Developer控制台](https://developer.adobe.com)
1. 選擇 __控制台__
1. 導航到 __項目__
1. 選擇與擴展關聯的項目
1. 選擇 __生產__ 工作區
1. 選擇 __提交以供審批__
1. 填寫並提交表單，根據需要更新欄位。

#### 批准部署請求

![延期審批](./assets/deploy/adobe-exchange.png){align="center"}

1. 登錄到 [AdobeExchange](https://exchange.adobe.com/)
1. 導航到 __管理__ > __待審應用程式__
1. __審閱__ 擴展應用程式生成器應用
1. 如果擴展更改是可接受的 __接受__ 評論。 這會立即在Adobe組織AEM內的所有as a Cloud Service作者服務上插入擴展。

一旦擴展請求獲得批准，該擴展將立即在「as a Cloud Service作者」服AEM務中處於活動狀態。

## 刪除擴展

![刪除擴展](./assets/deploy/revoke.png)

要刪除擴展，請從AdobeExchange中撤消（或刪除）它。 撤消擴展後，將從所有as a Cloud Service作者服AEM務中刪除該擴展。

1. 登錄到 [AdobeExchange](https://exchange.adobe.com/)
1. 導航到 __管理__ > __應用生成器應用__
1. __撤消__ 要刪除的擴展
