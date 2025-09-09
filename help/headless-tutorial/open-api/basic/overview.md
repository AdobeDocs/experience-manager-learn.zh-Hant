---
title: AEM Headless OpenAPI教學課程 | 內容片段傳送
description: 端對端教學課程，說明如何使用AEM的OpenAPI型內容片段傳送API來建立和公開內容。
doc-type: Tutorial
mini-toc-levels: 1
jira: KT-6678
thumbnail: KT-6678.jpg
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2022-10-17T00:00:00Z
duration: 54
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 22%

---

# 開始使用OpenAPI進行AEM內容片段傳送

探索本教學課程，說明如何在Headless AEM案例中，使用AEM的OpenAPI內容片段傳送來建置和公開CMS內容，並由外部應用程式使用。 本檔案透過建立React應用程式來探索這些概念，該應用程式會顯示WKND團隊和相關成員的詳細資訊。 團隊和成員使用AEM內容片段模型建立模型，並由React應用程式使用搭配OpenAPI API的AEM內容片段傳送來使用。

![WKND Teams應用程式](./assets/overview/main.png)

本教學課程涵蓋以下主題：

* 建立專案設定
* 建立內容片段模型來建立資料模型
* 根據先前建立的模型建立內容片段
* 探索如何使用OpenAPI檔案的「Try It」功能，使用AEM內容片段傳送來查詢AEM中的內容片段
* 透過來自範例React應用程式的OpenAPI API呼叫，透過AEM內容片段傳送使用內容片段資料
* 增強React應用程式，使其可在通用編輯器中編輯

## 先決條件 {#prerequisites}

若要依照本教學課程內容進行，以下為必要條件：

* AEM Sites as a Cloud Service 
* 基本的 HTML 和 JavaScript 技能
* 必須在本機安裝以下工具：
   * [Node.js v22+](https://nodejs.org/)
   * [Git](https://git-scm.com/)
   * IDE (例如 [Microsoft® Visual Studio Code](https://code.visualstudio.com/))

### AEM as a Cloud Service環境

若要完成本教學課程，建議您擁有AEM as a Cloud Service環境的&#x200B;**AEM管理員**&#x200B;存取權。 也可以使用&#x200B;**開發**&#x200B;環境、**快速開發環境**&#x200B;或&#x200B;**沙箱程式**&#x200B;中的環境。

## 我們開始吧！

從[定義內容片段模型](1-content-fragment-models.md)開始本教學課程。

## github專案

原始程式碼和內容套件可在[AEM Headless教學課程](https://github.com/adobe/aem-tutorials) GitHub存放庫中取得。

[`main`分支包含此教學課程的最終原始程式碼](https://github.com/adobe/aem-tutorials/tree/main/headless/open-api/basic)。
每個步驟結束時的程式碼快照可做為Git標籤使用。

* 第4章開始 — React應用程式： [`headless_open-api_basic`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic//headless/open-api/basic)
* 第4章結束 — React應用程式： [`headless_open-api_basic_4-end`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_4-end//headless/open-api/basic)
* 第5章結尾 — 通用編輯器： [`headless_open-api_basic_5-end`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_5-end//headless/open-api/basic)

若您發現本教學課程或程式碼有問題，請留下 [GitHub 問題](https://github.com/adobe/aem-tutorials/issues)。
