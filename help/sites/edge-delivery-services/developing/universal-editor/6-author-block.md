---
title: 製作區塊
description: 使用通用編輯器製作 Edge Delivery Services 區塊。
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 500
exl-id: ca356d38-262d-4c30-83a0-01c8a1381ee6
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '376'
ht-degree: 100%

---

# 製作區塊

將 [Teaser 區塊的 JSON](./5-new-block.md) 推送至 `teaser` 分支後，該區塊即可在 AEM 通用編輯器中進行編輯。

在開發過程中製作區塊非常重要，原因如下：

1. 可以檢查並確認區塊的定義和模型是否準確。
1. 開發人員能夠藉此檢閱作為開發基礎的區塊語義 HTML。
1. 可以將內容和語義 HTML 部署至預覽環境，支援更快速的區塊開發。

## 使用來自 `teaser` 分支的程式碼開啟通用編輯器

1. 登入 AEM Author。
2. 導覽至「**Sites**」，然後選取於[先前章節](./2-new-aem-site.md)中所建立的網站 (「WKND (通用編輯器)」)。

   ![AEM Sites](./assets/6-author-block/open-new-site.png)

3. 建立或編輯頁面以新增區塊，確保可以使用內容來支援本機開發。雖然可以在網站內的任何位置建立頁面，但通常最好是每個新作品都建立獨立的頁面。建立一個名為&#x200B;**分支**&#x200B;的新「資料夾」頁面。使用每個子頁面來支援同名 Git 分支的開發。

   ![AEM Sites - 建立分支頁面](./assets/6-author-block/branches-page-3.png)

4. 在「**分支**」頁面之下，建立一個新的頁面，標題為「**Teaser**」，與開發分支名稱相符，然後按一下「**開啟**」來編輯頁面。

   ![AEM Sites - 建立 Teaser 頁面](./assets/6-author-block/teaser-page-3.png)

5. 更新通用編輯器，在 URL 中新增 `?ref=teaser` 便可以從 `teaser` 分支載入程式碼。務必將查詢參數新增於 `#` 符號&#x200B;**之前**。

   ![通用編輯器 - 選取 Teaser 分支](./assets/6-author-block/select-branch.png)

6. 選取「**主要**」之下的第一個區段，按一下「**新增**」按鈕，然後選擇「**Teaser**」區塊。

   ![通用編輯器 - 新增區塊](./assets/6-author-block/add-teaser-2.png)

7. 在畫布上，選取最近新增的 Teaser，然後製作在右側的欄位，或使用內嵌編輯功能。

   ![通用編輯器 - 製作區塊](./assets/6-author-block/author-block.png)

8. 製作完成後，選取通用編輯器右上角的「**發佈**」按鈕，選擇「發佈至&#x200B;**預覽**」，將變更發佈至預覽環境。接著會把變更發佈至網站的 `aem.page` 網域。
   ![AEM Sites - 發佈或預覽](./assets/6-author-block/publish-to-preview.png)

9. 等待變更發佈至預覽，然後透過 [AEM CLI](./3-local-development-environment.md#install-the-aem-cli) 於 [http://localhost:3000/branches/teaser](http://localhost:3000/branches/teaser) 開啟網頁。

   ![本機網站 - 重新整理](./assets/6-author-block/preview.png)

現在，於預覽網站上能看到製作完成之 Teaser 區塊內容和語義 HTML，準備好在本機開發環境中使用 AEM CLI 進行開發。
