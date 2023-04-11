---
title: AEM無頭快速設定AEMas a Cloud Service
description: AEM Headless快速設定可讓您使用WKND Site範例專案中的內容，以及透過AEM Headless GraphQL API取用內容的React應用程式，來與AEM Headless進行實際操作。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 9442
thumbnail: 339073.jpg
exl-id: 62e807b7-b1a4-4344-9b1e-2c626b869e10
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '1081'
ht-degree: 1%

---

# AEM無頭快速設定AEMas a Cloud Service

AEM Headless快速設定可讓您使用WKND Site範例專案中的內容，以及透過AEM Headless GraphQL API取用內容的範例React應用程式(SPA)，來與AEM Headless進行實際操作。

## 必備條件

要執行此快速設定，需要以下操作：

+ AEMas a Cloud Service沙箱環境（最好是開發）
+ 存取AEMas a Cloud Service和Cloud Manager
   + __AEM管理員__ 存取AEMas a Cloud Service
   + __Cloud Manager — 部署管理員__ 存取Cloud Manager
+ 必須在本機安裝下列工具：
   + [Node.js v18](https://nodejs.org/en/)
   + [Git](https://git-scm.com/)
   + IDE(例如 [Microsoft® Visual Studio代碼](https://code.visualstudio.com/))

## 1.建立Cloud Manager Git存放庫

首先，建立用於部署WKND網站的Cloud Manager Git存放庫。 WKND Site是範例AEM網站專案，其中包含快速設定的React應用程式所使用的內容（內容片段）和GraphQL AEM端點。

_步驟的截屏_
>[!VIDEO](https://video.tv.adobe.com/v/339073?quality=12&learn=on)

1. 導覽至 [https://my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com)
1. 選取Cloud Manager __方案__ 包含要用於此快速設定的AEMas a Cloud Service環境
1. 為WKND站點項目建立Git儲存庫
   1. 選擇 __儲存庫__ 中
   1. 選擇 __添加儲存庫__ 在頂端動作列中
   1. 命名新的Git存放庫： `aem-headless-quick-setup-wknd`
      + 每個Adobe組織的Git存放庫名稱必須是唯一的
   1. 選擇 __儲存__，並等待Git存放庫初始化

## 2.將範例WKND網站專案推送至Cloud Manager Git存放庫

在建立Cloud Manager Git存放庫後，從GitHub複製WKND Site專案的原始碼，並推送至Cloud Manager Git存放庫。 現在，Cloud Manager可存取WKND Site專案，並部署至AEMas a Cloud Service環境。

_步驟的截屏_
>[!VIDEO](https://video.tv.adobe.com/v/339074?quality=12&learn=on)

1. 從命令列，從GitHub複製範例WKND網站專案的原始碼

   ```shell
   $ mkdir -p ~/Code
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. 將Cloud Manager Git存放庫新增為遠端
   1. 選擇 __儲存庫__ 中
   1. 選擇 __存取存放庫資訊__ 從頂端動作列
   1. 在中找到執行命令 __將遠端新增至您的Git存放庫__ 從命令行

      ```shell
      $ cd aem-guides-wknd
      $ git remote add adobe https://git.cloudmanager.adobe.com/<YOUR ADOBE ORGANIZATION>/aem-headless-quick-setup-wknd/
      ```

1. 將範例專案的原始碼從您本機的Git存放庫推送至Cloud Manager Git存放庫

   ```shell
   $ git push adobe main:main
   ```

   提示輸入憑證時，請提供 __使用者名稱__ 和 __密碼__ 從Cloud Manager的 __儲存庫資訊__ 模式。

## 3.將WKND站點部署到AEMas a Cloud Service

將WKND Site專案推送至Cloud Manager Git存放庫後，便無法使用Cloud Manager管道將其部署至AEMas a Cloud Service。

請記得，WKND Site專案提供React應用程式透過AEM Headless GraphQL API所耗用的範例內容。

_步驟的截屏_
>[!VIDEO](https://video.tv.adobe.com/v/339075?quality=12&learn=on)

1. 附加 __非生產部署管道__ 新Git存放庫
   1. 選擇 __管道__ 中
   1. 選擇 __新增管道__ 從頂端動作列
   1. 在 __設定__ 標籤
      1. 選擇 __部署管道__ 選項
      1. 設定 __非生產管道名稱__ to `Dev Deployment pipeline`
      1. 選擇 __部署觸發器> Git變更時__
      1. 選擇 __重要量度失敗行為>立即繼續__
      1. 選擇 __繼續__
   1. 在 __原始碼__ 標籤
      1. 選擇 __完整堆疊程式碼__ 選項
      1. 選取 __AEMas a Cloud Service開發環境__ 從 __合格的部署環境__ 選取方塊
      1. 選擇 `aem-headless-quick-setup-wknd` 在 __存放庫__ 選取方塊
      1. 選擇 `main` 從 __Git分支__ 選取方塊
      1. 選擇 __儲存__
1. 執行 __開發部署管道__
   1. 選擇 __概述__ 中
   1. 找出新建立的 __開發部署管道__ 在 __管道__ 節
   1. 選取 __...__ 輸入管道的右側
   1. 選擇 __執行__，並在強制回應視窗中確認
   1. 選取 __...__ 到現在管道的右側
   1. 選擇 __查看詳細資訊__
1. 從管道執行的詳細資訊，監視進度，直到成功完成。 管道執行需要30到40分鐘。

## 4.下載並運行WKND React應用

透過AEMas a Cloud Service引導程式，從WKND Site專案內容下載並啟動範例WKND React應用程式，透過AEM Headless GraphQL API取用WKND Site的內容。

_步驟的截屏_
>[!VIDEO](https://video.tv.adobe.com/v/339076?quality=12&learn=on)

1. 從命令列，從GitHub複製React應用程式的原始碼。

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 開啟資料夾 `~/Code/aem-guides-wknd-graphql/react-app` 在IDE中。
1. 在IDE中，開啟檔案 `.env.development`.
1. 指向AEMas a Cloud Service __發佈__ 服務的主機URI，來自  `REACT_APP_HOST_URI` 屬性。

   ```plain
   REACT_APP_HOST_URI=https://publish-pXXXX-eYYYY.adobeaemcloud.com
   ...
   ```

   要查找AEMas a Cloud Service發佈服務的主機URI，請執行以下操作：

   1. 在Cloud Manager中，選取 __環境__ 中
   1. 選擇 __開發__ 環境
   1. 找出 __發佈服務(AEM與Dispatcher)__ 連結 __環境區段__ 表格
   1. 複製連結的地址，並將其用作AEMas a Cloud Service發佈服務的URI

1. 在IDE中，將更改保存到 `.env.development`
1. 從命令列執行React應用程式

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. 在本機執行的React應用程式，從 [http://localhost:3000](http://localhost:3000) 並顯示冒險清單，這些冒險來源為使用AEM Headless的GraphQL API的AEM as a Cloud Service。

## 5.編輯AEM中的內容

透過範例WKND React應用程式連線至AEM Headless GraphQL API並使用其內容，在AEM Author服務中製作內容，並了解React應用程式的體驗如何一併更新。

_步驟的截屏_
>[!VIDEO](https://video.tv.adobe.com/v/339077?quality=12&learn=on)

1. 登入AEMas a Cloud Service作者服務
1. 導覽至 __「資產」 > 「檔案」 > 「WKND共用」 > 「英文」 > 「歷險」__
1. 開啟 __南猶他騎腳踏車__ 資料夾
1. 選取 __南猶他騎腳踏車__ 內容片段，然後選取 __編輯__ 從頂端動作列
1. 更新內容片段的某些欄位，例如：
   + 標題: `Cycling Utah's National Parks`
   + 行程長度： `6 Days`
   + 困難： `Intermediate`
   + 價格: `3500`
   + 主映像： `/content/dam/wknd-shared/en/activities/cycling/mountain-biking.jpg`
1. 選擇 __儲存__ 在頂端動作列中
1. 選擇 __快速發佈__ 從頂端動作列的 __...__
1. 重新整理在上執行的React應用程式 [http://localhost:3000](http://localhost:3000).
1. 在React應用程式中，選取現在更新的循環歷程，並驗證對內容片段所做的內容變更。

1. 在AEM製作服務中使用相同的方法：
   1. 取消發佈現有的冒險內容片段，然後確認其已從React應用程式體驗中移除
   1. 建立並發佈新的冒險內容片段，並驗證其是否顯示在React應用程式體驗中

   >[!TIP]
   >
   > 如果您不熟悉如何建立及發佈新內容，或不熟悉如何取消發佈現有內容片段，請觀看上方的螢幕錄影。

## 恭喜！

恭喜！您已成功使用AEM Headless來支援React應用程式！

若要詳細了解React應用程式如何從AEMas a Cloud Service取用內容，請查看 [AEM Headless教學課程](../multi-step/overview.md). 本教學課程探討AEM中內容片段的建立方式，以及此React應用程式如何以JSON取用其內容。

### 後續步驟

+ [啟動AEM Headless教學課程](../multi-step/overview.md)
