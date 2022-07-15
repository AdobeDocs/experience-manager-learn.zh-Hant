---
title: AEM無頭快速設定AEMas a Cloud Service
description: 無AEM頭快速設定可讓您使用WKND站點示例項目中的內AEM容與無頭進行操作，並使用通過無頭GraphQL API消耗內容的反AEM應應用。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 9442
thumbnail: 339073.jpg
exl-id: 62e807b7-b1a4-4344-9b1e-2c626b869e10
source-git-commit: b4c04a9ef7d8cfdaa5675fdfe259ab9d813fb7e0
workflow-type: tm+mt
source-wordcount: '1084'
ht-degree: 1%

---

# AEM無頭快速設定AEMas a Cloud Service

「AEM無頭」快速設定可讓您使用WKND站點示例項目中的內容與AEMHeadless進行操作，並提供一個示例反應應用(aSPA)，該應用會通過無頭圖形QL APIAEM來消耗內容。

## 必備條件

要執行此快速設定，需要執行以下操作：

+ AEMas a Cloud Service沙盒環境（最好是開發）
+ 訪問AEMas a Cloud Service和雲管理器
   + __管AEM理員__ 訪問AEMas a Cloud Service
   + __雲管理器 — 部署管理器__ 訪問雲管理器
+ 必須本地安裝以下工具：
   + [Node.js v10+](https://nodejs.org/en/)
   + [npm 6+](https://www.npmjs.com/)
   + [蠢貨](https://git-scm.com/)
   + IDE(例如， [Microsoft® Visual Studio代碼](https://code.visualstudio.com/))

## 1。建立Cloud Manager Git儲存庫

首先，建立用於部署WKND站點的Cloud Manager Git儲存庫。 WKND站點是一個示例網站項AEM目，它包含快速設定的React App使用的內AEM容（內容片段）和GraphQL終結點。

_步驟截屏_
>[!VIDEO](https://video.tv.adobe.com/v/339073/?quality=12&learn=on)

1. 導航到 [https://my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com)
1. 選擇雲管理器 __計畫__ 包含用於AEM此快速設定的as a Cloud Service環境
1. 為WKND站點項目建立Git儲存庫
   1. 選擇 __儲存庫__ 在
   1. 選擇 __添加儲存庫__ 的子菜單。
   1. 命名新Git儲存庫： `aem-headless-quick-setup-wknd`
      + 每個Adobe組織的Git儲存庫名稱必須唯一，
   1. 選擇 __保存__，並等待Git儲存庫初始化

## 2.將示例WKND站點項目推送到Cloud Manager Git儲存庫

建立Cloud Manager Git儲存庫後，從GitHub克隆WKND站點項目的原始碼，並將其推送到Cloud Manager Git儲存庫。 現在，Cloud Manager可以訪問WKND站點項目，並將其部署到AEMas a Cloud Service環境。

_步驟截屏_
>[!VIDEO](https://video.tv.adobe.com/v/339074/?quality=12&learn=on)

1. 從命令行中，從GitHub克隆示例WKND站點項目的原始碼

   ```shell
   $ mkdir -p ~/Code
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. 將Cloud Manager Git儲存庫添加為遠程
   1. 選擇 __儲存庫__ 在
   1. 選擇 __訪問回購資訊__ 從頂部操作欄
   1. 在中找到執行命令 __將遠程程式添加到Git儲存庫__ 命令行

      ```shell
      $ cd aem-guides-wknd
      $ git remote add adobe https://git.cloudmanager.adobe.com/<YOUR ADOBE ORGANIZATION>/aem-headless-quick-setup-wknd/
      ```

1. 將示例項目的原始碼從您的本地Git儲存庫推送到Cloud Manager Git儲存庫

   ```shell
   $ git push adobe main:main
   ```

   提示輸入憑據時，請提供 __用戶名__ 和 __密碼__ 從雲管理器 __資料庫資訊__ 模式。

## 3.將WKND站點部署到AEMas a Cloud Service

將WKND站點項目推送到Cloud Manager Git儲存庫後，無法使用Cloud Manager管道將其部署到AEMas a Cloud Service。

請記住，WKND站點項目提供的示例內容是React應用程式使用的無AEM頭GraphQL API。

_步驟截屏_
>[!VIDEO](https://video.tv.adobe.com/v/339075/?quality=12&learn=on)

1. 附加 __非生產部署管道__ 到新的Git儲存庫
   1. 選擇 __管線__ 在
   1. 選擇 __添加管線__ 從頂部操作欄
   1. 在 __配置__ 頁籤
      1. 選擇 __部署管道__ 選項
      1. 設定 __非生產管道名稱__ 至 `Dev Deployment pipeline`
      1. 選擇 __部署觸發器> On Git更改__
      1. 選擇 __重要指標故障行為>立即繼續__
      1. 選擇 __繼續__
   1. 在 __原始碼__ 頁籤
      1. 選擇 __完整堆棧代碼__ 選項
      1. 選擇 __AEMas a Cloud Service開發環境__ 從 __合格的部署環境__ 選擇框
      1. 選擇 `aem-headless-quick-setup-wknd` 的 __儲存庫__ 選擇框
      1. 選擇 `main` 從 __Git分支__ 選擇框
      1. 選擇 __保存__
1. 運行 __開發部署管道__
   1. 選擇 __概述__ 在
   1. 查找新建立的 __開發部署管道__ 的 __管線__ 節
   1. 選擇 __...__ 輸入管道的右側
   1. 選擇 __運行__，並在模式中確認
   1. 選擇 __...__ 到正在運行的管道的右側
   1. 選擇 __查看詳細資訊__
1. 從管道執行的詳細資訊中，監視進度，直到成功完成。 管道執行需要30到40分鐘。

## 4.下載並運行WKND React應用

通AEM過as a Cloud Service引導WKND站點項目的內容，下載並啟動示例WKND React App，該示例通過無頭GraphQL API消耗WKND站點的AEM內容。

_步驟截屏_
>[!VIDEO](https://video.tv.adobe.com/v/339076/?quality=12&learn=on)

1. 在命令行中，從GitHub克隆React App的原始碼。

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 開啟資料夾 `~/Code/aem-guides-wknd-graphql/react-app` 在IDE中。
1. 在IDE中，開啟檔案 `.env.development`。
1. 指向AEMas a Cloud Service __發佈__ 服務的主機URI  `REACT_APP_HOST_URI` 屬性。

   ```plain
   REACT_APP_HOST_URI=https://publish-pXXXX-eYYYY.adobeaemcloud.com
   ...
   ```

   要查找AEMas a Cloud Service發佈服務的主機URI:

   1. 在雲管理器中，選擇 __環境__ 在
   1. 選擇 __開發__ 環境
   1. 查找 __發佈服AEM務（和Dispatcher）__ 連結 __環境段__ 表
   1. 複製連結的地址，並將其用作AEMas a Cloud Service發佈服務的URI

1. 在IDE中，將更改保存到 `.env.development`
1. 從命令行運行React App

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. 本地運行的React App啟動 [http://localhost:3000](http://localhost:3000) 並顯示冒險清單，這些冒險是使用AEMHeadless的AEMGraphQL API從as a Cloud Service獲取的。

## 5.編輯內AEM容

當示例WKND React App連接到AEMHeadless GraphQL API並從其中使用內容時，在AEM Author服務中編寫內容，並查看React App的體驗如何一致更新。

_步驟截屏_
>[!VIDEO](https://video.tv.adobe.com/v/339077/?quality=12&learn=on)

1. 登錄到AEMas a Cloud Service作者服務
1. 導航到 __「資產」>「檔案」>「WKND共用」>「英語」>「冒險」__
1. 開啟 __南猶他州腳踏車__ 資料夾
1. 選擇 __南猶他州腳踏車__ 內容片段，並選擇 __編輯__ 從頂部操作欄
1. 更新內容片段的某些欄位，例如：
   + 標題: `Cycling Utah's National Parks`
   + 行程長度： `6 Days`
   + 困難： `Intermediate`
   + 價格: `3500`
   + 主映像： `/content/dam/wknd-shared/en/activities/cycling/mountain-biking.jpg`
1. 選擇 __保存__ 的子菜單。
1. 選擇 __快速發佈__ 從頂部操作欄 __...__
1. 刷新運行在上的React App [http://localhost:3000](http://localhost:3000)。
1. 在React App中，選擇現在更新的循環冒險，並驗證對內容片段所做的內容更改。

1. 在AEM Author服務中，使用相同的方法：
   1. 取消發佈現有的冒險內容片段，並驗證是否從React App體驗中刪除了它
   1. 建立並發佈新的冒險內容片段，並驗證它是否顯示在React App體驗中

   >[!TIP]
   >
   > 如果您不熟悉建立和發佈新內容或取消發佈現有內容片段，請查看上面的截屏。

## 恭喜！

恭喜！ 您已成功使AEM用無頭功能為React App提供電源！

要詳細瞭解React App如何使用來自as a Cloud Service的AEM內容，請簽出 [無頭AEM教程](../multi-step/overview.md)。 本教程將探討內容片AEM段在建立時的使用方式，以及此React App如何將其內容作為JSON使用。

### 後續步驟

+ [啟動無AEM頭教程](../multi-step/overview.md)
