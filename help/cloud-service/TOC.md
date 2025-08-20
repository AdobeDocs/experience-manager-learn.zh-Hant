---
user-guide-title: Adobe Experience Manager as a Cloud Service 教學課程
user-guide-description: Adobe Experience Manager as a Cloud Service 教學課程集合。
breadcrumb-title: AEM as a Cloud Service 教學課程
solution: Experience Manager, Experience Manager as a Cloud Service
sub-product: Experience Manager as a Cloud Service
version: Experience Manager as a Cloud Service
team: TM
source-git-commit: 7d6f6d710f7ecbe01359f54e0f51d3e84ec64373
workflow-type: tm+mt
source-wordcount: '1403'
ht-degree: 100%

---


# Adobe Experience Manager as a Cloud Service 教學課程 {#cloud-service}

+ [概觀](./overview.md)
+ AEM 試用版 {#aem-trials}
   + [影像](./aem-trials/images.md)
+ 播放清單{#playlists}
   + [AEM 開發](./playlists/development.md)
+ AEM as a Cloud Service 簡介{#introduction}
   + [什麼是 AEM as a Cloud Service？](./introduction/what-is-aem-as-a-cloud-service.md)
   + [架構](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
   + 策略與思想領導力{#strategy}
      + [Experience Manager：治理與人力配置模型和原型](./introduction/experience-manager-governance-and-staffing-models.md)
+ Experience Cloud 整合{#integrations}
   + [整合](./integrations/experience-cloud.md)
   + [AEM Headless 和 Target](./integrations/target.md)
+ 基礎技術 {#underlying-technology}
   + [AEM 架構](./underlying-technology/introduction-architecture.md)
   + [OSGi](./underlying-technology/introduction-osgi.md)
   + [Java 內容倉庫](./underlying-technology/introduction-jcr.md)
   + [Sling](./underlying-technology/introduction-sling.md)
   + [Author 與 Publish 服務](./underlying-technology/introduction-author-publish.md)
   + [Dispatcher](./underlying-technology/introduction-dispatcher.md)
+ Edge Delivery Services {#edge-delivery-services}
   + [AEM Assets Sidekick 外掛程式](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/edge-delivery-services/sidekick-plugin.html){target=_blank}
+ Cloud Manager {#cloud-manager}
   + [方案](./cloud-manager/programs.md)
   + [環境](./cloud-manager/environments.md)
   + [使用 GitHub 存放庫](./cloud-manager/byogithub.md)
   + [CI/CD 生產管道](./cloud-manager/cicd-production-pipeline.md)
   + [CI/CD 非生產管道](./cloud-manager/cicd-non-production-pipeline.md)
   + [活動](./cloud-manager/activity.md)
   + [自訂網域名稱](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/cloud-service/content-delivery/custom-domain-names){target=_blank}
   + [內容還原](./cloud-manager/content-restore.md)
   + Dev Ops{#devops}
      + [部署程式碼](./cloud-manager/devops/deploy-code.md)
      + [合併專案](./cloud-manager/devops/merge-projects.md)
      + [設定管道](./cloud-manager/devops/configure-pipelines.md)
      + [持續整合](./cloud-manager/devops/continuous-integration.md)
      + [分析測試結果](./cloud-manager/devops/analyze-test-results.md)
      + [Dispatcher 設定](./cloud-manager/devops/dispatcher-configurations.md)
      + [CDN 記錄分析](./cloud-manager/devops/cdn-log-analysis.md)
+ 本機開發環境設定 {#local-development-environment-set-up}
   + [概觀](./local-development-environment/overview.md)
   + [開發工具](./local-development-environment/development-tools.md)
   + [本機 AEM SDK](./local-development-environment/aem-runtime.md)
   + [本機 Dispatcher 工具](./local-development-environment/dispatcher-tools.md)
+ 開發{#developing}
   + 擴展性{#extensibility}
      + App Builder{#app-builder}
         + [產生 JWT 存取權杖](./developing/extensibility/app-builder/jwt-auth.md)
         + [產生伺服器對伺服器存取權杖](./developing/extensibility/app-builder/server-to-server-auth.md)
         + [Github Webhook 驗證](./developing/extensibility/app-builder/github-webhook-verification.md)
      + 使用者介面擴展性{#ui}
         + [概觀](./developing/extensibility/ui/overview.md)
         + [Adobe Developer Console 專案](./developing/extensibility/ui/adobe-developer-console-project.md)
         + [初始化應用程式](./developing/extensibility/ui/app-initialization.md)
         + [註冊擴充功能](./developing/extensibility/ui/extension-registration.md)
         + [模態視窗](./developing/extensibility/ui/modal.md)
         + [Adobe I/O Runtime 動作](./developing/extensibility/ui/runtime-action.md)
         + [驗證](./developing/extensibility/ui/verify.md)
         + [部署](./developing/extensibility/ui/deploy.md)
         + 內容片段{#content-fragments}
            + [概觀](./developing/extensibility/ui/content-fragments/overview.md)
            + 範例{#examples}
               + [AI 影像生成](./developing/extensibility/ui/content-fragments/examples/console-image-generation-and-image-upload.md)
               + [大量屬性更新](./developing/extensibility/ui/content-fragments/examples/console-bulk-property-update.md)
               + [自訂網格欄](./developing/extensibility/ui/content-fragments/examples/custom-grid-columns.md)
               + [匯出為 XML](./developing/extensibility/ui/content-fragments/examples/editor-export-to-xml.md)
               + [RTE 工具列按鈕](./developing/extensibility/ui/content-fragments/examples/editor-rte-toolbar.md)
               + [RTE 小工具](./developing/extensibility/ui/content-fragments/examples/editor-rte-widget.md)
               + [RTE 徽章](./developing/extensibility/ui/content-fragments/examples/editor-rte-badges.md)
               + [自訂欄位](./developing/extensibility/ui/content-fragments/examples/editor-custom-field.md)
   + 開發基礎知識{#basics}
      + [AEM SDK](./developing/basics/aem-sdk.md)
      + [本機開發環境](./developing/basics/local-development-environment.md)
      + [AEM 專案原型](./developing/basics/aem-project-archetype.md)
      + [AEM 專案結構](./developing/basics/project-structure.md)
      + [可變與不可變內容](./developing/basics/mutable-immutable.md)
      + [存放庫結構套件](./developing/basics/repository-structure-package.md)
      + [內容發佈](./developing/basics/content-publishing.md)
      + [OSGi 設定](./developing/basics/osgi-configurations.md)
      + [Dispatcher 設定移轉](./developing/basics/dispatcher-configuration.md)
   + AEM 專案{#aem-projects}
      + [AEM Maven 專案](./developing/projects/maven-project-structure.md)
      + [清理 AEM Maven 專案](./developing/projects/remove-samples.md)
   + OSGi 服務{#osgi-services}
      + [OSGi 服務基礎知識](./developing/osgi-services/basics.md)
      + [OSGi 元件生命週期](./developing/osgi-services/lifecycle.md)
      + [OSGi 設定基礎知識](./developing/osgi-services/configurations.md)
      + [使用 OCD 進行 OSGi 設定](./developing/osgi-services/configurations-ocd.md)
   + 進階{#advanced}
      + [快取頁面變體](./developing/advanced/variant-caching.md)
      + [CSRF 保護](./developing/advanced/csrf-protection.md)
      + [自訂命名空間](./developing/advanced/custom-namespaces.md)
      + [從 HTL 參數化 Sling 模型](./developing/advanced/sling-model-parameters.md)
      + [密碼](./developing/advanced/secrets.md)
      + [服務使用者](./developing/advanced/service-users.md)
      + [網頁最佳化的影像 API](./developing/advanced/web-optimized-image-delivery-java-apis.md)
      + [在 AEM Author 中的領導實例上執行工作](./developing/advanced/run-job-on-leader-instance-in-aem-author.md)
   + 快速開發環境{#rde}
      + [概觀](./developing/rde/overview.md)
      + [設定方法](./developing/rde/how-to-setup.md)
      + [使用方法](./developing/rde/how-to-use.md)
      + [開發生命週期](./developing/rde/development-life-cycle.md)
   + 通用編輯器{#universal-editor}
      + React 應用程式編輯{#react-app-editing}
         + [概觀](./developing/universal-editor/react-app/overview.md)
         + [本機開發設定](./developing/universal-editor/react-app/local-development-setup.md)
         + [配置 React 應用程式](./developing/universal-editor/react-app/instrument-to-edit-content.md)
   + [AEM SDK API JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/index.html){target=_blank}
+ AEM 偵錯{#debugging}
   + AEM SDK 偵錯{#debugging-aem-sdk}
      + [概觀](./debugging/aem-sdk-local-quickstart/overview.md)
      + [記錄](./debugging/aem-sdk-local-quickstart/logs.md)
      + [遠端偵錯](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [OSGi 網頁主控台](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Dispatcher 工具](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [其他工具](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + AEM as a Cloud Service 偵錯{#debugging-aem-as-a-cloud-service}
      + [概觀](./debugging/cloud-service/overview.md)
      + [記錄](./debugging/cloud-service/logs.md)
      + [建置和部署](./debugging/cloud-service/build-and-deployment.md)
      + [Developer Console](./debugging/cloud-service/developer-console.md)
      + [存放庫瀏覽器](./debugging/cloud-service/repository-browser.md)
      + 風險{#risks}
         + [周遊警告](./debugging/cloud-service/risks/traversals.md)
+ 個人化 {#personalization}
   + [概觀](./personalization/overview.md)
   + 設定{#setup}
      + [整合 Adobe Target](./personalization/setup/integrate-adobe-target.md)
      + [整合標記](./personalization/setup/integrate-adobe-tags.md)
   + 使用案例 {#use-cases}
      + [實驗 (A/B 測試)](./personalization/use-cases/experimentation.md)
+ AEM API{#aem-apis}
   + [概觀](./apis/overview.md)
   + OpenAPI{#openapis}
      + [概觀](./apis/openapis/overview.md)
      + [設定方法](./apis/openapis/setup.md)
      + [伺服器對伺服器驗證](./apis/openapis/use-cases/invoke-api-using-oauth-s2s.md)
      + [使用者驗證 (網頁應用程式)](./apis/openapis/use-cases/invoke-api-using-oauth-web-app.md)
      + [使用者驗證 (SPA)](./apis/openapis/use-cases/invoke-api-using-oauth-single-page-app.md)
      + 操作說明{#how-to}
         + [認證和產品設定檔管理](./apis/openapis/how-to/credentials-and-product-profile-management.md)
         + [權限管理](./apis/openapis/how-to/services-user-group-permission-management.md)
+ 內容傳遞{#content-delivery}
   + [自訂網域名稱](./content-delivery/custom-domain-names.md)
   + [使用 Adobe 受管理 CDN 的自訂網域名稱](./content-delivery/custom-domain-name-with-adobe-managed-cdn.md)
   + [使用客戶 CDN 的自訂網域名稱](./content-delivery/custom-domain-names-with-customer-managed-cdn.md)
   + [快取](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/cloud-service/caching/overview){target=_blank}
   + [Adobe CDN：快取之外的功能](./content-delivery/adobe-cdn-beyond-caching.md)
   + [自訂錯誤頁面](./content-delivery/custom-error-pages.md)
   + [URL 重新導向](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/administration/url-redirection.html){target=_blank}
+ 快取{#caching}
   + [概觀](./caching/overview.md)
   + [AEM Publish 服務](./caching/publish.md)
   + [AEM Author 服務](./caching/author.md)
   + [CDN 快取命中比例分析](./caching/cdn-cache-hit-ratio-analysis.md)
   + 操作說明{#how-to}
      + [啟用快取](./caching/how-to/enable-caching.md)
      + [停用快取](./caching/how-to/disable-caching.md)
      + [清除快取](./caching/how-to/purge-cache.md)
+ 存取 AEM{#accessing}
   + [概觀](./accessing/overview.md)
   + [Adobe IMS 使用者](./accessing/adobe-ims-users.md)
   + [Adobe IMS 使用者群組](./accessing/adobe-ims-user-groups.md)
   + [Adobe IMS 產品設定檔](./accessing/adobe-ims-product-profiles.md)
   + [AEM 使用者、群組和權限](./accessing/aem-users-groups-and-permissions.md)
   + [AEM 存取權設定逐步解說](./accessing/walk-through.md)
+ 驗證{#authentication}
   + [概觀](./authentication/authentication.md)
   + [SAML 2.0](./authentication/saml-2-0.md)
+ 進階網路{#networking}
   + [概觀](./networking/advanced-networking.md)
   + [彈性連接埠輸出](./networking/flexible-port-egress.md)
   + [專用輸出 IP 位址](./networking/dedicated-egress-ip-address.md)
   + [虛擬私人網路](./networking/vpn.md)
   + 程式碼範例{#examples}
      + [非標準連接埠上的 HTTP/HTTPS，用於彈性連接埠輸出](./networking/examples/http-on-non-standard-ports-flexible-port-egress.md)
      + [用於專用輸出 IP 位址/VPN 的 HTTP/HTTPS](./networking/examples/http-dedicated-egress-ip-vpn.md)
      + [使用 DataSourcePool 的 SQL 連線](./networking/examples/sql-datasourcepool.md)
      + [使用 Java SQL API 的 SQL 連線](./networking/examples/sql-java-apis.md)
      + [電子郵件服務](./networking/examples/email-service.md)
+ 安全性 {#security}
   + [使用流量篩選器規則封鎖 DoS/DDoS 攻擊](./security/blocking-dos-attack-using-traffic-filter-rules.md)
   + 流量篩選器規則包括 WAF 規則 {#traffic-filter-and-waf-rules}
      + [保護 AEM 網站](./security/traffic-filter-and-waf-rules/overview.md)
      + [設定方法](./security/traffic-filter-and-waf-rules/setup.md)
      + [使用流量篩選器規則](./security/traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md)
      + [使用 WAF 規則](./security/traffic-filter-and-waf-rules/use-cases/using-waf-rules.md)
      + [最佳實務](./security/traffic-filter-and-waf-rules/best-practices.md)
      + 操作說明{#how-to}
         + [監視敏感性要求](./security/traffic-filter-and-waf-rules/how-to/request-logging.md)
         + [限制存取權](./security/traffic-filter-and-waf-rules/how-to/request-blocking.md)
         + [標準化要求](./security/traffic-filter-and-waf-rules/how-to/request-transformation.md)
+ AEM Eventing{#aem-eventing}
   + [概觀](./eventing/overview.md)
   + 範例{#examples}
      + [Webhook - 接收 AEM 事件](./eventing/examples/webhook.md)
      + [編寫日誌 - 載入 AEM 事件](./eventing/examples/journaling.md)
      + [Adobe I/O Runtime 動作 - 接收 AEM 事件](./eventing/examples/runtime-action.md)
      + [Adobe I/O Runtime 動作 - 處理 AEM 事件](./eventing/examples/event-processing-using-runtime-action.md)
      + [AEM Assets 事件 - PIM 整合](./eventing/examples/assets-pim-integration.md)
+ 移轉 {#migration}
   + [內容轉移工具](./migration/content-transfer-tool.md)
   + [大量匯入資產](./migration/bulk-import.md)
   + 移至 AEM as a Cloud Service {#moving-to-aem-as-a-cloud-service}
      + [簡介](./migration/moving-to-aem-as-a-cloud-service/introduction.md)
      + [上線](./migration/moving-to-aem-as-a-cloud-service/onboarding.md)
      + [Cloud Manager](./migration/moving-to-aem-as-a-cloud-service/cloud-manager.md)
      + [BPA 和 CAM](./migration/moving-to-aem-as-a-cloud-service/bpa-and-cam.md)
      + [AEM 現代化工具](./migration/moving-to-aem-as-a-cloud-service/aem-modernization-tools.md)
      + [存放庫現代化](./migration/moving-to-aem-as-a-cloud-service/repository-modernization.md)
      + [Asset Compute 微服務](./migration/moving-to-aem-as-a-cloud-service/asset-compute-microservices.md)
      + [Dispatcher](./migration/moving-to-aem-as-a-cloud-service/dispatcher.md)
      + [搜尋和索引](./migration/moving-to-aem-as-a-cloud-service/search-and-indexing.md)
      + 內容移轉 {#content-migration}
         + [大量匯入服務](./migration/moving-to-aem-as-a-cloud-service/content-migration/bulk-import-service.md)
         + [內容轉移工具](./migration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.md)
         + [常見問題](./migration/moving-to-aem-as-a-cloud-service/content-migration/faq.md)
      + [疑難排解](./migration/moving-to-aem-as-a-cloud-service/troubleshooting.md)
      + AEM Forms as a Cloud Service {#aem-forms}
         + [簡介](./migration/moving-to-aem-as-a-cloud-service/aem-forms/introduction.md)
         + [數位註冊](./migration/moving-to-aem-as-a-cloud-service/aem-forms/digital-enrollment.md)
         + [通訊](./migration/moving-to-aem-as-a-cloud-service/aem-forms/communications.md)
   + Cloud Acceleration Manager {#cloud-acceleration-manager}
      + [簡介](./migration/cloud-acceleration-manager/introduction.md)
      + [準備程度與最佳做法分析工具](./migration/cloud-acceleration-manager/readiness-and-best-practice-analyzer.md)
      + [實施階段](./migration/cloud-acceleration-manager/implementation-phase.md)
      + [程式碼重構工具](./migration/cloud-acceleration-manager/code-refactoring-tools.md)
      + [程式碼存放庫現代化工具](./migration/cloud-acceleration-manager/code-repository-modernizer.md)
      + [Dispatcher 轉換工具](./migration/cloud-acceleration-manager/dispatcher-converter.md)
      + [索引轉換工具](./migration/cloud-acceleration-manager/index-converter.md)
      + [資產工作流程移轉工具](./migration/cloud-acceleration-manager/asset-workflow-migration-tool.md)
      + [導覽 Cloud Acceleration Manager](./migration/cloud-acceleration-manager/navigating.md)
      + [使用 Cloud Acceleration Manager](./migration/cloud-acceleration-manager/using.md)
+ [內容片段](https://experienceleague.adobe.com/docs/experience-manager-learn/content-fragments-console/overview.html){target=_blank}
+ Forms{#forms}
   + 針對 Forms as a Cloud Service 開發{#developing-for-cloud-service}
      + [1 - 快速入門](./forms/developing-for-cloud-service/getting-started.md)
      + [2 - 安裝 IntelliJ](./forms/developing-for-cloud-service/intellij-set-up.md)
      + [3 - 設定 Git](./forms/developing-for-cloud-service/setup-git.md)
      + [4 - 將 IntelliJ 與 AEM 同步](./forms/developing-for-cloud-service/intellij-and-aem-sync.md)
      + [5 - 建置表單](./forms/developing-for-cloud-service/deploy-your-first-form.md)
      + [6 - 自訂提交處理常式](./forms/developing-for-cloud-service/custom-submit-to-servlet.md)
      + [7 - 使用資源類型註冊 servlet](./forms/developing-for-cloud-service/registering-servlet-using-resourcetype.md)
      + [8 - 啟用表單入口元件](./forms/developing-for-cloud-service/forms-portal-components.md)
      + [9 - 納入雲端服務和 FDM](./forms/developing-for-cloud-service/azure-storage-fdm.md)
      + [10 - 內容感知雲端設定](./forms/developing-for-cloud-service/context-aware-fdm.md)
      + [11 - 推送至 Cloud Manager](./forms/developing-for-cloud-service/push-project-to-cloud-manager-git.md)
      + [12 - 部署至開發環境](./forms/developing-for-cloud-service/deploy-to-dev-environment.md)
      + [13 - 更新 Maven 原型](./forms/developing-for-cloud-service/updating-project-archetype.md)
   + 建立自適應表單{#create-first-af}
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
      + [新增及設定工具列](./forms/create-first-af/add-configure-toolbar.md)
   + 使用無周邊表單的自訂提交服務{#custom-submit-headless-forms}
      + [1 - 簡介](./forms/custom-submit-headless-forms/introduction.md)
      + [2 - 建立自訂提交服務](./forms/custom-submit-headless-forms/custom-submit-service.md)
      + [3 - 顯示回應](./forms/custom-submit-headless-forms/handle-response-react-app.md)
   + 建立地址區塊元件{#create-address-block}
      + [1 - 簡介](./forms/create-address-block-component/introduction.md)
      + [2 - 設定](./forms/create-address-block-component/set-up.md)
      + [3 - 建立元件](./forms/create-address-block-component/creating-address-component.md)
      + [4 - 部署元件](./forms/create-address-block-component/deploy-your-project.md)
   + 建立可點按的影像元件{#clickable-image-component}
      + [1 - 簡介](./forms/clickable-image-component/introduction.md)
      + [2 - 建立元件](./forms/clickable-image-component/create-component.md)
      + [3 - 處理點按事件](./forms/clickable-image-component/handle-click-event.md)
   + AEM Forms 和 Analytics{#forms-and-analytics}
      + [簡介](./forms/form-data-analytics/introduction.md)
      + [建立資料元素](./forms/form-data-analytics/data-elements.md)
      + [建立規則](./forms/form-data-analytics/rules.md)
      + [測試解決方案](./forms/form-data-analytics/test.md)
   + 建立國家/地區下拉式選單元件{#countries-drop-down}
      + [簡介](./forms/countries-drop-down/introduction.md)
      + [建立元件](./forms/countries-drop-down/component.md)
      + [建立對話框](./forms/countries-drop-down/dialog.md)
      + [建立 Sling 模型](./forms/countries-drop-down/slingmodel.md)
      + [建置與測試](./forms/countries-drop-down/build.md)
   + 建立按鈕變化版本{#style-system}
      + [簡介](./forms/style-system/introduction.md)
      + [定義原則](./forms/style-system/style-policy.md)
      + [定義變化版本](./forms/style-system/create-variations.md)
      + [測試變化版本](./forms/style-system/build.md)
   + 使用垂直索引標籤{#using-vertical-tabs}
      + [&#x200B;1. 簡介](./forms/using-vertical-tabs/introduction.md)
      + [&#x200B;2. 建立表單](./forms/using-vertical-tabs/create-af.md)
      + [&#x200B;3. 導覽](./forms/using-vertical-tabs/navigation.md)
      + [&#x200B;4. 新增圖示](./forms/using-vertical-tabs/icons.md)
   + 使用 Output 和 Forms 服務{#forms-cs-output-and-forms-service}
      + [產生 PDF](./forms/forms-cs-output-and-forms-service/outputservice.md)
   + 在 AEM Forms CS 中產生文件{#doc-gen-formscs}
      + [簡介](./forms/doc-gen-forms-cs/introduction.md)
      + [建立服務認證](./forms/doc-gen-forms-cs/service-credentials.md)
      + [建立 JWT 權杖](./forms/doc-gen-forms-cs/create-jwt.md)
      + [建立存取權杖](./forms/doc-gen-forms-cs/create-access-token.md)
      + [合併資料與範本](./forms/doc-gen-forms-cs/merge-data-with-template.md)
      + [測試解決方案](./forms/doc-gen-forms-cs/test.md)
      + [挑戰](./forms/doc-gen-forms-cs/challenge.md)
   + 使用 Forms Document Services API{#forms-document-services-api}
      + [簡介](./forms/forms-document-services/introduction.md)
      + [設定 OpenAPI](./forms/forms-document-services/using-open-api.md)
      + [產生存取權杖](./forms/forms-document-services/generate-access-token.md)
      + [套用使用權限](./forms/forms-document-services/make-api-calls.md)
      + [程式碼範例](./forms/forms-document-services/sample-project.md)
   + 使用批次 API 產生文件{#formscs-batch-api}
      + [簡介](./forms/formscs-batch-api/introduction.md)
      + [設定 Azure 儲存體](./forms/formscs-batch-api/configure-azure-storage.md)
      + [建立 USC 批次設定](./forms/formscs-batch-api/configure-usc-batch.md)
      + [建立批次設定](./forms/formscs-batch-api/create-batch-config.md)
      + [執行批次](./forms/formscs-batch-api/execute-batch-generate-documents.md)
   + 在 Forms CS 中操作 PDF{#forms-cs-assembler}
      + [簡介](./forms/forms-cs-assembler/introduction.md)
      + [建立服務認證](./forms/forms-cs-assembler/service-credentials.md)
      + [建立 JWT 權杖](./forms/forms-cs-assembler/create-jwt.md)
      + [建立存取權杖](./forms/forms-cs-assembler/create-access-token.md)
      + [組裝 PDF 檔案](./forms/forms-cs-assembler/assemble-pdf-files.md)
      + [PDF/A 公用程式](./forms/forms-cs-assembler/pdfa-utilities.md)
      + [測試解決方案](./forms/forms-cs-assembler/test.md)
      + [挑戰](./forms/forms-cs-assembler/challenge.md)
   + 使用 Blob 索引標籤儲存表單提交{#store-submiited-data-with-metadata-tags}
      + [簡介](./forms/store-submiited-data-with-metadata-tags/introduction.md)
      + [擴展選擇群組元件](./forms/store-submiited-data-with-metadata-tags/extend-choice-group-components.md)
      + [建立 OSGi 設定](./forms/store-submiited-data-with-metadata-tags/create-osgi-configuration.md)
      + [建立索引標籤](./forms/store-submiited-data-with-metadata-tags/create-blob-index-tags.md)
      + [建立自訂提交](./forms/store-submiited-data-with-metadata-tags/create-custom-submit.md)
   + 預填以核心元件為基礎的表單{#prefill-core-component-based-form}
      + [簡介](./forms/prefill-core-component-form/introduction.md)
      + [編寫預填服務](./forms/prefill-core-component-form/pre-fill-service.md)
      + [測試解決方案](./forms/prefill-core-component-form/test-solution.md)
   + Azure 入口網站儲存體{#forms-cs-azure-portal}
      + [簡介](./forms/forms-cs-azure-portal/introduction.md)
      + [建立表單資料模型](./forms/forms-cs-azure-portal/create-fdm.md)
      + [將表單資料儲存在 Azure 儲存體中](./forms/forms-cs-azure-portal/create-af.md)
      + [預填表單](./forms/forms-cs-azure-portal/prefill-af-storage.md)
      + [提交查詢](./forms/forms-cs-azure-portal/query-submitted-data.md)
   + 儲存並繼續填寫表單{#prefill-azure-storage}
      + [1 - 簡介](./forms/prefill-azure-storage/introduction.md)
      + [2 - 建立頁面元件](./forms/prefill-azure-storage/page-component.md)
      + [3 - 建立自適應表單範本](./forms/prefill-azure-storage/associate-page-component.md)
      + [4 - 建立 Azure 儲存體整合](./forms/prefill-azure-storage/create-fdm.md)
      + [5 - 建立 SendGrid 整合](./forms/prefill-azure-storage/send-grid-fdm.md)
      + [6 - 建立自適應表單](./forms/prefill-azure-storage/create-af.md)
      + [7 - 部署範例資產](./forms/prefill-azure-storage/deploy-sample-assets.md)

   + 建立檢閱工作流程{#create-aem-workflow}
      + [將工作流程儲存於外部](./forms/create-aem-workflow/externalize-workflow.md)
      + [建立工作流程模型](./forms/create-aem-workflow/create-workflow.md)
      + [觸發工作流程](./forms/create-aem-workflow/configure-af.md)
   + Acrobat Sign 與 AEM Forms{#forms-and-sign}
      + [簡介](./forms/forms-and-sign/introduction.md)
      + [Acrobat Sign API 應用程式](./forms/forms-and-sign/create-sign-api-application.md)
      + [Acrobat Sign 雲端設定](./forms/forms-and-sign/create-adobe-sign-cloud-configuration.md)
      + [建立自適應表單](./forms/forms-and-sign/create-adaptive-form.md)
      + [設定填寫及簽署](./forms/forms-and-sign/configure-form-fill-and-sign.md)
   + 與 Microsoft Power Automate 整合{#forms-cs-and-power-automate}
      + [設定整合](./forms/forms-cs-and-power-automate/integrate-formscs-power-automate.md)
      + [剖析提交的表單資料](./forms/forms-cs-and-power-automate/send-email-notification.md)
      + [以電子郵件附件的形式傳送記錄文件](./forms/forms-cs-and-power-automate/send-dor-email-attachment.md)
      + [從提交的資料中擷取表單附件](./forms/forms-cs-and-power-automate/send-af-attachments-in-email.md)
   + 與 Microsoft Dynamics 整合{#formscs-dynamics-crm}
      + [建立 Dynamics 應用程式](./forms/formscs-dynamics-crm/create-dynamics-account.md)
      + [設定資料來源](./forms/formscs-dynamics-crm/configure-odata-data-source.md)
      + [建立表單資料模型](./forms/formscs-dynamics-crm/create-form-data-model.md)
      + [建立自適應表單](./forms/formscs-dynamics-crm/create-adaptive-form.md)
   + 與 Salesforce 整合{#integrate-with-salesforce}
      + [簡介](./forms/integrate-with-salesforce/introduction.md)
      + [建立連接的應用程式](./forms/integrate-with-salesforce/create-connected-app.md)
      + [建立 Swagger 檔案](./forms/integrate-with-salesforce/describe-rest-api.md)
      + [建立資料來源](./forms/integrate-with-salesforce/create-data-source.md)
      + [建立表單資料模型](./forms/integrate-with-salesforce/create-form-data-model.md)
      + [測試表單提交](./forms/integrate-with-salesforce/create-lead-submitting-form.md)
      + [測試點按事件](./forms/integrate-with-salesforce/create-lead-click-event.md)
   + 將表單提交儲存在 OneDrive 和 SharePoint 中{#one-drive}
      + [將表單資料儲存在 OneDrive 中](./forms/forms-cs-one-drive/store-form-submission-one-drive.md)
      + [將表單資料儲存在 Sharepoint 中](./forms/forms-cs-sharepoint/store-form-submission-in-sharepoint.md)
      + [使用 SharePoint 清單中的資料預填表單](./forms/forms-cs-sharepoint/prefill-data-from-sharepoint-list.md)
      + [使用工作流程將資料插入 SharePoint 清單內](./forms/forms-cs-sharepoint/submit-data-sharepoint-list-workflow.md)
+ Asset Compute 擴展性{#asset-compute}
   + [概觀](./asset-compute/overview.md)
   + 建立{#set-up}
      + [帳戶和服務佈建](./asset-compute/set-up/accounts-and-services.md)
      + [本機開發環境](./asset-compute/set-up/development-environment.md)
      + [App Builder](./asset-compute/set-up/app-builder.md)
   + 開發{#develop}
      + [建立 Asset Compute 專案](./asset-compute/develop/project.md)
      + [設定環境變數](./asset-compute/develop/environment-variables.md)
      + [設定 manifest.yml](./asset-compute/develop/manifest.md)
      + [開發工作程式](./asset-compute/develop/worker.md)
      + [使用開發工具](./asset-compute/develop/development-tool.md)
   + 測試與偵錯{#test-debug}
      + [工作程式測試](./asset-compute/test-debug/test.md)
      + [工作程式偵錯](./asset-compute/test-debug/debug.md)
   + 部署{#deploy}
      + [部署至 Adobe I/O Runtime](./asset-compute/deploy/runtime.md)
      + [與 AEM 整合](./asset-compute/deploy/processing-profiles.md)
   + 進階{#advanced}
      + [中繼資料工作程式](./asset-compute/advanced/metadata.md)
   + [疑難排解](./asset-compute/troubleshooting.md)

+ 多步驟教學課程{#multi-step-tutorials}
   + [AEM Sites 開發](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html){target=_blank}
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html){target=_blank}
   + [SPA 編輯器 (React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html){target=_blank}
   + [AEM Sites 和 Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html){target=_blank}
   + [權杖型驗證](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html){target=_blank}
+ 專家資源 {#expert-resources}
   + AEM Champion {#aem-champions}
      + [Cloud Manager 上線教戰手冊](./expert-resources/aem-champions/onboarding-playbook.md)
      + [Cloud Manager 環境類型](./expert-resources/aem-champions/environment-types.md)
      + [Cloud Manager 使用者介面](./expert-resources/aem-champions/cloud-manager-ui.md)
   + [AEM 專家系列](./expert-resources/expert-series/aem-experts-series.md)
   + Cloud 5{#cloud-5}
      + [簡介](./expert-resources/cloud-5/cloud5-introduction.md)
      + [第 4 季](./expert-resources/cloud-5/cloud5-season-4.md)
      + [第 3 季](./expert-resources/cloud-5/cloud5-season-3.md)
      + [第 2 季](./expert-resources/cloud-5/cloud5-season-2.md)
      + [第 1 季](./expert-resources/cloud-5/cloud5-season-1.md)
      + [AEM CDN 第 1 部分](./expert-resources/cloud-5/cloud5-aem-cdn-part1.md)
      + [AEM CDN 第 2 部分](./expert-resources/cloud-5/cloud5-aem-cdn-part2.md)
      + [AEM 記錄檔案](./expert-resources/cloud-5/cloud5-aem-log-files.md)
      + [登入權杖](./expert-resources/cloud-5/cloud5-getting-login-token-integrations.md)
      + [雲端 Dispatcher](./expert-resources/cloud-5/cloud5-aem-dispatcher-cloud.md)
      + [移轉 1](./expert-resources/cloud-5/cloud5-aem-content-migration-part-1.md)
      + [Dispatcher 驗證工具](./expert-resources/cloud-5/cloud5-aem-dispatcher-validator.md)
      + [搜尋和索引](./expert-resources/cloud-5/cloud5-aem-search-and-indexing.md)
      + [Adobe App Builder](./expert-resources/cloud-5/cloud5-adobe-app-builder.md)
      + 第 2 季{#season-2}
         + [片段](./expert-resources/cloud-5/season-2/cloud5-experience-v-content-fragments.md)
         + [存放庫現代化工具](./expert-resources/cloud-5/season-2/cloud5-repo-modernizer.md)
         + [Admin Console](./expert-resources/cloud-5/season-2/cloud5-admin-console.md)
         + [REPOINIT](./expert-resources/cloud-5/season-2/cloud5-repoinit.md)
         + [Sling 工作排程工具](./expert-resources/cloud-5/season-2/cloud5-sling-job-scheduler.md)
         + [修正快取](./expert-resources/cloud-5/season-2/cloud5-fix-your-cache.md)
         + [修正重寫](./expert-resources/cloud-5/season-2/cloud5-fix-your-rewrites.md)
         + [Cloud Manager - 體驗稽核](./expert-resources/cloud-5/season-2/cloud5-mocm-experience-audit.md)
         + [Cloud Manager - 單位測試](./expert-resources/cloud-5/season-2/cloud5-mocm-unit-tests.md)
         + [Cloud Manager - 功能測試](./expert-resources/cloud-5/season-2/cloud5-mocm-functional-tests.md)
      + 第 3 季{#season-3}
         + [第三方搜尋](./expert-resources/cloud-5/season-3/cloud5-3rd-party-search.md)
         + [Edge 工作程式](./expert-resources/cloud-5/season-3/cloud5-edge-workers.md)
         + [在 Edge Delivery Services 中發佈和取消發佈事件](./expert-resources/cloud-5/season-3/cloud5-publish-events.md)
         + [查詢索引和 Excel 公式](./expert-resources/cloud-5/season-3/cloud5-query-indexes.md)
         + [自備 Cloudflare CDN](./expert-resources/cloud-5/season-3/cloud5-byo-cloudflare-cdn.md)
         + [整合 AEM Assets](./expert-resources/cloud-5/season-3/cloud5-integrate-assets.md)
         + [適用於 AEM Sites 的生成式 AI](./expert-resources/cloud-5/season-3/cloud5-generative-ai-for-aem-sites.md)
         + [探索通用編輯器](./expert-resources/cloud-5/season-3/cloud5-exploring-universal-editor.md)
         + [匯入網站](./expert-resources/cloud-5/season-3/cloud5-import-sites-to-edge-delivery-services.md)
         + [使用管理員 API](./expert-resources/cloud-5/season-3/cloud5-using-admin-api.md)
         + [獲得最佳 Lighthouse 分數 - 第 1 部分](./expert-resources/cloud-5/season-3/cloud5-lighthouse-score-optimization-part1.md)
         + [獲得最佳 Lighthouse 分數 - 第 2 部分](./expert-resources/cloud-5/season-3/cloud5-lighthouse-score-optimization-part2.md)
         + [獲得最佳 Lighthouse 分數 - 第 3 部分](./expert-resources/cloud-5/season-3/cloud5-lighthouse-score-optimization-part3.md)
      + 第 4 季{#season-4}
         + [最佳實務](./expert-resources/cloud-5/season-4/cloud5-edge-delivery-services-best-practices.md)
         + [搜尋最佳化](./expert-resources/cloud-5/season-4/cloud5-search-optimization.md)
         + [Google 地圖](./expert-resources/cloud-5/season-4/cloud5-google-maps.md)
