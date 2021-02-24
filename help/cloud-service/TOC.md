---
user-guide-title: Adobe Experience Manager as a Cloud Service 教學課程
user-guide-description: Adobe Experience Manager as a Cloud Service 教學課程的系列。
breadcrumb-title: AEM as a Cloud Service 教學課程
sub-product: 雲端服務
team: TM
translation-type: tm+mt
source-git-commit: 59b786d95d1428916adad37ceca4412b93463e9b
workflow-type: tm+mt
source-wordcount: '329'
ht-degree: 24%

---


# Adobe Experience Manager as a Cloud Service 教學課程 {#cloud-service}

+ [概覽](./overview.md)
+ AEM as a Cloud Service 簡介{#introduction}
   + [什麼是AEM雲端服務？](./introduction/what-is-aem-as-a-cloud-service.md)
   + [進化](./introduction/evolution.md)
   + [架構](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
+ 基礎技術{#underlying-technology}
   + [AEM架構](./underlying-technology/introduction-architecture.md)
   + [OSGi](./underlying-technology/introduction-osgi.md)
   + [Java內容儲存庫](./underlying-technology/introduction-jcr.md)
   + [Sling](./underlying-technology/introduction-sling.md)
   + [作者與發佈服務](./underlying-technology/introduction-author-publish.md)
   + [Dispatcher](./underlying-technology/introduction-dispatcher.md)
+ Cloud Manager {#cloud-manager}
   + [計劃](./cloud-manager/programs.md)
   + [環境](./cloud-manager/environments.md)
   + [CI/CD生產管道](./cloud-manager/cicd-production-pipeline.md)
   + [CI/CD非生產管道](./cloud-manager/cicd-non-production-pipeline.md)
   + [活動](./cloud-manager/activity.md)
   + 開發操作{#devops}
      + [部署程式碼](./cloud-manager/devops/deploy-code.md)
      + [合併專案](./cloud-manager/devops/merge-projects.md)
      + [配置管線](./cloud-manager/devops/configure-pipelines.md)
      + [持續整合](./cloud-manager/devops/continuous-integration.md)
      + [分析測試結果](./cloud-manager/devops/analyze-test-results.md)
      + [Dispatcher Configurations](./cloud-manager/devops/dispatcher-configurations.md)
      + [Cloud Manager API](./cloud-manager/devops/cloud-manager-apis.md)
+ 本地開發環境設定{#local-development-environment-set-up}
   + [概覽](./local-development-environment/overview.md)
   + [開發工具](./local-development-environment/development-tools.md)
   + [本機AEM Runtime](./local-development-environment/aem-runtime.md)
   + [Local Dispatcher Tools](./local-development-environment/dispatcher-tools.md)
+ 開發{#developing}
   + 開發基本知識{#basics}
      + [AEM SDK](./developing/basics/aem-sdk.md)
      + [當地開發環境](./developing/basics/local-development-environment.md)
      + [AEM 專案原型](./developing/basics/aem-project-archetype.md)
      + [AEM 專案結構](./developing/basics/project-structure.md)
      + [可變內容與不可變內容](./developing/basics/mutable-immutable.md)
      + [儲存庫結構包](./developing/basics/repository-structure-package.md)
      + [內容發佈](./developing/basics/content-publishing.md)
      + [OSGi配置](./developing/basics/osgi-configurations.md)
      + [Dispatcher配置遷移](./developing/basics/dispatcher-configuration.md)
   + [AEM SDK API JavaDocs](https://docs.adobe.com/content/help/en/experience-manager-cloud-service-javadoc/)
+ 除錯AEM{#debugging}
   + 除錯AEM SDK{#debugging-aem-sdk}
      + [概覽](./debugging/aem-sdk-local-quickstart/overview.md)
      + [記錄檔](./debugging/aem-sdk-local-quickstart/logs.md)
      + [遠端偵錯](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [OSGi Web控制台](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Dispatcher Tools](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [其他工具](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + 將AEM除錯為雲端服務{#debugging-aem-as-a-cloud-service}
      + [概覽](./debugging/cloud-service/overview.md)
      + [記錄檔](./debugging/cloud-service/logs.md)
      + [建立和部署](./debugging/cloud-service/build-and-deployment.md)
      + [開發人員控制台](./debugging/cloud-service/developer-console.md)
      + [CRXDE Lite](./debugging/cloud-service/crxde-lite.md)
+ 存取AEM{#accessing}
   + [概覽](./accessing/overview.md)
   + [Adobe IMS使用者](./accessing/adobe-ims-users.md)
   + [Adobe IMS使用者群組](./accessing/adobe-ims-user-groups.md)
   + [Adobe IMS產品設定檔](./accessing/adobe-ims-product-profiles.md)
   + [AEM使用者、群組和權限](./accessing/aem-users-groups-and-permissions.md)
   + [設定AEM逐步存取權](./accessing/walk-through.md)
+ 遷移{#migration}
   + [內容轉移工具](./migration/content-transfer-tool.md)
   + [大量匯入資產](./migration/bulk-import.md)
+ 資產計算擴充性{#asset-compute}
   + [概覽](./asset-compute/overview.md)
   + 設定{#set-up}
      + [帳戶和服務布建](./asset-compute/set-up/accounts-and-services.md)
      + [當地開發環境](./asset-compute/set-up/development-environment.md)
      + [Adobe Project Firefly](./asset-compute/set-up/firefly.md)
   + 開發{#develop}
      + [建立資產計算項目](./asset-compute/develop/project.md)
      + [配置環境變數](./asset-compute/develop/environment-variables.md)
      + [設定manifest.yml](./asset-compute/develop/manifest.md)
      + [開發員工](./asset-compute/develop/worker.md)
      + [使用開發工具](./asset-compute/develop/development-tool.md)
   + 測試與除錯{#test-debug}
      + [測試工作者](./asset-compute/test-debug/test.md)
      + [對工作者進行除錯](./asset-compute/test-debug/debug.md)
   + Deploy{#deploy}
      + [部署至Adobe I/O Runtime](./asset-compute/deploy/runtime.md)
      + [與AEM整合](./asset-compute/deploy/processing-profiles.md)
   + 進階{#advanced}
      + [中繼資料工作者](./asset-compute/advanced/metadata.md)
   + [疑難排解](./asset-compute/troubleshooting.md)
+ 多步驟教學課程{#multi-step-tutorials}
   + [AEM Sites開發](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/develop-wknd-tutorial.html)
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html)
   + [SPA編輯器(React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)
   + [SPA編輯器（角度）](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
   + [AEM Sites和Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html)
   + [代號型驗證](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)

