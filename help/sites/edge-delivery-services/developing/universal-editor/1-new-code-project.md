---
title: 建立程式碼專案
description: 建立Edge Delivery Services的程式碼專案，可使用通用編輯器進行編輯。
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
source-git-commit: e8ce91b0be577ec6cf8f3ab07ba9ff09c7e7a6ab
workflow-type: tm+mt
source-wordcount: '271'
ht-degree: 1%

---


# 建立Edge Delivery Services程式碼專案

若要建置Edge Delivery Services和Universal Editor的AEM網站，請使用Adobe的[AEM Boilerplate XWalk專案範本](https://github.com/adobe-rnd/aem-boilerplate-xwalk)。 此範本會建立新的程式碼專案，專案中包含用來建立網站體驗的CSS和JavaScript。 此範本會建立新的GitHub存放庫，並載入Adobe的程式碼和設定，為您的AEM網站專案提供堅實的基礎。

請記住，由Edge Delivery Services](https://experienceleague.adobe.com/en/docs/experience-manager-learn/sites/edge-delivery-services/overview)傳送的[AEM網站只有使用者端（瀏覽器）代碼。 網站程式碼不會在AEM作者或Publish服務中執行。

![新Edge Delivery Services專案](./assets/1-new-project/new-project.png)

請依照下列步驟，建立可在Universal Editor中編輯其內容的Edge Delivery Services程式碼專案：

1. **設定GitHub帳戶。**&#x200B;如果您要為貴組織建立專案，請確定該組織擁有GitHub帳戶，而且您是成員。
2. **使用[AEM Boilerplate XWalk專案範本](https://github.com/adobe-rnd/aem-boilerplate-xwalk)建立新的程式碼專案**。
3. **安裝AEM Code Sync GitHub應用程式**&#x200B;並授與存放庫的存取權。 您可以在這裡找到[應用程式](https://github.com/apps/aem-code-sync)。
4. **將您新專案的`fstab.yaml`**&#x200B;設定為指向正確的AEM Author服務。

   * 若要實驗，您可以使用較低的AEM as a Cloud Service環境（Stage、Dev或RDE），但真正的網站實作應設定為使用生產AEM Author服務。

5. **編輯您新專案的`paths.json`**，將AEM Author服務路徑對應至您網站的根目錄。

如需詳細指示，請參閱快速入門手冊中的[建立您的GitHub專案區段](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project)。
