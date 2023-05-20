---
title: 開發人員控制台
description: AEMas a Cloud Service為每個環境提供開發人員控制台，該控制台顯示運行服務的各AEM種詳細資訊，有助於調試。
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5433
thumbnail: kt-5433.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 0499ff9f-d452-459f-b1a2-2853a228efd1
source-git-commit: 8ca9535866cc1a673a59ac3743847e68dfedd156
workflow-type: tm+mt
source-wordcount: '1471'
ht-degree: 0%

---

# 使用開AEM發人員控制台調試as a Cloud Service

AEMas a Cloud Service為每個環境提供開發人員控制台，該控制台顯示運行服務的各AEM種詳細資訊，有助於調試。

每AEM個as a Cloud Service環境都有自己的開發人員控制台。

## 導航到開發人員控制台

通過雲管理器按AEMas a Cloud Service環境訪問開發人員控制台。

![導航到開發人員控制台](./assets/developer-console/navigate.png)

1. 導航到 __[雲管理器](https://my.cloudmanager.adobe.com/)__
2. 開啟 __計畫__ 包含用於打AEM開開發人員控制台的as a Cloud Service環境。
3. 查找 __環境__，然後選擇 `...`。
4. 選擇 __開發人員控制台__ 清單中。


## 開發人員控制台訪問

要訪問和使用開發人員控制台，必須通過以下方式將以下權限授予開發人員的Adobe ID [AdobeAdmin Console](https://adminconsole.adobe.com)。

1. 確保在Adobe組織切換器中激活了影響雲管理AEM器和as a Cloud Service產品的Adobe組織。
1. 開發人員必須是 [Cloud Manager產品 __開發人員 — Cloud Service__ 產品配置檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-cloud-manager.html#assign-developer)。
   + 如果此成員身份不存在，開發人員將無法登錄到開發人員控制台。
1. 開發人員必須是 [__用AEM戶__ 或 __管AEM理員__ AEM作者和/或發佈上的產品配置檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html#aem-product-profiles)。
   + 如果此成員身份不存在， [狀態](#status) 轉儲將超時，出現「401 Unauthed（未經授權）」錯誤。

### 排除開發人員控制台訪問故障

#### 401轉儲狀態時出現未授權錯誤

![開發人員控制台 — 401未授權](./assets/developer-console/troubleshooting__401-unauthorized.png)

如果報告了轉儲任何狀態a 401未授權錯誤，則表示您的用戶尚未在as a Cloud Service中具有必要的權限，或AEM登錄令牌的使用無效或已過期。

要解決401未授權問題：

1. 確保您的用戶是開發人員控制台關聯的as a Cloud Service產品實例的相AEM應Adobe IMS產AEM品配置檔案（管理員或用戶）AEM的成員。
   + 請記住，開發人員控制台訪問2個Adobe IMS產品實例；「AEMas a Cloud Service作者」和「發佈產品實例」，因此，根據需要通過開發人員控制台訪問的服務層，確保使用正確的產品配置檔案。
1. 登錄到AEMas a Cloud Service（作者或發佈），並確保用戶和組已正確同步到AEM。
   + 開發人員控制台要求在相應服務層中創AEM建用戶記錄，以便它驗證到該服務層。
1. 清除瀏覽器cookie以及應用程式狀態（本地儲存），並重新登錄到Developer Console，確保Developer Console使用的訪問令牌正確且未到期。

## Pod

AEMas a Cloud Service作者和發佈服務分別由多個實例組成，以處理流量變異性和滾動更新，而無停機。 這些實例稱為Pod。 Developer Console中的Pod選擇定義了通過其他控制項公開的資料範圍。

![開發人員控制台 — Pod](./assets/developer-console/pod.png)

+ Pod是服務（作者或發佈）的一AEM部分的離散實例
+ 豆莢是瞬間的，AEM意味著as a Cloud Service會根據需要創造和銷毀它們
+ 僅列出屬於關聯as a Cloud Service環AEM境一部分的Pod，即該環境的Developer Console的Pod轉換器。
+ 在Pod交換機底部，便利選項允許按服務類型選擇Pod:
   + 所有作者
   + 所有發佈者
   + 所有實例

## 狀態

狀態提供了以文本或AEMJSON輸出輸出特定運行時狀態的選項。 開發人員控制台提供與AEMSDK的本地快速啟動OSGi Web控制台類似的資訊，其顯著區別是開發人員控制台是只讀的。

![開發人員控制台 — 狀態](./assets/developer-console/status.png)

### 捆綁

捆綁包列出中的所有OSGi捆AEM綁包。 此功能與 [AEMSDK的本地快速啟動OSGi捆綁包](http://localhost:4502/system/console/bundles) 在 `/system/console/bundles`。

捆綁調試幫助，方法如下：

+ 列出部署到「服務」的AEM所有OSGi捆綁包
+ 列出每個OSGi捆綁包的狀態；包括活動或不活動
+ 提供未解決的依賴項的詳細資訊，使OSGi捆綁包不會變為活動

### 元件

元件列出中的所有OSGi組AEM件。 此功能與 [AEMSDK的本地快速啟動的OSGi元件](http://localhost:4502/system/console/components) 在 `/system/console/components`。

元件在調試中的幫助，方法是：

+ 列出部署到AEMas a Cloud Service的
+ 提供每個OSGi元件的狀態；包括活動或不滿足
+ 向未滿足的服務引用提供詳細資訊可能會導致OSGi元件變為活動
+ 列出綁定到OSGi元件的OSGi屬性及其值。
   + 這將顯示通過 [OSGi環境配置變數](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values)。

### 設定

配置列出所有OSGi元件的配置（OSGi屬性和值）。 此功能與 [AEMSDK的本地快速啟動的OSGi Configuration Manager](http://localhost:4502/system/console/configMgr) 在 `/system/console/configMgr`。

配置通過以下方式幫助調試：

+ 用OSGi元件列出OSGi屬性及其值
   + 這不會顯示通過 [OSGi環境配置變數](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values)。 請參閱 [元件](#components) 上面，表示注入值。
+ 查找和標識配置錯誤的屬性

### 橡樹索引

Oak索引提供在下定義的節點的轉儲 `/oak:index`。 請記住，這不會顯示合併索引，而合併索引在修改AEM索引時發生。

Oak索引通過以下方式幫助調試：

+ 列出所有Oak索引定義，提供有關搜索查詢在中執行方式的AEM見解。 請記住，修改為索AEM引的內容不會在此反映。 此視圖僅對僅由自定義代碼提供或僅由自AEM定義代碼提供的索引有幫助。

### OSGi服務

元件列出所有OSGi服務。 此功能與 [SDKAEM的本地快速入門OSGi服務](http://localhost:4502/system/console/services) 在 `/system/console/services`。

OSGi Services通過以下方式幫助調試：

+ 列出中的所有OSGi服AEM務，以及其提供的OSGi捆綁包，以及使用它的所有OSGi捆綁包

### Sling 工作

Sling Jobs列出所有Sling Jobs隊列。 此功能與 [AEMSDK的本地快速入門作業](http://localhost:4502/system/console/slingevent) 在 `/system/console/slingevent`。

Sling Jobs通過以下方式幫助調試：

+ Sling作業隊列及其配置清單
+ 提供對活動、排隊和處理的Sling作業數量的洞見，這有助於調試工作流、臨時工作流和中Sling作業執行的其他工作的問AEM題。

## Java包

Java包允許檢查Java包和版本是否可用於AEMas a Cloud Service。 此功能與 [AEMSDK的本地快速啟動依賴關係查找器](http://localhost:4502/system/console/depfinder) 在 `/system/console/depfinder`。

![開發人員控制台 — Java包](./assets/developer-console/java-packages.png)

Java包用於難於拍攝由於未解析的導入或指令碼（HTL、JSP等）中未解析的類而未啟動的包。 如果Java包報告沒有綁定導出Java包（或版本與OSGi綁定導入的版本不匹配）:

+ 確保項目的AEMAPI主依賴項版本與環境的發行版AEM本匹配（如果可能，將所有內容更新為最新版本）。
+ 如果在Maven項目中使用了額外的Maven依賴項
   + 確定是否可以改用SDK API依賴AEM項提供的替代API。
   + 如果需要額外的依賴關係，請確保它作為OSGi捆綁包（而不是純Jar）提供，並且它嵌入到項目的代碼包中(`ui.apps`)，與核心OSGi捆綁包嵌入到 `ui.apps` 檔案。

## Servlet

Servlet用於提供如何解析Java Servlet或AEM指令碼(HTL、JSP)的URL的洞見，這些URL最終可處理請求。 此功能與 [AEMSDK的本地快速啟動的Sling Servlet解析器](http://localhost:4502/system/console/servletresolver) 在 `/system/console/servletresolver`。

![開發人員控制台 — Servlet](./assets/developer-console/servlets.png)

Servlet有助於調試確定：

+ 如何將URL分解為其可定址部分（資源、選擇器、擴展）。
+ URL解析到的Servlet或指令碼，有助於識別格式錯誤的URL或註冊錯誤的Servlet/指令碼。

## 查詢

查詢有助於深入瞭解在上執行搜索查詢的方式和AEM方式。 此功能與  [AEMSDK的本地快速入門工具>查詢效能 ](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) 控制台。

僅當選擇特定的Pod時，查詢才起作用，因為它會開啟該Pod的Query Performance Web控制台，要求開發人員具有登錄服務的AEM權限。

![開發人員控制台 — 查詢 — 解釋查詢](./assets/developer-console/queries__explain-query.png)

查詢通過以下方式幫助調試：

+ 解釋Oak如何解釋、分析和執行查詢。 在跟蹤查詢速度慢的原因以及瞭解如何加快查詢速度時，這一點非常重要。
+ 列出運行在中的最AEM常用查詢，並能夠解釋它們。
+ 列出中運行的最慢AEM查詢，並能夠解釋它們。
