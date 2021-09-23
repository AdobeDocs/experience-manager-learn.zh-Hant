---
user-guide-title: Adobe Experience Manager as a Cloud Service 教學課程
user-guide-description: Adobe Experience Manager as a Cloud Service 教學課程的系列。
breadcrumb-title: AEM as a Cloud Service 教學課程
sub-product: cloud-service
team: TM
source-git-commit: 598d00578e5179f76b6f309c5c14dc7b1634f051
workflow-type: tm+mt
source-wordcount: '561'
ht-degree: 22%

---


# Adobe Experience Manager as a Cloud Service 教學課程 {#cloud-service}

+ [概覽](./overview.md)
+ AEM as a Cloud Service 簡介{#introduction}
   + [什麼是AEM作為Cloud Service?](./introduction/what-is-aem-as-a-cloud-service.md)
   + [進化](./introduction/evolution.md)
   + [架構](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
+ 基礎技術{#underlying-technology}
   + [AEM架構](./underlying-technology/introduction-architecture.md)
   + [OSGi](./underlying-technology/introduction-osgi.md)
   + [Java內容儲存庫](./underlying-technology/introduction-jcr.md)
   + [Sling](./underlying-technology/introduction-sling.md)
   + [製作與發佈服務](./underlying-technology/introduction-author-publish.md)
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
      + [配置管道](./cloud-manager/devops/configure-pipelines.md)
      + [持續整合](./cloud-manager/devops/continuous-integration.md)
      + [分析測試結果](./cloud-manager/devops/analyze-test-results.md)
      + [Dispatcher設定](./cloud-manager/devops/dispatcher-configurations.md)
      + [Cloud Manager API](./cloud-manager/devops/cloud-manager-apis.md)
+ 本地開發環境設定{#local-development-environment-set-up}
   + [概覽](./local-development-environment/overview.md)
   + [開發工具](./local-development-environment/development-tools.md)
   + [本機AEM執行階段](./local-development-environment/aem-runtime.md)
   + [本機Dispatcher工具](./local-development-environment/dispatcher-tools.md)
+ 開發{#developing}
   + 開發基本知識{#basics}
      + [AEM SDK](./developing/basics/aem-sdk.md)
      + [本機開發環境](./developing/basics/local-development-environment.md)
      + [AEM 專案原型](./developing/basics/aem-project-archetype.md)
      + [AEM 專案結構](./developing/basics/project-structure.md)
      + [可變內容與不可變內容](./developing/basics/mutable-immutable.md)
      + [儲存庫結構包](./developing/basics/repository-structure-package.md)
      + [內容發佈](./developing/basics/content-publishing.md)
      + [OSGi配置](./developing/basics/osgi-configurations.md)
      + [Dispatcher設定移轉](./developing/basics/dispatcher-configuration.md)
   + AEM 專案{#aem-projects}
      + [AEM Maven專案](./developing/projects/maven-project-structure.md)
   + OSGi服務{#osgi-services}
      + [OSGi服務基本知識](./developing/osgi-services/basics.md)
      + [OSGi元件生命週期](./developing/osgi-services/lifecycle.md)
      + [OSGi配置基本介紹](./developing/osgi-services/configurations.md)
      + [使用OCD的OSGi配置](./developing/osgi-services/configurations-ocd.md)
   + [AEM SDK API JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/index.html)
+ 調試AEM{#debugging}
   + 為AEM SDK{#debugging-aem-sdk}除錯
      + [概覽](./debugging/aem-sdk-local-quickstart/overview.md)
      + [記錄檔](./debugging/aem-sdk-local-quickstart/logs.md)
      + [遠端偵錯](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [OSGi Web控制台](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Dispatcher工具](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [其他工具](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + 將AEM作為Cloud Service進行偵錯{#debugging-aem-as-a-cloud-service}
      + [概覽](./debugging/cloud-service/overview.md)
      + [記錄檔](./debugging/cloud-service/logs.md)
      + [建置和部署](./debugging/cloud-service/build-and-deployment.md)
      + [開發人員控制台](./debugging/cloud-service/developer-console.md)
      + [CRXDE Lite](./debugging/cloud-service/crxde-lite.md)
+ 存取AEM{#accessing}
   + [概覽](./accessing/overview.md)
   + [AdobeIMS使用者](./accessing/adobe-ims-users.md)
   + [AdobeIMS使用者群組](./accessing/adobe-ims-user-groups.md)
   + [AdobeIMS產品設定檔](./accessing/adobe-ims-product-profiles.md)
   + [AEM使用者、群組和權限](./accessing/aem-users-groups-and-permissions.md)
   + [設定AEM存取權逐步說明](./accessing/walk-through.md)
+ 遷移{#migration}
   + [內容轉移工具](./migration/content-transfer-tool.md)
   + [大量匯入資產](./migration/bulk-import.md)

   + 轉移至 AEM as a Cloud Service {#moving-to-aem-as-a-cloud-service}
      + [簡介](./migration/moving-to-aem-as-a-cloud-service/introduction.md)
      + [入門](./migration/moving-to-aem-as-a-cloud-service/onboarding.md)
      + [Cloud Manager](./migration/moving-to-aem-as-a-cloud-service/cloud-manager.md)
      + [雙酚A和CAM](./migration/moving-to-aem-as-a-cloud-service/bpa-and-cam.md)
      + [AEM現代化工具](./migration/moving-to-aem-as-a-cloud-service/aem-modernization-tools.md)
      + [存放庫現代化](./migration/moving-to-aem-as-a-cloud-service/repository-modernization.md)
      + [asset compute微服務](./migration/moving-to-aem-as-a-cloud-service/asset-compute-microservices.md)
      + [Dispatcher](./migration/moving-to-aem-as-a-cloud-service/dispatcher.md)
      + [搜尋和索引](./migration/moving-to-aem-as-a-cloud-service/search-and-indexing.md)
      + 內容移轉{#content-migration}
         + [批量導入服務](./migration/moving-to-aem-as-a-cloud-service/content-migration/bulk-import-service.md)
         + [內容轉移工具](./migration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.md)
      + [疑難排解](./migration/moving-to-aem-as-a-cloud-service/troubleshooting.md)
      + AEM Forms as aCloud Service{#aem-forms}
         + [簡介](./migration/moving-to-aem-as-a-cloud-service/aem-forms/introduction.md)
         + [數位註冊](./migration/moving-to-aem-as-a-cloud-service/aem-forms/digital-enrollment.md)
         + [通訊](./migration/moving-to-aem-as-a-cloud-service/aem-forms/communications.md)
   + Cloud Acceleration Manager {#cloud-acceleration-manager}
      + [簡介](./migration/cloud-acceleration-manager/introduction.md)
      + [Readiness and Best Practice Analyzer](./migration/cloud-acceleration-manager/readiness-and-best-practice-analyzer.md)
      + [實施階段](./migration/cloud-acceleration-manager/implementation-phase.md)
      + [內容轉移工具](./migration/cloud-acceleration-manager/content-transfer-tool.md)
      + [程式碼重構工具](./migration/cloud-acceleration-manager/code-refactoring-tools.md)
      + [Code Repository Modernizer](./migration/cloud-acceleration-manager/code-repository-modernizer.md)
      + [Dispatcher 轉換工具](./migration/cloud-acceleration-manager/dispatcher-converter.md)
      + [索引轉換器](./migration/cloud-acceleration-manager/index-converter.md)
      + [資產工作流程移轉工具](./migration/cloud-acceleration-manager/asset-workflow-migration-tool.md)
      + [導覽Cloud Acceleration Manager](./migration/cloud-acceleration-manager/navigating.md)
      + [使用Cloud Acceleration Manager](./migration/cloud-acceleration-manager/using.md)
+ 表單{#forms}
   + 建立最適化表單{#create-first-af}
      + [簡介](./forms/create-first-af/introduction.md)
      + [建立主題](./forms/create-first-af/create-theme.md)
      + [建立範本](./forms/create-first-af/create-template.md)
      + [建立片段](./forms/create-first-af/create-fragments.md)
      + [建立表單](./forms/create-first-af/create-af.md)
      + [配置根面板](./forms/create-first-af/configure-root-panel.md)
      + [配置人員面板](./forms/create-first-af/configure-people-panel.md)
      + [配置收入面板](./forms/create-first-af/configure-income-panel.md)
      + [設定資產面板](./forms/create-first-af/configure-assets-panel.md)
      + [配置開始面板](./forms/create-first-af/configure-start-panel.md)
      + [添加和配置工具欄](./forms/create-first-af/add-configure-toolbar.md)
   + Document CloudAPI和AEM Forms CS{#doc-cloud-sdk}
      + [簡介](./forms/doc-cloud-sdk/introduction.md)
      + [建立Adobe I/O專案](./forms/doc-cloud-sdk/create-document-cloud-credentials.md)
      + [建立OSGi配置](./forms/doc-cloud-sdk/create-doc-cloud-configuration.md)
      + [定義介面](./forms/doc-cloud-sdk/create-interface.md)
      + [實作介面](./forms/doc-cloud-sdk/implement-interface.md)
      + [建立JSON部件](./forms/doc-cloud-sdk/get-content-analyzer.md)
      + [自訂程式步驟](./forms/doc-cloud-sdk/custom-process-step.md)
   + Azure門戶儲存{#forms-cs-azure-portal}
      + [簡介](./forms/forms-cs-azure-portal/introduction.md)
      + [建立表單資料模型](./forms/forms-cs-azure-portal/create-fdm.md)
      + [將表單資料儲存在Azure儲存中](./forms/forms-cs-azure-portal/create-af.md)
      + [預填表單](./forms/forms-cs-azure-portal/prefill-af-storage.md)
      + [查詢提交](./forms/forms-cs-azure-portal/query-submitted-data.md)


      + 建立審核工作流{#create-aem-workflow}
         + [建立工作流模型](./forms/create-aem-workflow/create-workflow.md)
         + [觸發工作流程](./forms/create-aem-workflow/configure-af.md)
      + Adobe Sign搭配AEM Forms{#forms-and-sign}
         + [簡介](./forms/forms-and-sign/introduction.md)
         + [Adobe Sign API應用程式](./forms/forms-and-sign/create-sign-api-application.md)
         + [Adobe 簽署雲端組態](./forms/forms-and-sign/create-adobe-sign-cloud-configuration.md)
         + [建立最適化表單](./forms/forms-and-sign/create-adaptive-form.md)
         + [為填寫和簽名進行配置](./forms/forms-and-sign/configure-form-fill-and-sign.md)
      + 與Salesforce整合{#integrate-with-salesforce}
         + [簡介](./forms/integrate-with-salesforce/introduction.md)
         + [建立連線的應用程式](./forms/integrate-with-salesforce/create-connected-app.md)
         + [建立Swagger檔案](./forms/integrate-with-salesforce/describe-rest-api.md)
         + [建立資料來源](./forms/integrate-with-salesforce/create-data-source.md)
         + [建立表單資料模型](./forms/integrate-with-salesforce/create-form-data-model.md)
         + [測試表單提交](./forms/integrate-with-salesforce/create-lead-submitting-form.md)
         + [測試點按事件](./forms/integrate-with-salesforce/create-lead-click-event.md)
+ asset compute擴展性{#asset-compute}
   + [概覽](./asset-compute/overview.md)
   + 設定{#set-up}
      + [帳戶和服務設定](./asset-compute/set-up/accounts-and-services.md)
      + [本地開發環境](./asset-compute/set-up/development-environment.md)
      + [AdobeProject Firefly](./asset-compute/set-up/firefly.md)
   + 開發{#develop}
      + [建立Asset compute專案](./asset-compute/develop/project.md)
      + [設定環境變數](./asset-compute/develop/environment-variables.md)
      + [設定manifest.yml](./asset-compute/develop/manifest.md)
      + [開發員工](./asset-compute/develop/worker.md)
      + [使用開發工具](./asset-compute/develop/development-tool.md)
   + 測試和調試{#test-debug}
      + [測試工作人員](./asset-compute/test-debug/test.md)
      + [調試工作](./asset-compute/test-debug/debug.md)
   + 部署{#deploy}
      + [部署至Adobe I/O Runtime](./asset-compute/deploy/runtime.md)
      + [與AEM整合](./asset-compute/deploy/processing-profiles.md)
   + 進階{#advanced}
      + [中繼資料背景工作](./asset-compute/advanced/metadata.md)
   + [疑難排解](./asset-compute/troubleshooting.md)
+ 多步驟Tutorials{#multi-step-tutorials}
   + [AEM Sites開發](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html)
   + [SPA編輯器(React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)
   + [SPA編輯器(Angular)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
   + [AEM Sites和Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html)
   + [基於令牌的驗證](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)
