---
title: 使用 ModSecurity 協助保護您的 AEM 網站抵禦 DoS 攻擊
description: 了解如何使用 OWASP ModSecurity 核心規則集 (CRS) 啟用 ModSecurity，協助保護您的網站抵禦阻斷服務 (DoS) 攻擊。
feature: Security
version: Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Security, Development
role: Admin, Architect
level: Experienced
jira: KT-10385
thumbnail: KT-10385.png
doc-type: Article
last-substantial-update: 2023-08-18T00:00:00Z
exl-id: 9f689bd9-c846-4c3f-ae88-20454112cf9a
duration: 783
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1171'
ht-degree: 100%

---

# 使用 ModSecurity 協助保護您的 AEM 網站抵禦 DoS 攻擊

了解如何使用 Adobe Experience Manager (AEM) Publish Dispatcher 上的 **OWASP ModSecurity 核心規則集 (CRS)** 啟用 ModSecurity，協助保護您的網站抵禦阻斷服務 (DoS) 攻擊。


>[!VIDEO](https://video.tv.adobe.com/v/3422976?quality=12&learn=on)

## 概觀

[Open Web Application Security Project® (OWASP)](https://owasp.org/) 基金會提供 [**OWASP 十大安全漏洞**](https://owasp.org/www-project-top-ten/)，概述網頁應用程式的十項最重大的安全性問題。

ModSecurity 是一項開放原始碼的跨平台解決方案，可抵禦針對網頁應用程式發動的多種攻擊。也可以進行 HTTP 流量監視、記錄和即時分析。

OWSAP® 也提供 [OWASP® ModSecurity 核心規則集 (CRS)](https://github.com/coreruleset/coreruleset)。CRS 是一套與 ModSecurity 搭配使用的通用&#x200B;**攻擊偵測**&#x200B;規則。因此，CRS 希望以最少的錯誤警報，保護網頁應用程式抵禦各種攻擊，包括 OWASP 十大安全漏洞。

本教學課程會示範如何啟用和設定 **DOS-PROTECTION** CRS 規則，保護您的網站抵禦潛在的 DoS 攻擊。

>[!TIP]
>
>值得注意的是，AEM as a Cloud Service 的[託管 CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html) 符合大多數客戶對於效能和安全性的要求。但是，ModSecurity 會提供額外的安全防護層，並允許客戶特定的規則和設定。

## 在 Dispatcher 專案模組中新增 CRS

1. 下載並解壓縮[最新的 OWASP ModSecurity 核心規則集](https://github.com/coreruleset/coreruleset/releases)。

   ```shell
   # Replace the X.Y.Z with relevent version numbers.
   $ wget https://github.com/coreruleset/coreruleset/archive/refs/tags/vX.Y.Z.tar.gz
   
   # For version v3.3.5 when this tutorial is published
   $ wget https://github.com/coreruleset/coreruleset/archive/refs/tags/v3.3.5.tar.gz
   
   # Extract the downloaded file
   $ tar -xvzf coreruleset-3.3.5.tar.gz
   ```

1. 在您 AEM 專案程式碼的 `dispatcher/src/conf.d/` 內，建立 `modsec/crs` 資料夾。例如，在 [AEM WKND 網站專案](https://github.com/adobe/aem-guides-wknd)的本機副本中。

   ![AEM 專案程式碼內的 CRS 資料夾 - ModSecurity](assets/modsecurity-crs/crs-folder-in-aem-dispatcher-module.png){width="200" zoomable="yes"}

1. 從已下載的 CRS 發行套件中將 `coreruleset-X.Y.Z/rules` 資料夾複製到 `dispatcher/src/conf.d/modsec/crs` 資料夾。
1. 從已下載的 CRS 發行套件中將 `coreruleset-X.Y.Z/crs-setup.conf.example` 檔案複製到 `dispatcher/src/conf.d/modsec/crs` 資料夾，並重新命名為 `crs-setup.conf`。
1. 把所有從 `dispatcher/src/conf.d/modsec/crs/rules` 複製而來的 CRS 規則重新命名為 `XXXX-XXX-XXX.conf.disabled`，將其停用。您可以使用下方的命令，一次重新命名所有檔案。

   ```shell
   # Go inside the newly created rules directory within the dispathcher module
   $ cd dispatcher/src/conf.d/modsec/crs/rules
   
   # Rename all '.conf' extension files to '.conf.disabled'
   $ for i in *.conf; do mv -- "$i" "$i.disabled"; done
   ```

   請參閱 WKND 專案程式碼中重新命名的 CRS 規則和設定檔案。

   ![在 AEM 專案程式碼內已停用的 CRS 規則 - ModSecurity ](assets/modsecurity-crs/disabled-crs-rules.png){width="200" zoomable="yes"}

## 啟用及設定阻斷服務 (DoS) 防護規則

若要啟用及設定阻斷服務 (DoS) 防護規則，請依照下列步驟進行：

1. 將 `dispatcher/src/conf.d/modsec/crs/rules` 資料夾內的 `REQUEST-912-DOS-PROTECTION.conf.disabled` 重新命名為 `REQUEST-912-DOS-PROTECTION.conf` (或從規則名稱副檔名移除 `.disabled`)，即可啟用 DoS 防護規則。
1. 透過定義 **DOS_COUNTER_THRESHOLD, DOS_BURST_TIME_SLICE, DOS_BLOCK_TIMEOUT** 變數來設定規則。
   1. 在 `dispatcher/src/conf.d/modsec/crs` 資料夾內建立一個 `crs-setup.custom.conf` 檔案。
   1. 在新建立的檔案中新增下方的規則片段。

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

在此規則設定範例中，**DOS_COUNTER_THRESHOLD** 為 25，**DOS_BURST_TIME_SLICE** 為 60 秒，且 **DOS_BLOCK_TIMEOUT** 逾時的時間限制為 600 秒。此設定在 60 秒內發現有 25 個要求 (不包括靜態檔案) 出現兩次以上，符合 DoS 攻擊的條件，導致發出要求的用戶端被封鎖 600 秒 (或 10 分鐘)。

>[!WARNING]
>
>若要定義符合您需求的數值，請與您的網頁安全性團隊合作。

## 將 CRS 初始化

若要將 CRS 初始化、移除常見的誤報項目，並對您的網站新增本機例外情況。請依照以下步驟進行：

1. 若要將 CRS 初始化，請從 **REQUEST-901-INITIALIZATION** 檔案中移除 `.disabled`。換句話說，將 `REQUEST-901-INITIALIZATION.conf.disabled` 檔案重新命名為 `REQUEST-901-INITIALIZATION.conf`。
1. 若要移除常見的誤報項目，例如本機 IP (127.0.0.1) ping，請從 **REQUEST-905-COMMON-EXCEPTIONS** 檔案中移除 `.disabled`。
1. 若要新增本機例外情況 (如 AEM 平台或網站特定路徑)，請將 `REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf.example` 重新命名為 `REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf`。
   1. 將 AEM 平台特定的路徑例外情況，新增至最近重新命名的檔案中。

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

1. 此外，針對 IP 信譽封鎖檢查，從 **REQUEST-910-IP-REPUTATION.conf.disabled** 中移除 `.disabled`，而對於異常情況分數檢查，則是從 `REQUEST-949-BLOCKING-EVALUATION.conf.disabled` 移除。

>[!TIP]
>
>在 AEM 6.5 上進行設定時，請確保將上述路徑替換為對應的 AMS，或能檢查 AEM 健康狀況的內部部署路徑 (又稱心跳路徑)。

## 新增 ModSecurity Apache 設定

若要啟用 ModSecurity (又名 `mod_security` Apache 模組)，請依照以下步驟進行：

1. 使用以下主要設定在 `dispatcher/src/conf.d/modsec/modsecurity.conf` 建立 `modsecurity.conf`。

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

1. 從您的 AEM 專案的 Dispatcher 模組 `dispatcher/src/conf.d/available_vhosts` 中選取所需的 `.vhost`，例如 `wknd.vhost`，在 `<VirtualHost>` 區塊以外新增以下項目。

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

以上所有 _ModSecurity CRS_ 和 _DOS-PROTECTION_ 設定皆可在 AEM WKND 網站專案的 [tutorial/enable-modsecurity-crs-dos-protection](https://github.com/adobe/aem-guides-wknd/tree/tutorial/enable-modsecurity-crs-dos-protection) 分支上找到，方便您檢閱。

### 驗證 Dispatcher 設定

使用 AEM as a Cloud Service 時，在部署 _Dispatcher 設定_&#x200B;變更之前，建議先使用 [AEM SDK Dispatcher 工具](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html)的 `validate` 指令碼在本機上驗證這些變更。

```
# Go inside Dispatcher SDK 'bin' directory
$ cd <YOUR-AEM-SDK-DIR>/<DISPATCHER-SDK-DIR>/bin

# Validate the updated Dispatcher configurations
$ ./validate.sh <YOUR-AEM-PROJECT-CODE-DIR>/dispatcher/src
```

## 部署

使用 Cloud Manager [網頁層](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/configuring-production-pipelines.html?#web-tier-config)或[完整堆疊](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/configuring-production-pipelines.html?#full-stack-code)管道，部署經本機驗證的 Dispatcher 設定。您也可以使用[快速開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html)來縮短完成時間。

## 驗證

若要驗證 DoS 防護，在此範例中，請於 60 秒內發送超過 50 個要求 (臨界值是 25 個要求，乘以發生兩次)。然而，這些要求應該能夠傳遞通過[內建的](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html) AEM as a Cloud Service 或任何位在您網站前端的[其他 CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?#point-to-point-CDN)。

能達到 CDN 傳遞的其中一種技術，是新增一個&#x200B;**附帶各網站頁面要求之新隨機值**&#x200B;的查詢參數。

若要在短時間內 (例如 60 秒) 觸發大量要求 (50 個以上)，可以使用 Apache [JMeter](https://jmeter.apache.org/) 或 [Benchmark 或 ab 工具](https://httpd.apache.org/docs/2.4/programs/ab.html)。

### 使用 JMeter 指令碼模擬 DoS 攻擊

若要使用 JMeter 模擬 DoS 攻擊，請依照下列步驟進行：

1. [下載 Apache JMeter](https://jmeter.apache.org/download_jmeter.cgi) 並[安裝](https://jmeter.apache.org/usermanual/get-started.html#install)於本機
1. 使用 `<JMETER-INSTALL-DIR>/bin` 目錄中的 `jmeter` 指令碼並在本地[執行](https://jmeter.apache.org/usermanual/get-started.html#running)。
1. 使用「**開啟**」工具選單，在 JMeter 中開啟 [WKND-DoS-Attack-Simulation-Test](assets/modsecurity-crs/WKND-DoS-Attack-Simulation-Test.jmx) JMX 指令碼範例。

   ![開啟 WKND DoS 攻擊 JMX 測試指令碼範例 - ModSecurity](assets/modsecurity-crs/open-wknd-dos-attack-jmx-test-script.png)

1. 更新「_首頁_」和「_探險頁面_」HTTP 要求取樣器中的「**伺服器名稱或 IP**」欄位值，使其符合您的測試 AEM 環境 URL。檢閱 JMeter 指令碼範例的其他詳細資訊。

   ![AEM 伺服器名稱 HTTP 要求 JMetere - ModSecurity](assets/modsecurity-crs/aem-server-name-http-request.png)

1. 按下工具選單中的「**開始**」按鈕，執行指令碼。指令碼會向 WKND 網站的「_首頁_」和「_探險頁面_」發送 50 個 HTTP 要求 (5 個使用者和 10 個循環計數)。因此，總共會有 100 個對非靜態檔案的要求，依據 **DOS-PROTECTION** CRS 規則自訂設定，其符合 DoS 攻擊的條件。

   ![執行 JMeter 指令碼 - ModSecurity](assets/modsecurity-crs/execute-jmeter-script.png)

1. **以表格形式檢視結果** JMeter 監聽程式顯示編號 53 及之後的要求回應狀態皆是「**失敗**」。

   ![JMeter 以表格形式檢視結果中的失敗回應 - ModSecurity](assets/modsecurity-crs/failed-response-jmeter.png)

1. 失敗的要求會傳回 **503 HTTP 回應代碼**，您可以使用&#x200B;**檢視結果樹** JMeter 監聽程式檢視詳細資訊。

   ![503 回應 JMeter - ModSecurity](assets/modsecurity-crs/503-response-jmeter.png)

### 檢閱記錄

ModSecurity 記錄程式設定會記錄 DoS 攻擊事件的詳細資訊。若要檢視詳細資訊，請依照以下步驟進行：

1. 下載並開啟&#x200B;**發佈 Dispatcher** 的 `httpderror` 記錄檔案。
1. 在記錄檔案中搜尋單詞 `burst`，查看&#x200B;**錯誤**&#x200B;文字行

   ```
   Tue Aug 15 15:19:40.229262 2023 [security2:error] [pid 308:tid 140200050567992] [cm-p46652-e1167810-aem-publish-85df5d9954-bzvbs] [client 192.150.10.209] ModSecurity: Warning. Operator GE matched 2 at IP:dos_burst_counter. [file "/etc/httpd/conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf"] [line "265"] [id "912170"] [msg "Potential Denial of Service (DoS) Attack from 192.150.10.209 - # of Request Bursts: 2"] [ver "OWASP_CRS/3.3.5"] [tag "application-multi"] [tag "language-multi"] [tag "platform-multi"] [tag "paranoia-level/1"] [tag "attack-dos"] [tag "OWASP_CRS"] [tag "capec/1000/210/227/469"] [hostname "publish-p46652-e1167810.adobeaemcloud.com"] [uri "/content/wknd/us/en/adventures.html"] [unique_id "ZNuXi9ft_9sa85dovgTN5gAAANI"]
   
   ...
   
   Tue Aug 15 15:19:40.515237 2023 [security2:error] [pid 309:tid 140200051428152] [cm-p46652-e1167810-aem-publish-85df5d9954-bzvbs] [client 192.150.10.209] ModSecurity: Access denied with connection close (phase 1). Operator EQ matched 0 at IP. [file "/etc/httpd/conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf"] [line "120"] [id "912120"] [msg "Denial of Service (DoS) attack identified from 192.150.10.209 (1 hits since last alert)"] [ver "OWASP_CRS/3.3.5"] [tag "application-multi"] [tag "language-multi"] [tag "platform-multi"] [tag "paranoia-level/1"] [tag "attack-dos"] [tag "OWASP_CRS"] [tag "capec/1000/210/227/469"] [hostname "publish-p46652-e1167810.adobeaemcloud.com"] [uri "/us/en.html"] [unique_id "ZNuXjAN7ZtmIYHGpDEkmmwAAAQw"]
   ```

1. 檢閱詳細資訊，像是&#x200B;_用戶端 IP 位址_、動作、錯誤訊息和要求詳細資訊。

## ModSecurity 的效能影響

啟用 ModSecurity 和相關聯的規則會對效能產生一些影響，因此請留意哪些規則是必要的、多餘的和可以略過的。與您的網頁安全性專家合作，啟用並自訂 CRS 規則。

### 其他規則

本教學課程僅為示範目的而啟用和自訂 **DOS-PROTECTION** CRS 規則。建議與網頁安全性專家合作，了解、檢閱和設定適當的規則。
