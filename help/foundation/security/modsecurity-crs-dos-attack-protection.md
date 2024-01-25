---
title: 使用ModSecurity保護您的AEM網站免受DoS攻擊
description: 瞭解如何使用OWASP ModSecurity核心規則集(CRS)啟用ModSecurity，以保護您的網站免受拒絕服務(DoS)攻擊。
feature: Security
version: 6.5, Cloud Service
topic: Security, Development
role: Admin, Architect
level: Experienced
jira: KT-10385
thumbnail: KT-10385.png
doc-type: Article
last-substantial-update: 2023-08-18T00:00:00Z
exl-id: 9f689bd9-c846-4c3f-ae88-20454112cf9a
duration: 874
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1172'
ht-degree: 0%

---

# 使用ModSecurity保護AEM網站免受DoS攻擊

瞭解如何啟用ModSecurity，使用保護網站免受拒絕服務(DoS)攻擊 **OWASP ModSecurity核心規則集(CRS)** 在Adobe Experience Manager (AEM) Publish Dispatcher上。


>[!VIDEO](https://video.tv.adobe.com/v/3422976?quality=12&learn=on)

## 概觀

此 [開啟Web Application Security Project® (OWASP)](https://owasp.org/) foundation提供 [**OWASP前10名**](https://owasp.org/www-project-top-ten/) 概述Web應用程式十大最重要的安全性考量。

ModSecurity是開放原始碼的跨平台解決方案，可針對針對Web應用程式的一系列攻擊提供保護。 它也允許進行HTTP流量監視、記錄和即時分析。

OWSAP®也提供 [OWASP® ModSecurity核心規則集(CRS)](https://github.com/coreruleset/coreruleset). CRS是一組泛型 **攻擊偵測** 與ModSecurity搭配使用的規則。 因此，CRS的目標是保護網路應用程式免受各種攻擊，包括OWASP Top Ten，而且最少會發出錯誤警示。

本教學課程示範如何啟用及設定 **DOS保護** CRS規則可保護您的網站免受潛在的DoS攻擊。

>[!TIP]
>
>請務必注意，AEMas a Cloud Service的 [受管理的CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html) 符合大多數客戶的效能與安全性需求。 不過，ModSecurity可提供額外的安全層，並允許客戶特定的規則和設定。

## 將CRS新增到Dispatcher專案模組

1. 下載並解壓縮 [最新的OWASP ModSecurity核心規則集](https://github.com/coreruleset/coreruleset/releases).

   ```shell
   # Replace the X.Y.Z with relevent version numbers.
   $ wget https://github.com/coreruleset/coreruleset/archive/refs/tags/vX.Y.Z.tar.gz
   
   # For version v3.3.5 when this tutorial is published
   $ wget https://github.com/coreruleset/coreruleset/archive/refs/tags/v3.3.5.tar.gz
   
   # Extract the downloaded file
   $ tar -xvzf coreruleset-3.3.5.tar.gz
   ```

1. 建立 `modsec/crs` 資料夾範圍 `dispatcher/src/conf.d/` 在您的AEM專案程式碼中。 例如，在 [AEM WKND網站專案](https://github.com/adobe/aem-guides-wknd).

   ![AEM專案程式碼中的CRS資料夾 — ModSecurity](assets/modsecurity-crs/crs-folder-in-aem-dispatcher-module.png){width="200" zoomable="yes"}

1. 複製 `coreruleset-X.Y.Z/rules` 資料夾（從下載的CRS發行套件下載至） `dispatcher/src/conf.d/modsec/crs` 資料夾。
1. 複製 `coreruleset-X.Y.Z/crs-setup.conf.example` 從下載的CRS發行套件中的檔案至 `dispatcher/src/conf.d/modsec/crs` 資料夾並將其重新命名為 `crs-setup.conf`.
1. 停用以下位置的所有CRS規則： `dispatcher/src/conf.d/modsec/crs/rules` 將它們重新命名為 `XXXX-XXX-XXX.conf.disabled`. 您可以使用下列指令一次重新命名所有檔案。

   ```shell
   # Go inside the newly created rules directory within the dispathcher module
   $ cd dispatcher/src/conf.d/modsec/crs/rules
   
   # Rename all '.conf' extension files to '.conf.disabled'
   $ for i in *.conf; do mv -- "$i" "$i.disabled"; done
   ```

   請參閱WKND專案程式碼中重新命名的CRS規則和設定檔案。

   ![AEM專案程式碼中已停用CRS規則 — ModSecurity ](assets/modsecurity-crs/disabled-crs-rules.png){width="200" zoomable="yes"}

## 啟用並設定拒絕服務(DoS)保護規則

若要啟用並設定拒絕服務(DoS)保護規則，請遵循下列步驟：

1. 重新命名以啟用DoS保護規則 `REQUEST-912-DOS-PROTECTION.conf.disabled` 至 `REQUEST-912-DOS-PROTECTION.conf` (或移除 `.disabled` ，此路徑為 `dispatcher/src/conf.d/modsec/crs/rules` 資料夾。
1. 透過定義  **DOS_COUNTER_THRESHOLD， DOS_BURST_TIME_SLICE， DOS_BLOCK_TIMEOUT** 變數。
   1. 建立 `crs-setup.custom.conf` 內的檔案 `dispatcher/src/conf.d/modsec/crs` 資料夾。
   1. 將下列規則片段新增至新建立的檔案。

   ```
   # The Denial of Service (DoS) protection against clients making requests too quickly.
   # When a client is making more than 25 requests (excluding static files) within
   # 60 seconds, this is considered a 'burst'. After two bursts, the client is
   # blocked for 600 seconds.
   SecAction \
       "id:900700,\
       phase:1,\
       nolog,\
       pass,\
       t:none,\
       setvar:'tx.dos_burst_time_slice=60',\
       setvar:'tx.dos_counter_threshold=25',\
       setvar:'tx.dos_block_timeout=600'"    
   ```

在此範例規則設定中， **DOS_COUNTER_THRESHOLD** 為25， **DOS_BURST_TIME_SLICE** 為60秒，且 **DOS_BLOCK_TIMEOUT** 逾時為600秒。 此設定會在60秒內識別超過2次出現的25個請求（不包括靜態檔案）符合DoS攻擊的資格，導致請求使用者端被封鎖600秒（或10分鐘）。

>[!WARNING]
>
>若要根據您的需求定義適當的值，請與您的Web安全性團隊共同作業。

## 初始化CRS

若要初始化CRS，請移除常見的誤判，並為您的網站新增本機例外，請遵循下列步驟：

1. 若要初始化CRS，請移除 `.disabled` 從 **REQUEST-901-INITIALIZATION** 檔案。 換言之，重新命名 `REQUEST-901-INITIALIZATION.conf.disabled` 檔案到 `REQUEST-901-INITIALIZATION.conf`.
1. 若要移除本機IP (127.0.0.1) ping等常見的誤判，請移除 `.disabled` 從 **REQUEST-905-COMMON-EXCEPTIONS** 檔案。
1. 若要新增本機例外狀況(例如AEM平台或您的網站專用路徑)，請重新命名 `REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf.example` 至 `REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf`
   1. 將AEM平台專屬的路徑例外新增至新重新命名的檔案。

   ```
   ########################################################
   # AEM as a Cloud Service exclusions                    #
   ########################################################
   # Ignoring AEM-CS Specific internal and reserved paths
   
   SecRule REQUEST_URI "@beginsWith /systemready" \
       "id:1010,\
       phase:1,\
       pass,\
       nolog,\
       ctl:ruleEngine=Off"    
   
   SecRule REQUEST_URI "@beginsWith /system/probes" \
       "id:1011,\
       phase:1,\
       pass,\
       nolog,\
       ctl:ruleEngine=Off"
   
   SecRule REQUEST_URI "@beginsWith /gitinit-status" \
       "id:1012,\
       phase:1,\
       pass,\
       nolog,\
       ctl:ruleEngine=Off"
   
   ########################################################
   # ADD YOUR SITE related exclusions                     #
   ########################################################
   ...
   ```

1. 另外，請移除 `.disabled` 從 **REQUEST-910-IP-REPOSITANCE.conf.disabled** 用於IP信譽區塊檢查和 `REQUEST-949-BLOCKING-EVALUATION.conf.disabled` 用於異常分數檢查。

>[!TIP]
>
>在AEM 6.5上設定時，請務必使用可驗證AEM健全狀態的個別AMS或內部部署路徑（亦稱為心率路徑）來取代上述路徑。

## 新增ModSecurity Apache設定

啟用ModSecurity (亦稱為 `mod_security` Apache模組)，請遵循以下步驟：

1. 建立 `modsecurity.conf` 在 `dispatcher/src/conf.d/modsec/modsecurity.conf` 搭配以下主要設定。

   ```
   # Include the baseline crs setup
   Include conf.d/modsec/crs/crs-setup.conf
   
   # Include your customizations to crs setup if exist
   IncludeOptional conf.d/modsec/crs/crs-setup.custom.conf
   
   # Select all available CRS rules:
   #Include conf.d/modsec/crs/rules/*.conf
   
   # Or alternatively list only specific ones you want to enable e.g.
   Include conf.d/modsec/crs/rules/REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf
   Include conf.d/modsec/crs/rules/REQUEST-901-INITIALIZATION.conf
   Include conf.d/modsec/crs/rules/REQUEST-905-COMMON-EXCEPTIONS.conf
   Include conf.d/modsec/crs/rules/REQUEST-910-IP-REPUTATION.conf
   Include conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf
   Include conf.d/modsec/crs/rules/REQUEST-949-BLOCKING-EVALUATION.conf
   
   # Start initially with engine off, then switch to detection and observe, and when sure enable engine actions
   #SecRuleEngine Off
   #SecRuleEngine DetectionOnly
   SecRuleEngine On
   
   # Remember to use relative path for logs:
   SecDebugLog logs/httpd_mod_security_debug.log
   
   # Start with low debug level
   SecDebugLogLevel 0
   #SecDebugLogLevel 1
   
   # Start without auditing
   SecAuditEngine Off
   #SecAuditEngine RelevantOnly
   #SecAuditEngine On
   
   # Tune audit accordingly:
   SecAuditLogRelevantStatus "^(?:5|4(?!04))"
   SecAuditLogParts ABIJDEFHZ
   SecAuditLogType Serial
   
   # Remember to use relative path for logs:
   SecAuditLog logs/httpd_mod_security_audit.log
   
   # You might still use /tmp for temporary/work files:
   SecTmpDir /tmp
   SecDataDir /tmp
   ```

1. 選取所需的 `.vhost` 來自您的AEM專案的Dispatcher模組 `dispatcher/src/conf.d/available_vhosts`例如， `wknd.vhost`，在外部新增以下專案 `<VirtualHost>` 區塊。

   ```
   # Enable the ModSecurity and OWASP CRS
   <IfModule mod_security2.c>
       Include conf.d/modsec/modsecurity.conf
   </IfModule>
   
   ...
   
   <VirtualHost *:80>
       ServerName    "publish"
       ...
   </VirtualHost>
   ```

以上全部 _modsecurity CRS_ 和 _DOS保護_ 可在AEM WKND Sites專案的 [tutorial/enable-modsecurity-crs-dos-protection](https://github.com/adobe/aem-guides-wknd/tree/tutorial/enable-modsecurity-crs-dos-protection) 分支供您檢閱。

### 驗證Dispatcher設定

使用AEMas a Cloud Service時，在部署之前 _Dispatcher設定_ 變更時，建議使用在本機驗證 `validate` 的指令碼 [AEM SDK的Dispatcher工具](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html).

```
# Go inside Dispatcher SDK 'bin' directory
$ cd <YOUR-AEM-SDK-DIR>/<DISPATCHER-SDK-DIR>/bin

# Validate the updated Dispatcher configurations
$ ./validate.sh <YOUR-AEM-PROJECT-CODE-DIR>/dispatcher/src
```

## 部署

使用Cloud Manager部署本機驗證的Dispatcher設定 [網頁層](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/configuring-production-pipelines.html?#web-tier-config) 或 [完整棧疊](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/configuring-production-pipelines.html?#full-stack-code) 管道。 您也可以使用 [快速開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html) 以縮短週轉時間。

## 驗證

為了驗證DoS保護，在此範例中，讓我們在60秒的範圍內傳送超過50個請求（25個請求臨界值乘以兩次發生次數）。 不過，這些要求應透過AEMas a Cloud Service [內建](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html) 或任何 [其他CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?#point-to-point-CDN) 正對您的網站。

實現CDN傳遞的一種技術是，新增查詢引數包含 **每個網站頁面請求的新隨機值**.

若要在短時間內觸發較大量的請求（50秒以上），Apache會 [Jmeter](https://jmeter.apache.org/) 或 [基準或標籤工具](https://httpd.apache.org/docs/2.4/programs/ab.html) 可使用。

### 使用JMeter指令碼模擬DoS攻擊

若要使用JMeter模擬DoS攻擊，請遵循下列步驟：

1. [下載Apache JMet](https://jmeter.apache.org/download_jmeter.cgi) 和 [安裝](https://jmeter.apache.org/usermanual/get-started.html#install) 它在本機
1. [執行](https://jmeter.apache.org/usermanual/get-started.html#running) 它在本機使用 `jmeter` 指令碼來自 `<JMETER-INSTALL-DIR>/bin` 目錄。
1. 開啟範例 [WKND-DoS-Attack-Simulation-Test](assets/modsecurity-crs/WKND-DoS-Attack-Simulation-Test.jmx) 使用JMX指令碼進入JMeter **開啟** 工具功能表。

   ![開啟範例WKND DoS攻擊JMX測試指令碼 — ModSecurity](assets/modsecurity-crs/open-wknd-dos-attack-jmx-test-script.png)

1. 更新 **伺服器名稱或IP** 中的欄位值 _首頁_ 和 _冒險頁面_ 符合測試AEM環境URL的HTTP要求取樣器。 檢閱範例JMeter指令碼的其他詳細資料。

   ![AEM伺服器名稱HTTP要求JMetere — 模組安全性](assets/modsecurity-crs/aem-server-name-http-request.png)

1. 按下指令碼以執行指令碼 **開始** 工具選單中的按鈕。 指令碼會對WKND網站傳送50個HTTP要求（5個使用者和10個回圈計數） _首頁_ 和 _冒險頁面_. 因此，針對非靜態檔案的總共100個要求，它符合DoS攻擊的資格，每 **DOS保護** CRS規則自訂設定。

   ![執行JMeter指令碼 — ModSecurity](assets/modsecurity-crs/execute-jmeter-script.png)

1. 此 **在表格中檢視結果** JMeter接聽程式顯示 **已失敗** 要求數目~ 53及以上的回應狀態。

   ![在資料表JMeter - ModSecurity中檢視結果的回應失敗](assets/modsecurity-crs/failed-response-jmeter.png)

1. 此 **503 HTTP回應代碼** 會針對失敗的請求傳回，您可以使用以下方式檢視詳細資料 **檢視結果樹狀結構** JMeter監聽器。

   ![503回應JMeter - ModSecurity](assets/modsecurity-crs/503-response-jmeter.png)

### 檢閱記錄

ModSecurity記錄器設定會記錄DoS攻擊事件的詳細資訊。 若要檢視詳細資訊，請遵循下列步驟：

1. 下載並開啟 `httpderror` 的記錄檔 **發佈Dispatch**.
1. 搜尋單字 `burst` 在記錄檔中，檢視 **錯誤** 行

   ```
   Tue Aug 15 15:19:40.229262 2023 [security2:error] [pid 308:tid 140200050567992] [cm-p46652-e1167810-aem-publish-85df5d9954-bzvbs] [client 192.150.10.209] ModSecurity: Warning. Operator GE matched 2 at IP:dos_burst_counter. [file "/etc/httpd/conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf"] [line "265"] [id "912170"] [msg "Potential Denial of Service (DoS) Attack from 192.150.10.209 - # of Request Bursts: 2"] [ver "OWASP_CRS/3.3.5"] [tag "application-multi"] [tag "language-multi"] [tag "platform-multi"] [tag "paranoia-level/1"] [tag "attack-dos"] [tag "OWASP_CRS"] [tag "capec/1000/210/227/469"] [hostname "publish-p46652-e1167810.adobeaemcloud.com"] [uri "/content/wknd/us/en/adventures.html"] [unique_id "ZNuXi9ft_9sa85dovgTN5gAAANI"]
   
   ...
   
   Tue Aug 15 15:19:40.515237 2023 [security2:error] [pid 309:tid 140200051428152] [cm-p46652-e1167810-aem-publish-85df5d9954-bzvbs] [client 192.150.10.209] ModSecurity: Access denied with connection close (phase 1). Operator EQ matched 0 at IP. [file "/etc/httpd/conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf"] [line "120"] [id "912120"] [msg "Denial of Service (DoS) attack identified from 192.150.10.209 (1 hits since last alert)"] [ver "OWASP_CRS/3.3.5"] [tag "application-multi"] [tag "language-multi"] [tag "platform-multi"] [tag "paranoia-level/1"] [tag "attack-dos"] [tag "OWASP_CRS"] [tag "capec/1000/210/227/469"] [hostname "publish-p46652-e1167810.adobeaemcloud.com"] [uri "/us/en.html"] [unique_id "ZNuXjAN7ZtmIYHGpDEkmmwAAAQw"]
   ```

1. 檢閱詳細資訊，例如 _使用者端IP位址_、動作、錯誤訊息和請求詳細資料。

## ModSecurity對效能的影響

啟用ModSecurity和相關規則會對效能造成影響，因此請留意哪些規則是必要、冗餘和略過的。 與您的網頁安全專家合作，共同啟用及自訂CRS規則。

### 其他規則

本教學課程只會啟用和自訂 **DOS保護** CRS規則用於示範用途。 建議您與網頁安全性專家合作，以瞭解、檢閱及設定適當的規則。
