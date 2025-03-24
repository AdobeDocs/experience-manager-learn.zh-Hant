---
title: AEM Headless快速設定AEM as a Cloud Service
description: AEM Headless快速設定讓您運用WKND Site範例專案中的內容，以AEM Headless進行操作，以及透過AEM Headless GraphQL API使用內容的React應用程式。
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-9442
thumbnail: 339073.jpg
exl-id: 62e807b7-b1a4-4344-9b1e-2c626b869e10
duration: 781
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1078'
ht-degree: 0%

---

# AEM Headless快速設定AEM as a Cloud Service

AEM Headless快速設定可讓您使用AEM Headless的實際操作，其中包含來自WKND Site範例專案的內容，以及透過AEM Headless GraphQL API使用內容的範例React應用程式(SPA)。

## 先決條件

進行此快速設定需要下列專案：

+ AEM as a Cloud Service沙箱環境（最好是開發環境）
+ 存取AEM as a Cloud Service和Cloud Manager
   + __AEM管理員__&#x200B;存取AEM as a Cloud Service
   + __Cloud Manager — 部署管理員__&#x200B;對Cloud Manager的存取權
+ 下列工具必須安裝在本機：
   + [Node.js v18](https://nodejs.org/en/)
   + [Git](https://git-scm.com/)
   + IDE (例如，[Microsoft® Visual Studio Code](https://code.visualstudio.com/))

## 1.建立Cloud Manager Git存放庫

首先，建立用於部署WKND網站的Cloud Manager Git存放庫。 WKND網站是範例AEM網站專案，其中包含快速設定的React應用程式所使用的內容（內容片段）和GraphQL AEM端點。

_步驟的熒幕廣播_
>[!VIDEO](https://video.tv.adobe.com/v/339073?quality=12&learn=on)

1. 導覽至[https://my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com)
1. 選取包含要用於此快速設定的AEM as a Cloud Service環境的Cloud Manager __方案__
1. 為WKND網站專案建立Git存放庫
   1. 在頂端導覽中選取&#x200B;__存放庫__
   1. 在頂端動作列中選取&#x200B;__新增存放庫__
   1. 命名新的Git存放庫： `aem-headless-quick-setup-wknd`
      + 每個Adobe組織的Git存放庫名稱必須是唯一的，
   1. 選取&#x200B;__儲存__，並等待Git存放庫初始化

## 2.將範例WKND網站專案推送到Cloud Manager Git存放庫

建立Cloud Manager Git存放庫後，從GitHub複製WKND網站專案的原始程式碼，並將其推送到Cloud Manager Git存放庫。 現在，Cloud Manager可存取WKND網站專案，並將其部署至AEM as a Cloud Service環境。

_步驟的熒幕廣播_
>[!VIDEO](https://video.tv.adobe.com/v/339074?quality=12&learn=on)

1. 從命令列，從GitHub複製範例WKND網站專案的原始程式碼

   ```shell
   $ mkdir -p ~/Code
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. 將Cloud Manager Git存放庫新增為遠端
   1. 在頂端導覽中選取&#x200B;__存放庫__
   1. 從頂端動作列選取&#x200B;__存取存放庫資訊__
   1. 執行&#x200B;__中找到的命令，從命令列將遠端加入您的Git存放庫__

      ```shell
      $ cd aem-guides-wknd
      $ git remote add adobe https://git.cloudmanager.adobe.com/<YOUR ADOBE ORGANIZATION>/aem-headless-quick-setup-wknd/
      ```

1. 將範例專案的原始程式碼從您的本機Git存放庫推送到Cloud Manager Git存放庫

   ```shell
   $ git push adobe main:main
   ```

   出現認證提示時，請提供Cloud Manager __存放庫資訊__&#x200B;強制回應視窗中的&#x200B;__使用者名稱__&#x200B;和&#x200B;__密碼__。

## 3.將WKND網站部署至AEM as a Cloud Service

由於WKND網站專案已推送至Cloud Manager Git存放庫，因此無法使用Cloud Manager管道將其部署至AEM as a Cloud Service。

請記住，WKND Site專案提供React應用程式透過AEM Headless GraphQL API使用的範例內容。

_步驟的熒幕廣播_
>[!VIDEO](https://video.tv.adobe.com/v/339075?quality=12&learn=on)

1. 將&#x200B;__非生產部署管道__&#x200B;附加至新的Git存放庫
   1. 在上方導覽中選取&#x200B;__管道__
   1. 從頂端動作列中選取&#x200B;__新增管道__
   1. 在&#x200B;__組態__&#x200B;索引標籤上
      1. 選取&#x200B;__部署管道__&#x200B;選項
      1. 將&#x200B;__非生產管道名稱__&#x200B;設定為`Dev Deployment pipeline`
      1. 選取&#x200B;__部署觸發程式>開啟Git變更__
      1. 選取&#x200B;__重要量度失敗行為>立即繼續__
      1. 選取&#x200B;__繼續__
   1. 在&#x200B;__Source程式碼__&#x200B;索引標籤上
      1. 選取&#x200B;__完整棧疊代碼__&#x200B;選項
      1. 從&#x200B;__合格的部署環境__&#x200B;選取方塊中選取&#x200B;__AEM as a Cloud Service開發環境__
      1. 在&#x200B;__存放庫__&#x200B;選取方塊中選取`aem-headless-quick-setup-wknd`
      1. 從&#x200B;__Git分支__&#x200B;選取方塊中選取`main`
      1. 選取&#x200B;__儲存__
1. 執行&#x200B;__開發部署管道__
   1. 在頂端導覽中選取&#x200B;__概觀__
   1. 在&#x200B;__管道__&#x200B;區段中找出新建立的&#x200B;__開發部署管道__
   1. 選取管道專案右側的&#x200B;__...__
   1. 選取&#x200B;__執行__，並在強制回應視窗中確認
   1. 選取目前執行中管道右側的&#x200B;__...__
   1. 選取&#x200B;__檢視詳細資料__
1. 從管道執行的詳細資訊，監視進度直到成功完成。 管道執行應需要30到40分鐘之間。

## 4.下載並執行WKND React應用程式

使用WKND Site專案中的內容啟動AEM as a Cloud Service，下載並啟動範例WKND React應用程式，該應用程式透過AEM Headless GraphQL API使用WKND網站的內容。

_步驟的熒幕廣播_
>[!VIDEO](https://video.tv.adobe.com/v/339076?quality=12&learn=on)

1. 從命令列，從GitHub複製React應用程式的原始程式碼。

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 在IDE中開啟資料夾`~/Code/aem-guides-wknd-graphql/react-app`。
1. 在IDE中，開啟檔案`.env.development`。
1. 從`REACT_APP_HOST_URI`屬性指向AEM as a Cloud Service __Publish__&#x200B;服務的主機URI。

   ```plain
   REACT_APP_HOST_URI=https://publish-pXXXX-eYYYY.adobeaemcloud.com
   ...
   ```

   若要尋找AEM as a Cloud Service Publish服務的主機URI：

   1. 在Cloud Manager的頂端導覽列中選取&#x200B;__環境__
   1. 選取&#x200B;__開發__&#x200B;環境
   1. 找到&#x200B;__發佈服務(AEM和Dispatcher)__&#x200B;連結&#x200B;__環境區段__&#x200B;表格
   1. 複製連結位址，並將其用作AEM as a Cloud Service發佈服務的URI

1. 在IDE中，將變更儲存至`.env.development`
1. 從命令列，執行React應用程式

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. 本機執行的React應用程式從[http://localhost:3000](http://localhost:3000)開始，並顯示冒險清單，這些冒險是使用AEM Headless的GraphQL API從AEM as a Cloud Service取得。

## 5.在AEM中編輯內容

使用範例WKND React應用程式連線到AEM Headless GraphQL API並使用其內容，在AEM Author服務中製作內容，並檢視React應用程式的體驗如何一致更新。

_步驟的熒幕廣播_
>[!VIDEO](https://video.tv.adobe.com/v/339077?quality=12&learn=on)

1. 登入AEM as a Cloud Service Author服務
1. 導覽至&#x200B;__Assets >檔案> WKND共用>英文>冒險__
1. 開啟&#x200B;__Cycling Southern Utah__&#x200B;資料夾
1. 選取&#x200B;__Cycling Southern Utah__&#x200B;內容片段，並從上方動作列選取&#x200B;__編輯__
1. 更新內容片段的某些欄位，例如：
   + 標題： `Cycling Utah's National Parks`
   + 行程長度： `6 Days`
   + 難度： `Intermediate`
   + 價格： `3500`
   + 主要影像： `/content/dam/wknd-shared/en/activities/cycling/mountain-biking.jpg`
1. 在頂端動作列中選取&#x200B;__儲存__
1. 從頂端動作列的&#x200B;__選取__&#x200B;快速發佈&#x200B;__...__
1. 重新整理[http://localhost:3000](http://localhost:3000)上執行的React應用程式。
1. 在React應用程式中，選取現在更新的「循環冒險」，並驗證對內容片段進行的內容變更。

1. 使用相同方法，在AEM作者服務中：
   1. 取消發佈現有的冒險內容片段，並確認該片段已從React應用程式體驗中移除
   1. 建立並發佈新的冒險內容片段，並確認它出現在React應用程式體驗中

   >[!TIP]
   >
   > 如果您不熟悉如何建立和發佈新的內容片段，或無法發佈現有的內容片段，請觀看上方的熒幕擷取畫面。

## 恭喜！

恭喜！您已成功使用AEM Headless來增強React應用程式！

若要詳細瞭解React應用程式如何使用AEM as a Cloud Service的內容，請檢視[AEM Headless教學課程](../multi-step/overview.md)。 本教學課程會探索如何在AEM中建立內容片段，以及此React應用程式如何以JSON形式使用其內容。

### 後續步驟

+ [開始AEM Headless教學課程](../multi-step/overview.md)
