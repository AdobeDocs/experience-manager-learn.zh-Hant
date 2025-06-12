---
title: 建立程式碼專案
description: 建立 Edge Delivery Services 的程式碼專案，並可以使用通用編輯器進行編輯。
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: e1fb7a58-2bba-4952-ad53-53ecf80836db
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '287'
ht-degree: 100%

---

# 建立 Edge Delivery Services 程式碼專案

若要建置 Edge Delivery Services 和通用編輯器的 AEM 網站，請使用 Adobe 的 [AEM 樣板專案 XWalk 專案範本](https://github.com/adobe-rnd/aem-boilerplate-xwalk)。此範本會建立一個新的程式碼專案，其中包含用於建立網站體驗的 CSS 和 JavaScript。此範本會建立一個新的 GitHub 存放庫，並使用 Adobe 的樣板專案程式碼及設定載入存放庫，為您的 AEM 網站專案提供堅實的基礎。

請記住，[透過 Edge Delivery Services 傳遞的 AEM 網站](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/sites/edge-delivery-services/overview)僅有用戶端 (瀏覽器) 程式碼。在 AEM Author 或 Publish 服務中不會執行網站程式碼。

![新的 Edge Delivery Services 專案](./assets/1-new-project/new-project.png)

依照[文件中列出的詳細步驟](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project)建立 Edge Delivery Services 程式碼專案，且其內容可在通用編輯器中進行編輯。以下為步驟的摘要清單，包括本教學課程中所使用的值。

1. **設定 GitHub 帳戶。**&#x200B;如果您是為組織建立專案，請確保組織擁有 GitHub 帳戶，而且您是其成員。
2. 使用 [AEM 樣板專案 XWalk 專案範本](https://github.com/adobe-rnd/aem-boilerplate-xwalk)**建立新的程式碼專案**。
3. **安裝 AEM Code Sync GitHub 應用程式**，並授予存放庫存取權。您可以[在這裡找到應用程式](https://github.com/apps/aem-code-sync)。
4. **設定新專案的`fstab.yaml`** 以指向正確的 AEM Author 服務。

   * 若要進行實驗，您可以使用較低階的 AEM as a Cloud Service 環境 (中繼或開發)，但實際的網站實施應設定為使用生產 AEM 服務。

5. **編輯新專案的`paths.json`**，將 AEM Author 服務路徑對應到您網站的根目錄。

此 Git 存放庫已原地複製到[本機開發環境](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/sites/edge-delivery-services/developing/universal-editor/3-local-development-environment)章節，以及程式碼的開發地點。
