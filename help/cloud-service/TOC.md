---
user-guide-title: Adobe Experience Manager as a Cloud Service 教學課程
user-guide-description: Adobe Experience Manager as a Cloud Service 教學課程的系列。
breadcrumb-title: AEM as a Cloud Service 教學課程
solution: Experience Manager, Experience Manager as a Cloud Service
sub-product: Experience Manager as a Cloud Service
version: Cloud Service
team: TM
source-git-commit: d9612adbc2ff3e601c2efe5a779c03ad24769276
workflow-type: tm+mt
source-wordcount: '1084'
ht-degree: 16%

---


# Adobe Experience Manager as a Cloud Service 教學課程 {#cloud-service}

+ [概觀](./overview.md)
+ AEM試用版 {#aem-trials}
   + [影像](./aem-trials/images.md)
+ AEMas a Cloud Service簡介{#introduction}
   + [AEM有哪些as a Cloud Service？](./introduction/what-is-aem-as-a-cloud-service.md)
   + [架構](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
   + 策略與思維領導力{#strategy}
      + [Experience Manager — 治理與人員配置模型與原型](./introduction/experience-manager-governance-and-staffing-models.md)
      + [如何使用Adobe Experience Manager提升內容速度](./introduction/drive-content-velocity-for-sites.md)
      + [使用AEM樣式系統加快內容速度](./introduction/accelerate-content-velocity-aem.md)
+ Experience Cloud整合{#integrations}
   + [整合](./integrations/experience-cloud.md)
   + [Adobe Target](./integrations/target.md)
+ 基礎技術 {#underlying-technology}
   + [AEM架構](./underlying-technology/introduction-architecture.md)
   + [osgi](./underlying-technology/introduction-osgi.md)
   + [Java內容存放庫](./underlying-technology/introduction-jcr.md)
   + [Sling](./underlying-technology/introduction-sling.md)
   + [作者與發佈服務](./underlying-technology/introduction-author-publish.md)
   + [Dispatcher](./underlying-technology/introduction-dispatcher.md)
+ Edge Delivery Services {#edge-delivery-services}
   + [AEM AssetsSidekick外掛程式](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/edge-delivery-services/sidekick-plugin.html){target=_blank}
+ Cloud Manager {#cloud-manager}
   + [方案](./cloud-manager/programs.md)
   + [環境](./cloud-manager/environments.md)
   + [CI/CD 生產管道](./cloud-manager/cicd-production-pipeline.md)
   + [CI/CD 非生產管道](./cloud-manager/cicd-non-production-pipeline.md)
   + [活動](./cloud-manager/activity.md)
   + Dev Ops{#devops}
      + [部署程式碼](./cloud-manager/devops/deploy-code.md)
      + [合併專案](./cloud-manager/devops/merge-projects.md)
      + [設定管道](./cloud-manager/devops/configure-pipelines.md)
      + [持續整合](./cloud-manager/devops/continuous-integration.md)
      + [分析測試結果](./cloud-manager/devops/analyze-test-results.md)
      + [Dispatcher 設定](./cloud-manager/devops/dispatcher-configurations.md)
      + [Cloud Manager API](./cloud-manager/devops/cloud-manager-apis.md)
+ 本機開發環境設定 {#local-development-environment-set-up}
   + [概觀](./local-development-environment/overview.md)
   + [開發工具](./local-development-environment/development-tools.md)
   + [本機AEM SDK](./local-development-environment/aem-runtime.md)
   + [本機 Dispatcher 工具](./local-development-environment/dispatcher-tools.md)
+ 開發{#developing}
   + 擴充性{#extensibility}
      + App Builder{#app-builder}
         + [產生存取權杖](./developing/extensibility/app-builder/jwt-auth.md)
      + UI擴充性{#ui}
         + [概觀](./developing/extensibility/ui/overview.md)
         + [Adobe Developer Console專案](./developing/extensibility/ui/adobe-developer-console-project.md)
         + [初始化應用程式](./developing/extensibility/ui/app-initialization.md)
         + [註冊副檔名](./developing/extensibility/ui/extension-registration.md)
         + [模型](./developing/extensibility/ui/modal.md)
         + [Adobe I/O Runtime動作](./developing/extensibility/ui/runtime-action.md)
         + [驗證](./developing/extensibility/ui/verify.md)
         + [部署](./developing/extensibility/ui/deploy.md)
         + 內容片段{#content-fragments}
            + [概觀](./developing/extensibility/ui/content-fragments/overview.md)
            + 範例{#examples}
               + [AI影像產生](./developing/extensibility/ui/content-fragments/examples/console-image-generation-and-image-upload.md)
               + [大量屬性更新](./developing/extensibility/ui/content-fragments/examples/console-bulk-property-update.md)
               + [自訂格線欄](./developing/extensibility/ui/content-fragments/examples/custom-grid-columns.md)
               + [匯出為XML](./developing/extensibility/ui/content-fragments/examples/editor-export-to-xml.md)
               + [RTE工具列按鈕](./developing/extensibility/ui/content-fragments/examples/editor-rte-toolbar.md)
               + [RTE Widget](./developing/extensibility/ui/content-fragments/examples/editor-rte-widget.md)
               + [RTE徽章](./developing/extensibility/ui/content-fragments/examples/editor-rte-badges.md)
   + 開發基本知識{#basics}
      + [AEM SDK](./developing/basics/aem-sdk.md)
      + [本機開發環境](./developing/basics/local-development-environment.md)
      + [AEM 專案原型](./developing/basics/aem-project-archetype.md)
      + [AEM 專案結構](./developing/basics/project-structure.md)
      + [可變與不可變內容](./developing/basics/mutable-immutable.md)
      + [存放庫結構套件](./developing/basics/repository-structure-package.md)
      + [內容發佈](./developing/basics/content-publishing.md)
      + [OSGi 設定](./developing/basics/osgi-configurations.md)
      + [Dispatcher設定移轉](./developing/basics/dispatcher-configuration.md)
   + AEM專案{#aem-projects}
      + [AEM Maven專案](./developing/projects/maven-project-structure.md)
      + [清理AEM Maven專案](./developing/projects/remove-samples.md)
   + OSGi服務{#osgi-services}
      + [OSGi服務基本知識](./developing/osgi-services/basics.md)
      + [OSGi元件生命週期](./developing/osgi-services/lifecycle.md)
      + [OSGi設定基本需知](./developing/osgi-services/configurations.md)
      + [使用OCD的OSGi設定](./developing/osgi-services/configurations-ocd.md)
   + 進階{#advanced}
      + [快取頁面變體](./developing/advanced/variant-caching.md)
      + [CSRF保護](./developing/advanced/csrf-protection.md)
      + [自訂名稱空間](./developing/advanced/custom-namespaces.md)
      + [服務使用者](./developing/advanced/service-users.md)
      + [網頁最佳化的影像API](./developing/advanced/web-optimized-image-delivery-java-apis.md)
   + 快速開發環境{#rde}
      + [概觀](./developing/rde/overview.md)
      + [設定方法](./developing/rde/how-to-setup.md)
      + [使用方式](./developing/rde/how-to-use.md)
      + [開發生命週期](./developing/rde/development-life-cycle.md)
   + [AEM SDK API JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/index.html){target=_blank}
+ 偵錯AEM{#debugging}
   + 為AEM SDK除錯{#debugging-aem-sdk}
      + [概觀](./debugging/aem-sdk-local-quickstart/overview.md)
      + [記錄檔](./debugging/aem-sdk-local-quickstart/logs.md)
      + [遠端偵錯](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [OSGi Web主控台](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Dispatcher 工具](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [其他工具](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + 偵錯AEMas a Cloud Service{#debugging-aem-as-a-cloud-service}
      + [概觀](./debugging/cloud-service/overview.md)
      + [記錄檔](./debugging/cloud-service/logs.md)
      + [建置和部署](./debugging/cloud-service/build-and-deployment.md)
      + [開發人員主控台](./debugging/cloud-service/developer-console.md)
      + [存放庫瀏覽器](./debugging/cloud-service/repository-browser.md)
      + 風險{#risks}
         + [周遊警告](./debugging/cloud-service/risks/traversals.md)
+ 內容傳送{#content-delivery}
   + [URL重新導向](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/administration/url-redirection.html){target=_blank}
+ 快取{#caching}
   + [概觀](./caching/overview.md)
   + [AEM發佈服務](./caching/publish.md)
   + [AEM作者服務](./caching/author.md)
   + [CDN快取命中率分析](./caching/cdn-cache-hit-ratio-analysis.md)
   + 操作說明{#how-to}
      + [啟用快取](./caching/how-to/enable-caching.md)
      + [停用快取](./caching/how-to/disable-caching.md)
+ 存取AEM{#accessing}
   + [概觀](./accessing/overview.md)
   + [Adobe IMS 使用者](./accessing/adobe-ims-users.md)
   + [Adobe IMS 使用者群組](./accessing/adobe-ims-user-groups.md)
   + [Adobe IMS 產品設定檔](./accessing/adobe-ims-product-profiles.md)
   + [AEM使用者、群組和許可權](./accessing/aem-users-groups-and-permissions.md)
   + [AEM 存取權設定逐步說明](./accessing/walk-through.md)
+ 驗證{#authentication}
   + [概觀](./authentication/authentication.md)
   + [SAML 2.0](./authentication/saml-2-0.md)
+ 進階網路{#networking}
   + [概觀](./networking/advanced-networking.md)
   + [彈性的連線埠輸出](./networking/flexible-port-egress.md)
   + [專用輸出IP位址](./networking/dedicated-egress-ip-address.md)
   + [虛擬私人網路](./networking/vpn.md)
   + 程式碼範例{#examples}
      + [非標準連線埠上的HTTP/HTTPS，用於彈性連線埠輸出](./networking/examples/http-on-non-standard-ports-flexible-port-egress.md)
      + [專用輸出IP位址/VPN的HTTP/HTTPS](./networking/examples/http-dedicated-egress-ip-vpn.md)
      + [使用DataSourcePool的SQL連線](./networking/examples/sql-datasourcepool.md)
      + [使用Java SQL API的SQL連線](./networking/examples/sql-java-apis.md)
      + [電子郵件服務](./networking/examples/email-service.md)
+ 安全性 {#security}
   + 包含WAF規則的流量篩選規則{#traffic-filter-and-waf-rules}
      + [概觀](./security/traffic-filter-rules/overview.md)
      + [設定方法](./security/traffic-filter-rules/how-to-setup.md)
      + [範例和結果分析](./security/traffic-filter-rules/examples-and-analysis.md)
      + [最佳做法](./security/traffic-filter-rules/best-practices.md)
+ 移轉 {#migration}
   + [內容轉移工具](./migration/content-transfer-tool.md)
   + [大量匯入資產](./migration/bulk-import.md)
   + 轉移至 AEM as a Cloud Service {#moving-to-aem-as-a-cloud-service}
      + [簡介](./migration/moving-to-aem-as-a-cloud-service/introduction.md)
      + [上線](./migration/moving-to-aem-as-a-cloud-service/onboarding.md)
      + [Cloud Manager](./migration/moving-to-aem-as-a-cloud-service/cloud-manager.md)
      + [BPA與CAM](./migration/moving-to-aem-as-a-cloud-service/bpa-and-cam.md)
      + [AEM現代化工具](./migration/moving-to-aem-as-a-cloud-service/aem-modernization-tools.md)
      + [存放庫現代化](./migration/moving-to-aem-as-a-cloud-service/repository-modernization.md)
      + [asset compute微服務](./migration/moving-to-aem-as-a-cloud-service/asset-compute-microservices.md)
      + [Dispatcher](./migration/moving-to-aem-as-a-cloud-service/dispatcher.md)
      + [搜尋和建立索引](./migration/moving-to-aem-as-a-cloud-service/search-and-indexing.md)
      + 內容移轉 {#content-migration}
         + [大量匯入服務](./migration/moving-to-aem-as-a-cloud-service/content-migration/bulk-import-service.md)
         + [內容轉移工具](./migration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.md)
         + [常見問題集](./migration/moving-to-aem-as-a-cloud-service/content-migration/faq.md)
      + [疑難排解](./migration/moving-to-aem-as-a-cloud-service/troubleshooting.md)
      + AEM Formsas a Cloud Service {#aem-forms}
         + [簡介](./migration/moving-to-aem-as-a-cloud-service/aem-forms/introduction.md)
         + [數位註冊](./migration/moving-to-aem-as-a-cloud-service/aem-forms/digital-enrollment.md)
         + [通訊](./migration/moving-to-aem-as-a-cloud-service/aem-forms/communications.md)
   + Cloud Acceleration Manager {#cloud-acceleration-manager}
      + [簡介](./migration/cloud-acceleration-manager/introduction.md)
      + [整備與Best Practice Analyzer](./migration/cloud-acceleration-manager/readiness-and-best-practice-analyzer.md)
      + [實作階段](./migration/cloud-acceleration-manager/implementation-phase.md)
      + [內容轉移工具](./migration/cloud-acceleration-manager/content-transfer-tool.md)
      + [程式碼重構工具](./migration/cloud-acceleration-manager/code-refactoring-tools.md)
      + [程式碼存放庫現代化工具](./migration/cloud-acceleration-manager/code-repository-modernizer.md)
      + [Dispatcher轉換工具](./migration/cloud-acceleration-manager/dispatcher-converter.md)
      + [索引轉換器](./migration/cloud-acceleration-manager/index-converter.md)
      + [資產工作流程移轉工具](./migration/cloud-acceleration-manager/asset-workflow-migration-tool.md)
      + [瀏覽Cloud Acceleration Manager](./migration/cloud-acceleration-manager/navigating.md)
      + [使用Cloud Acceleration Manager](./migration/cloud-acceleration-manager/using.md)
+ [內容片段](https://experienceleague.adobe.com/docs/experience-manager-learn/content-fragments-console/overview.html){target=_blank}
+ Forms{#forms}
   + 針對Formsas a Cloud Service開發{#developing-for-cloud-service}
      + [1 — 快速入門](./forms/developing-for-cloud-service/getting-started.md)
      + [2 — 安裝IntelliJ](./forms/developing-for-cloud-service/intellij-set-up.md)
      + [3 — 設定Git](./forms/developing-for-cloud-service/setup-git.md)
      + [4 — 同步IntelliJ與AEM](./forms/developing-for-cloud-service/intellij-and-aem-sync.md)
      + [5 — 建立表單](./forms/developing-for-cloud-service/deploy-your-first-form.md)
      + [6 — 自訂提交處理常式](./forms/developing-for-cloud-service/custom-submit-to-servlet.md)
      + [7 — 使用資源型別註冊servlet](./forms/developing-for-cloud-service/registering-servlet-using-resourcetype.md)
      + [8 — 啟用Forms Portal元件](./forms/developing-for-cloud-service/forms-portal-components.md)
      + [9 — 包含Cloud Service和FDM](./forms/developing-for-cloud-service/azure-storage-fdm.md)
      + [10 — 內容感知雲端設定](./forms/developing-for-cloud-service/context-aware-fdm.md)
      + [11 — 推送到Cloud Manager](./forms/developing-for-cloud-service/push-project-to-cloud-manager-git.md)
      + [12 — 部署至開發環境](./forms/developing-for-cloud-service/deploy-to-dev-environment.md)
      + [13 — 更新maven原型](./forms/developing-for-cloud-service/updating-project-archetype.md)
   + 建立最適化表單{#create-first-af}
      + [簡介](./forms/create-first-af/introduction.md)
      + [建立主題](./forms/create-first-af/create-theme.md)
      + [建立範本](./forms/create-first-af/create-template.md)
      + [建立片段](./forms/create-first-af/create-fragments.md)
      + [建立表單](./forms/create-first-af/create-af.md)
      + [設定根面板](./forms/create-first-af/configure-root-panel.md)
      + [設定人員面板](./forms/create-first-af/configure-people-panel.md)
      + [設定收入面板](./forms/create-first-af/configure-income-panel.md)
      + [設定資產面板](./forms/create-first-af/configure-assets-panel.md)
      + [設定開始面板](./forms/create-first-af/configure-start-panel.md)
      + [新增並設定工具列](./forms/create-first-af/add-configure-toolbar.md)
   + 使用Headless表單的自訂提交服務{#custom-submit-headless-forms}
      + [1 — 簡介](./forms/custom-submit-headless-forms/introduction.md)
      + [2 — 建立自訂提交服務](./forms/custom-submit-headless-forms/custom-submit-service.md)
      + [3 — 顯示回應](./forms/custom-submit-headless-forms/handle-response-react-app.md)
   + AEM Forms與Analytics{#forms-and-analytics}
      + [簡介](./forms/form-data-analytics/introduction.md)
      + [建立資料元素](./forms/form-data-analytics/data-elements.md)
      + [建立規則](./forms/form-data-analytics/rules.md)
      + [測試解決方案](./forms/form-data-analytics/test.md)
   + AEM Forms CS中的檔案產生{#doc-gen-formscs}
      + [簡介](./forms/doc-gen-forms-cs/introduction.md)
      + [建立服務認證](./forms/doc-gen-forms-cs/service-credentials.md)
      + [建立JWT權杖](./forms/doc-gen-forms-cs/create-jwt.md)
      + [建立存取Token](./forms/doc-gen-forms-cs/create-access-token.md)
      + [將資料與範本合併](./forms/doc-gen-forms-cs/merge-data-with-template.md)
      + [測試解決方案](./forms/doc-gen-forms-cs/test.md)
      + [挑戰](./forms/doc-gen-forms-cs/challenge.md)
   + 使用批次API產生檔案{#formscs-batch-api}
      + [簡介](./forms/formscs-batch-api/introduction.md)
      + [設定Azure儲存體](./forms/formscs-batch-api/configure-azure-storage.md)
      + [建立USC批次設定](./forms/formscs-batch-api/configure-usc-batch.md)
      + [建立批次設定](./forms/formscs-batch-api/create-batch-config.md)
      + [執行批次](./forms/formscs-batch-api/execute-batch-generate-documents.md)
   + Forms CS中的PDF操作{#forms-cs-assembler}
      + [簡介](./forms/forms-cs-assembler/introduction.md)
      + [建立服務認證](./forms/forms-cs-assembler/service-credentials.md)
      + [建立JWT權杖](./forms/forms-cs-assembler/create-jwt.md)
      + [建立存取Token](./forms/forms-cs-assembler/create-access-token.md)
      + [組合PDF檔案](./forms/forms-cs-assembler/assemble-pdf-files.md)
      + [PDF/A公用程式](./forms/forms-cs-assembler/pdfa-utilities.md)
      + [測試解決方案](./forms/forms-cs-assembler/test.md)
      + [挑戰](./forms/forms-cs-assembler/challenge.md)
   + 使用Blob索引標籤儲存表單提交{#store-submiited-data-with-metadata-tags}
      + [簡介](./forms/store-submiited-data-with-metadata-tags/introduction.md)
      + [擴充選擇群組元件](./forms/store-submiited-data-with-metadata-tags/extend-choice-group-components.md)
      + [建立OSGi設定](./forms/store-submiited-data-with-metadata-tags/create-osgi-configuration.md)
      + [建立索引標籤](./forms/store-submiited-data-with-metadata-tags/create-blob-index-tags.md)
      + [建立自訂提交](./forms/store-submiited-data-with-metadata-tags/create-custom-submit.md)
   + 預填核心元件型表單{#prefill-core-component-based-form}
      + [簡介](./forms/prefill-core-component-form/introduction.md)
      + [寫入預填服務](./forms/prefill-core-component-form/pre-fill-service.md)
      + [測試解決方案](./forms/prefill-core-component-form/test-solution.md)
   + Azure入口網站儲存體{#forms-cs-azure-portal}
      + [簡介](./forms/forms-cs-azure-portal/introduction.md)
      + [建立表單資料模型](./forms/forms-cs-azure-portal/create-fdm.md)
      + [將表單資料儲存在Azure儲存體](./forms/forms-cs-azure-portal/create-af.md)
      + [預填表單](./forms/forms-cs-azure-portal/prefill-af-storage.md)
      + [查詢提交](./forms/forms-cs-azure-portal/query-submitted-data.md)
   + 儲存並繼續表單填寫{#prefill-azure-storage}
      + [1 — 簡介](./forms/prefill-azure-storage/introduction.md)
      + [2 — 建立頁面元件](./forms/prefill-azure-storage/page-component.md)
      + [3 — 建立自適應表單範本](./forms/prefill-azure-storage/associate-page-component.md)
      + [4 — 建立Azure儲存整合](./forms/prefill-azure-storage/create-fdm.md)
      + [5 — 建立SendGrid整合](./forms/prefill-azure-storage/send-grid-fdm.md)
      + [6 — 建立最適化表單](./forms/prefill-azure-storage/create-af.md)
      + [7 — 部署範例資產](./forms/prefill-azure-storage/deploy-sample-assets.md)

   + 建立稽核工作流程{#create-aem-workflow}
      + [將工作流程儲存區外部化](./forms/create-aem-workflow/externalize-workflow.md)
      + [建立工作流程模型](./forms/create-aem-workflow/create-workflow.md)
      + [觸發工作流程](./forms/create-aem-workflow/configure-af.md)
   + Acrobat Sign與AEM Forms{#forms-and-sign}
      + [簡介](./forms/forms-and-sign/introduction.md)
      + [Acrobat Sign API應用計畫](./forms/forms-and-sign/create-sign-api-application.md)
      + [Acrobat Sign雲端設定](./forms/forms-and-sign/create-adobe-sign-cloud-configuration.md)
      + [建立最適化表單](./forms/forms-and-sign/create-adaptive-form.md)
      + [設定以填寫並簽署](./forms/forms-and-sign/configure-form-fill-and-sign.md)
   + 整合Microsoft Power Automate{#forms-cs-and-power-automate}
      + [設定整合](./forms/forms-cs-and-power-automate/integrate-formscs-power-automate.md)
      + [剖析提交的表單資料](./forms/forms-cs-and-power-automate/send-email-notification.md)
      + [傳送DoR作為電子郵件附件](./forms/forms-cs-and-power-automate/send-dor-email-attachment.md)
      + [從提交的資料中擷取表單附件](./forms/forms-cs-and-power-automate/send-af-attachments-in-email.md)
   + 與Microsoft Dynamics整合{#formscs-dynamics-crm}
      + [建立Dynamics應用程式](./forms/formscs-dynamics-crm/create-dynamics-account.md)
      + [設定資料來源](./forms/formscs-dynamics-crm/configure-odata-data-source.md)
      + [建立表單資料模型](./forms/formscs-dynamics-crm/create-form-data-model.md)
      + [建立最適化表單](./forms/formscs-dynamics-crm/create-adaptive-form.md)
   + 與Salesforce整合{#integrate-with-salesforce}
      + [簡介](./forms/integrate-with-salesforce/introduction.md)
      + [建立連線應用程式](./forms/integrate-with-salesforce/create-connected-app.md)
      + [建立Swagger檔案](./forms/integrate-with-salesforce/describe-rest-api.md)
      + [建立資料來源](./forms/integrate-with-salesforce/create-data-source.md)
      + [建立表單資料模型](./forms/integrate-with-salesforce/create-form-data-model.md)
      + [測試表單提交](./forms/integrate-with-salesforce/create-lead-submitting-form.md)
      + [測試點按事件](./forms/integrate-with-salesforce/create-lead-click-event.md)
   + 將表單提交內容儲存在單一磁碟機和SharePoint中{#one-drive}
      + [將表單資料儲存在單一磁碟機中](./forms/forms-cs-one-drive/store-form-submission-one-drive.md)
      + [將表單資料儲存在SharePoint中](./forms/forms-cs-sharepoint/store-form-submission-in-sharepoint.md)
+ asset compute擴充性{#asset-compute}
   + [概觀](./asset-compute/overview.md)
   + 設定{#set-up}
      + [帳戶和服務布建](./asset-compute/set-up/accounts-and-services.md)
      + [本機開發環境](./asset-compute/set-up/development-environment.md)
      + [App Builder](./asset-compute/set-up/app-builder.md)
   + 開發{#develop}
      + [建立Asset compute專案](./asset-compute/develop/project.md)
      + [設定環境變數](./asset-compute/develop/environment-variables.md)
      + [設定manifest.yml](./asset-compute/develop/manifest.md)
      + [開發背景工作](./asset-compute/develop/worker.md)
      + [使用開發工具](./asset-compute/develop/development-tool.md)
   + 測試和除錯{#test-debug}
      + [測試背景工作](./asset-compute/test-debug/test.md)
      + [對背景工作進行偵錯](./asset-compute/test-debug/debug.md)
   + 部署{#deploy}
      + [部署至Adobe I/O Runtime](./asset-compute/deploy/runtime.md)
      + [與AEM整合](./asset-compute/deploy/processing-profiles.md)
   + 進階{#advanced}
      + [中繼資料背景工作](./asset-compute/advanced/metadata.md)
   + [疑難排解](./asset-compute/troubleshooting.md)

+ 多步驟Tutorials{#multi-step-tutorials}
   + [AEM Sites開發](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html){target=_blank}
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html){target=_blank}
   + [SPA編輯器(React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html){target=_blank}
   + [AEM Sites和Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html){target=_blank}
   + [權杖型驗證](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html){target=_blank}
+ 專家資源 {#expert-resources}
   + AEM Champions {#aem-champions}
      + [Cloud Manager入門行動手冊](./expert-resources/aem-champions/onboarding-playbook.md)
      + [Cloud Manager環境型別](./expert-resources/aem-champions/environment-types.md)
      + [Cloud Manager UI](./expert-resources/aem-champions/cloud-manager-ui.md)
   + [AEM Experts系列](./expert-resources/expert-series/aem-experts-series.md)
   + Cloud 5{#cloud-5}
      + [簡介](./expert-resources/cloud-5/cloud5-introduction.md)
      + [第1季](./expert-resources/cloud-5/cloud5-season-1.md)
      + [第2季](./expert-resources/cloud-5/cloud5-season-2.md)
      + [AEM CDN第1部分](./expert-resources/cloud-5/cloud5-aem-cdn-part1.md)
      + [AEM CDN第2部分](./expert-resources/cloud-5/cloud5-aem-cdn-part2.md)
      + [AEM記錄檔](./expert-resources/cloud-5/cloud5-aem-log-files.md)
      + [登入權杖](./expert-resources/cloud-5/cloud5-getting-login-token-integrations.md)
      + [雲端Dispatcher](./expert-resources/cloud-5/cloud5-aem-dispatcher-cloud.md)
      + [移轉1](./expert-resources/cloud-5/cloud5-aem-content-migration-part-1.md)
      + [移轉2](./expert-resources/cloud-5/cloud5-aem-content-migration-part-2.md)
      + [Dispatcher驗證器](./expert-resources/cloud-5/cloud5-aem-dispatcher-validator.md)
      + [搜尋和建立索引](./expert-resources/cloud-5/cloud5-aem-search-and-indexing.md)
      + [AdobeApp Builder](./expert-resources/cloud-5/cloud5-adobe-app-builder.md)
      + 第2季{#season-2}
         + [片段](./expert-resources/cloud-5/season-2/cloud5-experience-v-content-fragments.md)
         + [Repo Modernizer](./expert-resources/cloud-5/season-2/cloud5-repo-modernizer.md)
         + [Admin Console](./expert-resources/cloud-5/season-2/cloud5-admin-console.md)
         + [REPOINIT](./expert-resources/cloud-5/season-2/cloud5-repoinit.md)
         + [Sling工作排程器](./expert-resources/cloud-5/season-2/cloud5-sling-job-scheduler.md)
         + [修正您的快取](./expert-resources/cloud-5/season-2/cloud5-fix-your-cache.md)
         + [修正您的重新寫入](./expert-resources/cloud-5/season-2/cloud5-fix-your-rewrites.md)
         + [Cloud Manager — 體驗稽核](./expert-resources/cloud-5/season-2/cloud5-mocm-experience-audit.md)
         + [Cloud Manager — 單元測試](./expert-resources/cloud-5/season-2/cloud5-mocm-unit-tests.md)
         + [Cloud Manager — 功能測試](./expert-resources/cloud-5/season-2/cloud5-mocm-functional-tests.md)
