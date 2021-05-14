---
user-guide-title: Adobe Experience Manager as a Cloud Service 教學課程
user-guide-description: Adobe Experience Manager as a Cloud Service 教學課程的系列。
breadcrumb-title: AEM as a Cloud Service 教學課程
sub-product: 雲端服務
team: TM
source-git-commit: 84499d5a7c8adac87196f08c6328e8cb428c0130
workflow-type: tm+mt
source-wordcount: '440'
ht-degree: 21%

---


# Adobe Experience Manager as a Cloud Service 教學課程 {#cloud-service}

+ [概覽](./overview.md)
+ AEM as a Cloud Service 簡介{#introduction}
   + [什麼是AEMCloud Service?](./introduction/what-is-aem-as-a-cloud-service.md)
   + [進化](./introduction/evolution.md)
   + [架構](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
+ 基礎技術{#underlying-technology}
   + [架AEM構](./underlying-technology/introduction-architecture.md)
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
   + [本機執AEM行時期](./local-development-environment/aem-runtime.md)
   + [Local Dispatcher Tools](./local-development-environment/dispatcher-tools.md)
+ 開發{#developing}
   + 開發基本知識{#basics}
      + [AEMSDK](./developing/basics/aem-sdk.md)
      + [本機開發環境](./developing/basics/local-development-environment.md)
      + [AEM 專案原型](./developing/basics/aem-project-archetype.md)
      + [AEM 專案結構](./developing/basics/project-structure.md)
      + [可變內容與不可變內容](./developing/basics/mutable-immutable.md)
      + [儲存庫結構包](./developing/basics/repository-structure-package.md)
      + [內容發佈](./developing/basics/content-publishing.md)
      + [OSGi配置](./developing/basics/osgi-configurations.md)
      + [Dispatcher配置遷移](./developing/basics/dispatcher-configuration.md)
   + [AEMSDK API JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/index.html)
+ 除AEM錯{#debugging}
   + 除AEM錯SDK{#debugging-aem-sdk}
      + [概覽](./debugging/aem-sdk-local-quickstart/overview.md)
      + [記錄檔](./debugging/aem-sdk-local-quickstart/logs.md)
      + [遠端偵錯](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [OSGi Web控制台](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Dispatcher Tools](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [其他工具](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + 作AEM為Cloud Service調試{#debugging-aem-as-a-cloud-service}
      + [概覽](./debugging/cloud-service/overview.md)
      + [記錄檔](./debugging/cloud-service/logs.md)
      + [建立和部署](./debugging/cloud-service/build-and-deployment.md)
      + [開發人員控制台](./debugging/cloud-service/developer-console.md)
      + [CRXDE Lite](./debugging/cloud-service/crxde-lite.md)
+ 訪問AEM{#accessing}
   + [概覽](./accessing/overview.md)
   + [AdobeIMS使用者](./accessing/adobe-ims-users.md)
   + [AdobeIMS使用者群組](./accessing/adobe-ims-user-groups.md)
   + [AdobeIMS產品設定檔](./accessing/adobe-ims-product-profiles.md)
   + [AEM使用者、群組和權限](./accessing/aem-users-groups-and-permissions.md)
   + [設定AEM逐步存取](./accessing/walk-through.md)
+ 遷移{#migration}
   + [內容轉移工具](./migration/content-transfer-tool.md)
   + [大量匯入資產](./migration/bulk-import.md)
+ 表單{#forms}
   + 建立最適化表單{#create-first-af}
      + [簡介](./forms/create-first-af/introduction.md)
      + [建立主題](./forms/create-first-af/create-theme.md)
      + [建立範本](./forms/create-first-af/create-template.md)
      + [建立片段](./forms/create-first-af/create-fragments.md)
      + [建立表格](./forms/create-first-af/create-af.md)
      + [設定根面板](./forms/create-first-af/configure-root-panel.md)
      + [設定人員面板](./forms/create-first-af/configure-people-panel.md)
      + [設定收入面板](./forms/create-first-af/configure-income-panel.md)
      + [設定資產面板](./forms/create-first-af/configure-assets-panel.md)
      + [設定開始面板](./forms/create-first-af/configure-start-panel.md)
      + [新增和設定工具列](./forms/create-first-af/add-configure-toolbar.md)
   + Document CloudAPI和AEM FormsCS{#doc-cloud-sdk}
      + [簡介](./forms/doc-cloud-sdk/introduction.md)
      + [建立AdobeIO項目](./forms/doc-cloud-sdk/create-document-cloud-credentials.md)
      + [建立OSGI設定](./forms/doc-cloud-sdk/create-doc-cloud-configuration.md)
      + [定義介面](./forms/doc-cloud-sdk/create-interface.md)
      + [實作介面](./forms/doc-cloud-sdk/implement-interface.md)
      + [建立JSON部件](./forms/doc-cloud-sdk/get-content-analyzer.md)
      + [自訂流程步驟](./forms/doc-cloud-sdk/custom-process-step.md)
   + 建立審閱工作流{#create-aem-workflow}
      + [建立工作流程模型](./forms/create-aem-workflow/create-workflow.md)
      + [觸發工作流程](./forms/create-aem-workflow/configure-af.md)
   + Adobe Sign與AEM Forms合作{#forms-and-sign}
      + [簡介](./forms/forms-and-sign/introduction.md)
      + [Adobe SignAPI應用程式](./forms/forms-and-sign/create-sign-api-application.md)
      + [Adobe 簽署雲端組態](./forms/forms-and-sign/create-adobe-sign-cloud-configuration.md)
      + [建立最適化表單](./forms/forms-and-sign/create-adaptive-form.md)
      + [設定填寫和簽署](./forms/forms-and-sign/configure-form-fill-and-sign.md)
   + 與Salesforce整合{#integrate-with-salesforce}
      + [簡介](./forms/integrate-with-salesforce/introduction.md)
      + [建立連線的應用程式](./forms/integrate-with-salesforce/create-connected-app.md)
      + [建立Swagger檔案](./forms/integrate-with-salesforce/describe-rest-api.md)
      + [建立資料來源](./forms/integrate-with-salesforce/create-data-source.md)
      + [建立表單資料模型](./forms/integrate-with-salesforce/create-form-data-model.md)
      + [測試表單提交](./forms/integrate-with-salesforce/create-lead-submitting-form.md)
      + [測試點按事件](./forms/integrate-with-salesforce/create-lead-click-event.md)
+ asset compute擴充性{#asset-compute}
   + [概覽](./asset-compute/overview.md)
   + 設定{#set-up}
      + [帳戶和服務布建](./asset-compute/set-up/accounts-and-services.md)
      + [當地開發環境](./asset-compute/set-up/development-environment.md)
      + [AdobeProject Firefly](./asset-compute/set-up/firefly.md)
   + 開發{#develop}
      + [建立Asset compute專案](./asset-compute/develop/project.md)
      + [配置環境變數](./asset-compute/develop/environment-variables.md)
      + [設定manifest.yml](./asset-compute/develop/manifest.md)
      + [開發員工](./asset-compute/develop/worker.md)
      + [使用開發工具](./asset-compute/develop/development-tool.md)
   + 測試與除錯{#test-debug}
      + [測試工作者](./asset-compute/test-debug/test.md)
      + [對工作者進行除錯](./asset-compute/test-debug/debug.md)
   + Deploy{#deploy}
      + [部署至Adobe I/O Runtime](./asset-compute/deploy/runtime.md)
      + [與](./asset-compute/deploy/processing-profiles.md)
   + 進階{#advanced}
      + [中繼資料工作者](./asset-compute/advanced/metadata.md)
   + [疑難排解](./asset-compute/troubleshooting.md)
+ 多步Tutorials{#multi-step-tutorials}
   + [AEM Sites開發](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/develop-wknd-tutorial.html)
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html)
   + [編SPA輯器(React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)
   + [編SPA輯器(Angular)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
   + [AEM Sites和Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html)
   + [代號型驗證](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)
