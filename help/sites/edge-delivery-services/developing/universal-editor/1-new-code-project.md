---
title: 建立程式碼專案
description: 建立Edge Delivery Services的程式碼專案，可使用通用編輯器進行編輯。
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
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 1%

---

# 建立Edge Delivery Services程式碼專案

若要為Edge Delivery Services和Universal Editor建置AEM網站，請使用Adobe的[AEM Boilerplate XWalk專案範本](https://github.com/adobe-rnd/aem-boilerplate-xwalk)。 此範本會建立新的程式碼專案，專案中包含用來建立網站體驗的CSS和JavaScript。 此範本會建立新的GitHub存放庫，並載入Adobe的範本程式碼和設定，為您的AEM網站專案提供堅實的基礎。

請記住，由Edge Delivery Services[&#128279;](https://experienceleague.adobe.com/en/docs/experience-manager-learn/sites/edge-delivery-services/overview)傳送的AEM網站只有使用者端（瀏覽器）代碼。 網站程式碼不會在AEM製作或發佈服務中執行。

![新Edge Delivery Services專案](./assets/1-new-project/new-project.png)

請依照檔案[&#128279;](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project)中概述的詳細步驟，建立可在通用編輯器中編輯其內容的Edge Delivery Services程式碼專案。  以下是步驟的摘要清單，包括本教學課程中使用的值。

1. **設定GitHub帳戶。**&#x200B;如果您要為貴組織建立專案，請確定該組織擁有GitHub帳戶，而且您是成員。
2. **使用[AEM Boilerplate XWalk專案範本](https://github.com/adobe-rnd/aem-boilerplate-xwalk)建立新的程式碼專案**。
3. **安裝AEM程式碼同步GitHub應用程式**&#x200B;並授與存放庫的存取權。 您可以在這裡找到[應用程式](https://github.com/apps/aem-code-sync)。
4. **設定您新專案的`fstab.yaml`**&#x200B;以指向正確的AEM作者服務。

   * 若要進行實驗，您可以使用較低的AEM as a Cloud Service環境（中繼或開發），但真正的網站實作應設定為使用生產AEM服務。

5. **編輯您新專案的`paths.json`**，將AEM Author服務路徑對應至您網站的根目錄。

此Git存放庫已複製至[本機開發環境](https://experienceleague.adobe.com/en/docs/experience-manager-learn/sites/edge-delivery-services/developing/universal-editor/3-local-development-environment)章節，並在此開發程式碼。
