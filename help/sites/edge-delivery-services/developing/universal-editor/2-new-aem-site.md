---
title: 建立AEM網站
description: 在AEM Sites中建立可供Edge Delivery Services使用的網站，使用通用編輯器即可編輯。
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 500
source-git-commit: e8ce91b0be577ec6cf8f3ab07ba9ff09c7e7a6ab
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 0%

---

# 建立AEM網站

AEM網站是編輯、管理和發佈網站內容的來源。 若要建立透過Edge Delivery Services傳遞並使用Universal Editor編寫的AEM網站，請使用[Edge Delivery Services搭配AEM編寫網站範本](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases)在AEM Author上建立新網站。

AEM網站是儲存及編寫網站內容的地方。 最終體驗是AEM網站內容與[網站程式碼](./1-new-code-project.md)的組合

![適用於Edge Delivery Services和通用編輯器的新AEM網站](./assets/2-new-aem-site/new-site.png)

請依照下列步驟建立新的AEM網站：

1. **在AEM作者中建立新網站**。 本教學課程使用下列網站命名：
   * 網站標題： `WKND (Universal Editor)`
   * 網站名稱： `aem-wknd-eds-ue`
2. **從具有AEM編寫網站範本](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases)的[Edge Delivery Services匯入最新的範本**。
3. **為網站命名**&#x200B;以符合GitHub存放庫名稱，並將GitHub URL設為存放庫的URL。

如需詳細指示，請參閱快速入門手冊中的[建立和編輯新的AEM網站區段](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-aem-site)。

## Publish要預覽的新網站

在AEM Author中建立網站後，將其發佈到Edge Delivery Services預覽，讓內容可供[本機開發環境](./3-local-development-environment.md)使用。

1. 登入&#x200B;**AEM作者**&#x200B;並導覽至&#x200B;**網站**。
2. 選取&#x200B;**新網站** (`WKND (Universal Editor)`)並按一下&#x200B;**管理出版物**。
3. 在&#x200B;**目的地**&#x200B;下選擇&#x200B;**預覽**，然後按一下&#x200B;**下一步**。
4. 在&#x200B;**包含子系設定**&#x200B;下，選取&#x200B;**包含子系**，取消選取其他選項，然後按一下&#x200B;**確定**。
5. 按一下&#x200B;**Publish**&#x200B;發佈網站內容以供預覽。
