---
user-guide-title: Adobe Experience Manager as a Cloud Service 教學課程
user-guide-description: Adobe Experience Manager as a Cloud Service 教學課程的系列。
breadcrumb-title: AEM as a Cloud Service 教學課程
sub-product: Experience Manager as a Cloud Service
version: Cloud Service
team: TM
source-git-commit: d5645e975aa290392348cc69d078b24921a7d13a
workflow-type: tm+mt
source-wordcount: '841'
ht-degree: 19%

---


# Adobe Experience Manager as a Cloud Service 教學課程 {#cloud-service}

+ [概觀](./overview.md)
+ AEM as a Cloud Service 簡介{#introduction}
   + [什麼是AEMas a Cloud Service?](./introduction/what-is-aem-as-a-cloud-service.md)
   + [進化](./introduction/evolution.md)
   + [架構](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
   + 戰略與思想領導{#strategy}
      + [Experience Manager — 治理和人員配置模式和原型](./introduction/experience-manager-governance-and-staffing-models.md)
      + [如何使用Adobe Experience Manager提高內容速度](./introduction/drive-content-velocity-for-sites.md)
      + [使用AEM樣式系統加速內容速度](./introduction/accelerate-content-velocity-aem.md)
+ [Experience Cloud 整合](./experience-cloud/integrations.md)
+ 基礎技術 {#underlying-technology}
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
   + 開發作業{#devops}
      + [部署程式碼](./cloud-manager/devops/deploy-code.md)
      + [合併專案](./cloud-manager/devops/merge-projects.md)
      + [配置管道](./cloud-manager/devops/configure-pipelines.md)
      + [持續整合](./cloud-manager/devops/continuous-integration.md)
      + [分析測試結果](./cloud-manager/devops/analyze-test-results.md)
      + [Dispatcher 設定](./cloud-manager/devops/dispatcher-configurations.md)
      + [Cloud Manager API](./cloud-manager/devops/cloud-manager-apis.md)
+ 本機開發環境設定 {#local-development-environment-set-up}
   + [概觀](./local-development-environment/overview.md)
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
      + [清除AEM Maven專案](./developing/projects/remove-samples.md)
   + OSGi服務{#osgi-services}
      + [OSGi服務基本知識](./developing/osgi-services/basics.md)
      + [OSGi元件生命週期](./developing/osgi-services/lifecycle.md)
      + [OSGi配置基本介紹](./developing/osgi-services/configurations.md)
      + [使用OCD的OSGi配置](./developing/osgi-services/configurations-ocd.md)
   + 進階{#advanced}
      + [服務用戶](./developing/advanced/service-users.md)
      + [快取頁面變體](./developing/advanced/variant-caching.md)
   + [AEM SDK API JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/index.html)
+ 除錯AEM{#debugging}
   + 為AEM SDK除錯{#debugging-aem-sdk}
      + [概觀](./debugging/aem-sdk-local-quickstart/overview.md)
      + [記錄檔](./debugging/aem-sdk-local-quickstart/logs.md)
      + [遠端偵錯](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [OSGi Web控制台](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Dispatcher工具](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [其他工具](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + 除錯AEMas a Cloud Service{#debugging-aem-as-a-cloud-service}
      + [概觀](./debugging/cloud-service/overview.md)
      + [記錄檔](./debugging/cloud-service/logs.md)
      + [建置和部署](./debugging/cloud-service/build-and-deployment.md)
      + [開發人員控制台](./debugging/cloud-service/developer-console.md)
      + [存放庫瀏覽器](./debugging/cloud-service/repository-browser.md)
      + 風險{#risks}
         + [若干警告](./debugging/cloud-service/risks/traversals.md)
+ 內容傳送{#content-delivery}
   + [URL重新導向](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/administration/url-redirection.html)
+ 存取AEM{#accessing}
   + [概觀](./accessing/overview.md)
   + [Adobe IMS使用者](./accessing/adobe-ims-users.md)
   + [Adobe IMS使用者群組](./accessing/adobe-ims-user-groups.md)
   + [Adobe IMS產品設定檔](./accessing/adobe-ims-product-profiles.md)
   + [AEM使用者、群組和權限](./accessing/aem-users-groups-and-permissions.md)
   + [設定AEM存取權逐步說明](./accessing/walk-through.md)
+ 驗證{#authentication}
   + [概觀](./authentication/authentication.md)
   + [SAML 2.0](./authentication/saml-2-0.md)
+ 高級網路{#networking}
   + [概觀](./networking/advanced-networking.md)
   + [靈活的埠輸出](./networking/flexible-port-egress.md)
   + [專用的輸出IP地址](./networking/dedicated-egress-ip-address.md)
   + [虛擬專用網](./networking/vpn.md)
   + 程式碼範例{#examples}
      + [在非標準埠上使用HTTP/HTTPS以實現靈活的埠輸出](./networking/examples/http-on-non-standard-ports-flexible-port-egress.md)
      + [專用輸出IP地址/VPN的HTTP/HTTPS](./networking/examples/http-dedicated-egress-ip-vpn.md)
      + [使用DataSourcePool的SQL連接](./networking/examples/sql-datasourcepool.md)
      + [使用Java SQL API的SQL連接](./networking/examples/sql-java-apis.md)
      + [電子郵件服務](./networking/examples/email-service.md)
+ 移轉 {#migration}
   + [內容轉移工具](./migration/content-transfer-tool.md)
   + [大量匯入資產](./migration/bulk-import.md)

   + 轉移至 AEM as a Cloud Service {#moving-to-aem-as-a-cloud-service}
      + [簡介](./migration/moving-to-aem-as-a-cloud-service/introduction.md)
      + [上線](./migration/moving-to-aem-as-a-cloud-service/onboarding.md)
      + [Cloud Manager](./migration/moving-to-aem-as-a-cloud-service/cloud-manager.md)
      + [雙酚A和CAM](./migration/moving-to-aem-as-a-cloud-service/bpa-and-cam.md)
      + [AEM現代化工具](./migration/moving-to-aem-as-a-cloud-service/aem-modernization-tools.md)
      + [存放庫現代化](./migration/moving-to-aem-as-a-cloud-service/repository-modernization.md)
      + [asset compute微服務](./migration/moving-to-aem-as-a-cloud-service/asset-compute-microservices.md)
      + [Dispatcher](./migration/moving-to-aem-as-a-cloud-service/dispatcher.md)
      + [搜尋和索引](./migration/moving-to-aem-as-a-cloud-service/search-and-indexing.md)
      + 內容移轉 {#content-migration}
         + [批量導入服務](./migration/moving-to-aem-as-a-cloud-service/content-migration/bulk-import-service.md)
         + [內容轉移工具](./migration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.md)
         + [常見問題集](./migration/moving-to-aem-as-a-cloud-service/content-migration/faq.md)
      + [疑難排解](./migration/moving-to-aem-as-a-cloud-service/troubleshooting.md)
      + AEM Formsas a Cloud Service {#aem-forms}
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

   + 為Forms開發as a Cloud Service{#developing-for-cloud-service}
      + [快速入門](./forms/developing-for-cloud-service/getting-started.md)
      + [安裝IntelliJ](./forms/developing-for-cloud-service/intellij-set-up.md)
      + [設定Git](./forms/developing-for-cloud-service/setup-git.md)
      + [與AEM同步IntelliJ](./forms/developing-for-cloud-service/intellij-and-aem-sync.md)
      + [建立表單](./forms/developing-for-cloud-service/deploy-your-first-form.md)
      + [啟用Forms Portal元件](./forms/developing-for-cloud-service/forms-portal-components.md)
      + [包含Cloud Services和FDM](./forms/developing-for-cloud-service/azure-storage-fdm.md)
      + [內容感知雲端設定](./forms/developing-for-cloud-service/context-aware-fdm.md)
      + [推送至Cloud Manager](./forms/developing-for-cloud-service/push-project-to-cloud-manager-git.md)
      + [部署至開發環境](./forms/developing-for-cloud-service/deploy-to-dev-environment.md)
      + [更新主原型](./forms/developing-for-cloud-service/updating-project-archetype.md)
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
   + AEM Forms CS中的檔案產生{#doc-gen-formscs}
      + [簡介](./forms/doc-gen-forms-cs/introduction.md)
      + [建立服務憑據](./forms/doc-gen-forms-cs/service-credentials.md)
      + [建立JWT代號](./forms/doc-gen-forms-cs/create-jwt.md)
      + [建立存取權杖](./forms/doc-gen-forms-cs/create-access-token.md)
      + [合併資料與範本](./forms/doc-gen-forms-cs/merge-data-with-template.md)
      + [測試解決方案](./forms/doc-gen-forms-cs/test.md)
      + [挑戰](./forms/doc-gen-forms-cs/challenge.md)
   + 使用批次API產生檔案{#formscs-batch-api}
      + [簡介](./forms/formscs-batch-api/introduction.md)
      + [配置Azure儲存](./forms/formscs-batch-api/configure-azure-storage.md)
      + [建立USC批配置](./forms/formscs-batch-api/configure-usc-batch.md)
      + [建立批次設定](./forms/formscs-batch-api/create-batch-config.md)
      + [執行批](./forms/formscs-batch-api/execute-batch-generate-documents.md)
   + PDF在Forms CS中操作{#forms-cs-assembler}
      + [簡介](./forms/forms-cs-assembler/introduction.md)
      + [建立服務憑據](./forms/forms-cs-assembler/service-credentials.md)
      + [建立JWT代號](./forms/forms-cs-assembler/create-jwt.md)
      + [建立存取權杖](./forms/forms-cs-assembler/create-access-token.md)
      + [組合PDF檔案](./forms/forms-cs-assembler/assemble-pdf-files.md)
      + [PDF/A實用程式](./forms/forms-cs-assembler/pdfa-utilities.md)
      + [測試解決方案](./forms/forms-cs-assembler/test.md)
      + [挑戰](./forms/forms-cs-assembler/challenge.md)
   + Azure門戶儲存{#forms-cs-azure-portal}
      + [簡介](./forms/forms-cs-azure-portal/introduction.md)
      + [建立表單資料模型](./forms/forms-cs-azure-portal/create-fdm.md)
      + [將表單資料儲存在Azure儲存中](./forms/forms-cs-azure-portal/create-af.md)
      + [預填表單](./forms/forms-cs-azure-portal/prefill-af-storage.md)
      + [查詢提交](./forms/forms-cs-azure-portal/query-submitted-data.md)
   + 建立審核工作流{#create-aem-workflow}
      + [將工作流儲存外部化](./forms/create-aem-workflow/externalize-workflow.md)
      + [建立工作流模型](./forms/create-aem-workflow/create-workflow.md)
      + [觸發工作流程](./forms/create-aem-workflow/configure-af.md)
   + Acrobat Sign搭配AEM Forms{#forms-and-sign}
      + [簡介](./forms/forms-and-sign/introduction.md)
      + [Acrobat Sign API應用程式](./forms/forms-and-sign/create-sign-api-application.md)
      + [Acrobat Sign雲端設定](./forms/forms-and-sign/create-adobe-sign-cloud-configuration.md)
      + [建立最適化表單](./forms/forms-and-sign/create-adaptive-form.md)
      + [為填寫和簽名進行配置](./forms/forms-and-sign/configure-form-fill-and-sign.md)
   + 與Microsoft Power Automate整合{#forms-cs-and-power-automate}
      + [設定整合](./forms/forms-cs-and-power-automate/integrate-formscs-power-automate.md)
      + [剖析已提交的表單資料](./forms/forms-cs-and-power-automate/send-email-notification.md)
      + [將DoR作為電子郵件附件發送](./forms/forms-cs-and-power-automate/send-dor-email-attachment.md)
      + [從提交的資料中擷取表單附件](./forms/forms-cs-and-power-automate/send-af-attachments-in-email.md)
   + 與Microsoft Dynamics整合{#formscs-dynamics-crm}
      + [建立Dynamics應用程式](./forms/formscs-dynamics-crm/create-dynamics-account.md)
      + [配置資料源](./forms/formscs-dynamics-crm/configure-odata-data-source.md)
      + [建立表單資料模型](./forms/formscs-dynamics-crm/create-form-data-model.md)
      + [建立最適化表單](./forms/formscs-dynamics-crm/create-adaptive-form.md)
   + 與Salesforce整合{#integrate-with-salesforce}
      + [簡介](./forms/integrate-with-salesforce/introduction.md)
      + [建立連線的應用程式](./forms/integrate-with-salesforce/create-connected-app.md)
      + [建立Swagger檔案](./forms/integrate-with-salesforce/describe-rest-api.md)
      + [建立資料來源](./forms/integrate-with-salesforce/create-data-source.md)
      + [建立表單資料模型](./forms/integrate-with-salesforce/create-form-data-model.md)
      + [測試表單提交](./forms/integrate-with-salesforce/create-lead-submitting-form.md)
      + [測試點按事件](./forms/integrate-with-salesforce/create-lead-click-event.md)
+ asset compute擴充性{#asset-compute}
   + [概觀](./asset-compute/overview.md)
   + 設定{#set-up}
      + [帳戶和服務設定](./asset-compute/set-up/accounts-and-services.md)
      + [本地開發環境](./asset-compute/set-up/development-environment.md)
      + [App Builder](./asset-compute/set-up/app-builder.md)
   + 開發{#develop}
      + [建立Asset compute專案](./asset-compute/develop/project.md)
      + [設定環境變數](./asset-compute/develop/environment-variables.md)
      + [設定manifest.yml](./asset-compute/develop/manifest.md)
      + [開發員工](./asset-compute/develop/worker.md)
      + [使用開發工具](./asset-compute/develop/development-tool.md)
   + 測試和除錯{#test-debug}
      + [測試工作人員](./asset-compute/test-debug/test.md)
      + [調試工作](./asset-compute/test-debug/debug.md)
   + 部署{#deploy}
      + [部署至Adobe I/O Runtime](./asset-compute/deploy/runtime.md)
      + [與AEM整合](./asset-compute/deploy/processing-profiles.md)
   + 進階{#advanced}
      + [中繼資料背景工作](./asset-compute/advanced/metadata.md)
   + [疑難排解](./asset-compute/troubleshooting.md)
+ 雲5{#cloud-5}
   + [簡介](./cloud-5/cloud5-introduction.md)
   + [第1季](./cloud-5/cloud5-season-1.md)
   + [第2季](./cloud-5/cloud5-season-2.md)
   + [AEM CDN第1部分](./cloud-5/cloud5-aem-cdn-part1.md)
   + [AEM CDN第2部分](./cloud-5/cloud5-aem-cdn-part2.md)
   + [AEM記錄檔](./cloud-5/cloud5-aem-log-files.md)
   + [登入Token](./cloud-5/cloud5-getting-login-token-integrations.md)
   + [雲端Dispatcher](./cloud-5/cloud5-aem-dispatcher-cloud.md)
   + [移轉1](./cloud-5/cloud5-aem-content-migration-part-1.md)
   + [移轉2](./cloud-5/cloud5-aem-content-migration-part-2.md)
   + [Dispatcher驗證器](./cloud-5/cloud5-aem-dispatcher-validator.md)
   + [搜尋和索引](./cloud-5/cloud5-aem-search-and-indexing.md)
   + [Adobe應用程式產生器](./cloud-5/cloud5-adobe-app-builder.md)
   + 第2季{#season-2}
      + [片段](./cloud-5/season-2/cloud5-experience-v-content-fragments.md)
      + [Repo Modernizer](./cloud-5/season-2/cloud5-repo-modernizer.md)
      + [Admin Console](./cloud-5/season-2/cloud5-admin-console.md)
      + [重點](./cloud-5/season-2/cloud5-repoinit.md)
      + [Sling作業排程器](./cloud-5/season-2/cloud5-sling-job-scheduler.md)
      + [修正快取](./cloud-5/season-2/cloud5-fix-your-cache.md)
      + [修正您的重新寫入](./cloud-5/season-2/cloud5-fix-your-rewrites.md)
      + [Cloud Manager — 體驗稽核](./cloud-5/season-2/cloud5-mocm-experience-audit.md)
      + [Cloud Manager — 單元測試](./cloud-5/season-2/cloud5-mocm-unit-tests.md)
      + [Cloud Manager — 功能測試](./cloud-5/season-2/cloud5-mocm-functional-tests.md)
+ [AEM Experts系列](./aem-experts-series.md)
+ 多步驟Tutorials{#multi-step-tutorials}
   + [AEM Sites開發](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=zh-Hant)
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html)
   + [SPA編輯器(React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)
   + [AEM Sites和Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html)
   + [基於令牌的驗證](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)
