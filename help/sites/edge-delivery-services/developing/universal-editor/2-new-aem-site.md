---
title: 建立 AEM 網站
description: 在 AEM Sites 中建立一個 Edge Delivery Services 網站，並可以使用通用編輯器進行編輯。
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 500
exl-id: d1ebcaf4-cea6-4820-8b05-3a0c71749d33
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '302'
ht-degree: 100%

---

# 建立 AEM 網站

AEM 網站是編輯、管理和發佈網站內容的地方。若要建立透過 Edge Delivery Services 傳遞，並以通用編輯器製作的 AEM 網站，請使用 [Edge Delivery Services 搭配 AEM 製作網站範本](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases)在 AEM Author 上建立新網站。

AEM 網站是儲存和製作網站內容的地方。最終體驗是 AEM 網站內容與[網站程式碼](./1-new-code-project.md)的結合。

![適用於 Edge Delivery Services 和通用編輯器的新 AEM 網站](./assets/2-new-aem-site/new-site.png)

依照[文件中列出的詳細步驟](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-aem-site)建立新的 AEM 網站。以下為步驟的摘要清單，包括本教學課程中所使用的值。
1. 在 AEM Author 中&#x200B;**建立新網站**。本教學課程使用以下網站命名：
   * 網站標題：`WKND (Universal Editor)`
   * 網站名稱：`aem-wknd-eds-ue`

      * 網站名稱值必須與[新增至 `paths.json`](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/path-mapping) 的網站路徑名稱相符。

2. 從 [Edge Delivery Services 搭配 AEM 製作網站範本](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases)**匯入最新的範本**。
3. **網站命名**&#x200B;應符合 GitHub 存放庫名稱，並將 GitHub URL 設定為存放庫的 URL。

## 將新網站發佈至預覽

在 AEM Author 中建立網站後，將其發佈至 Edge Delivery Services 預覽，即可在[本機開發環境](./3-local-development-environment.md)使用其內容。

1. 登入 **AEM Author** 並導覽至「**Sites**」。
2. 選取「**新網站** (`WKND (Universal Editor)`)」然後按一下「**管理出版物**」。
3. 選擇「**目標**」之下的「**預覽**」，然後按「**下一步**」。
4. 在「**包含子頁面設定**」下，選取「**包含子頁面**」，取消選取其他選項，然後按一下「**確定**」。
5. 按一下「**發佈**」即可發佈網站內容以供預覽。
6. 一旦發佈至預覽後，即可在 Edge Delivery Services 預覽環境中使用頁面 (頁面不會出現在 AEM 預覽服務上)。
