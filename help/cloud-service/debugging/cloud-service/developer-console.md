---
title: 開發人員主控台
description: AEM as a Cloud Service為每個環境提供一個Developer Console，它會公開執行中有助於偵錯的AEM服務的各種詳細資訊。
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5433
thumbnail: kt-5433.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 0499ff9f-d452-459f-b1a2-2853a228efd1
duration: 295
source-git-commit: 1d9aeb4e5bd41096a28e3375d124bd6b6b8784aa
workflow-type: tm+mt
source-wordcount: '1562'
ht-degree: 0%

---

# 使用Developer Console除錯AEM as a Cloud Service

AEM as a Cloud Service為每個環境提供一個Developer Console，它會公開執行中有助於偵錯的AEM服務的各種詳細資訊。

每個AEM as a Cloud Service環境都有自己的Developer Console。

## 導覽至Developer Console

透過Cloud Manager在每個AEM as a Cloud Service環境中存取Developer Console。

![導覽至Developer Console](./assets/developer-console/navigate.png)

1. 導覽至&#x200B;__[Cloud Manager](https://my.cloudmanager.adobe.com/)__
2. 開啟包含AEM as a Cloud Service環境的&#x200B;__方案__&#x200B;以開啟Developer Console。
3. 找到&#x200B;__環境__，然後選取`...`。
4. 從下拉式清單中選取&#x200B;__Developer Console__。


## Developer Console存取權

若要存取及使用Developer Console，必須透過[Adobe的Admin Console](https://adminconsole.adobe.com)將下列許可權授予開發人員的Adobe ID。

1. 確保Adobe組織切換器中的，您可以看到與您要在Developer Console中檢查的環境相關的Adobe組織。
1. 為了能夠登入Developer Console，開發人員必須是以下任何角色的成員：
   + [Cloud Manager產品的&#x200B;__開發人員 — Cloud Service__&#x200B;產品設定檔](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-cloud-manager.html#assign-developer)：在此情況下，開發人員將看到所選Developer Console URL下可用的完整環境清單；如果已在Cloud Manager中選取開發環境或RDE，則可能會顯示相同程式中的其他開發環境或RDE。
   + __AEM作者__](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html#aem-product-profiles)上的&#x200B;[__AEM管理員__&#x200B;產品設定檔：在此情況下，上一個專案符號中說明的環境清單將限於指派此角色的相關產品設定檔。
1. 開發人員必須是AEM作者和/或Publish](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html#aem-product-profiles)上的&#x200B;[__AEM使用者__&#x200B;或&#x200B;__AEM管理員__&#x200B;產品設定檔的成員。
   + 如果此成員資格不存在，[狀態](#status)傾印將逾時，並出現401未授權錯誤。

### 疑難排解Developer Console存取問題

#### 當我登入時，我沒有看到我正在尋找的環境清單

請確認下列事項：

+ 您已透過Cloud Manager針對所選環境按一下三個點，然後選取Developer Console ，以選取正確的Developer Console URL。
+ 您擁有[Cloud Manager產品的&#x200B;__開發人員 — Cloud Service__&#x200B;產品設定檔](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-cloud-manager.html#assign-developer)以檢視完整的環境清單，或者您是找不到環境之&#x200B;__AEM作者__](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html#aem-product-profiles)上的&#x200B;[__AEM管理員__&#x200B;產品設定檔的一部分。

#### 401傾印狀態時發生未獲授權的錯誤

![Developer Console - 401未獲授權](./assets/developer-console/troubleshooting__401-unauthorized.png)

如果回報任何傾印狀態「401未授權錯誤」，表示您的使用者尚未存在，且AEM as a Cloud Service中有必要許可權，或是登入權杖使用的無效或已過期。

若要解決401 Unauthorized問題：

1. 確認您的使用者是Developer Console相關AEM as a Cloud Service產品執行個體的適當Adobe IMS產品設定檔成員(AEM管理員或AEM使用者)。
   + 請記住，Developer Console存取2個Adobe IMS產品執行個體；AEM as a Cloud Service作者和Publish產品執行個體，因此請確保根據需要透過Developer Console存取的服務層級使用正確的產品設定檔。
1. 登入AEM as a Cloud Service (Author或Publish)，並確保您的使用者和群組已正確同步至AEM。
   + Developer Console需要在對應的AEM服務層級中建立您的使用者記錄，以便對該服務層級進行驗證。
1. 清除您的瀏覽器Cookie以及應用程式狀態（本機儲存）並重新登入Developer Console，確保Developer Console使用的存取權杖正確且未過期。

## Pod

AEM as a Cloud Service Author和Publish服務分別由多個例項組成，以處理流量變數及滾動更新，避免停機時間。 這些例項稱為Pod。 Developer Console中的Pod選取範圍定義將透過其他控制項公開的資料範圍。

![Developer Console - Pod](./assets/developer-console/pod.png)

+ Pod為獨立例項，屬於AEM服務(Author或Publish)的一部分
+ Pod是暫時性的，表示AEM as a Cloud Service會視需要建立和銷毀
+ 只有屬於相關AEM as a Cloud Service環境的Pod會列出該環境的Developer Console Pod切換器。
+ 在Pod切換器底部，方便選項可依服務型別選取Pod：
   + 所有作者
   + 所有發行者
   + 所有執行個體

## 狀態

狀態提供以文字或JSON輸出輸出特定AEM執行階段狀態的選項。 Developer Console提供與AEM SDK的本機Quickstart的OSGi Web主控台類似的資訊，明顯差異為Developer Console為唯讀。

![Developer Console — 狀態](./assets/developer-console/status.png)

### 組合

套件組合列出AEM中的所有OSGi套件組合。 此功能類似於`/system/console/bundles`的[AEM SDK本機Quickstart的OSGi組合](http://localhost:4502/system/console/bundles)。

套件組合可協助您進行除錯，方法如下：

+ 列出部署至AEM as a Service的所有OSGi套件組合
+ 列出每個OSGi套件組合的狀態；包括它們是否作用中
+ 針對導致OSGi套件組合無法作用中的未解析相依性提供詳細資訊

### 元件

「元件」會列出AEM中的所有OSGi元件。 此功能類似於[AEM SDK本機Quickstart的OSGi元件](http://localhost:4502/system/console/components) （位於`/system/console/components`）。

元件可協助您透過下列方式執行偵錯：

+ 列出部署至AEM as a Cloud Service的所有OSGi元件
+ 提供每個OSGi元件的狀態；包括元件是作用中還是不滿意
+ 將詳細資訊提供給不滿意的服務參考可能會導致OSGi元件變成作用中
+ 列出繫結至OSGi元件的OSGi屬性及其值。
   + 這會顯示透過[OSGi環境設定變數](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values)插入的實際值。

### 設定

設定會列出所有OSGi元件的設定（OSGi屬性和值）。 此功能類似於[AEM SDK本機Quickstart的OSGi Configuration Manager](http://localhost:4502/system/console/configMgr) （位於`/system/console/configMgr`）。

設定可協助您透過以下方式偵錯：

+ 依OSGi元件列出OSGi屬性及其值
   + 這不會顯示透過[OSGi環境設定變數](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values)插入的實際值。 請參閱上面的[元件](#components)，以取得插入的值。
+ 尋找和識別設定錯誤的屬性

### Oak索引

Oak索引提供`/oak:index`下定義的節點傾印。 請記住，這不會顯示合併的索引，這在修改AEM索引時發生。

Oak索引可協助您透過以下方式偵錯：

+ 列出所有Oak索引定義，深入分析搜尋查詢在AEM中的執行方式。 請記住，修改成AEM索引的內容不會反映在這裡。 此檢視僅對由AEM單獨提供，或僅由自訂程式碼提供的索引有幫助。

### OSGi服務

元件會列出所有OSGi服務。 此功能類似於[AEM SDK在`/system/console/services`的本機Quickstart的OSGi服務](http://localhost:4502/system/console/services)。

OSGi Services偵錯說明，依據：

+ 列出AEM中的所有OSGi服務，以及其提供的OSGi套件組合，以及使用該套件的所有OSGi套件組合

### Sling 工作

Sling作業會列出所有Sling作業佇列。 此功能類似於[AEM SDK在`/system/console/slingevent`的本機Quickstart工作](http://localhost:4502/system/console/slingevent)。

Sling Jobs可協助您依照以下方式偵錯：

+ Sling工作佇列及其設定的清單
+ 提供對作用中、已佇列和已處理Sling工作數量的深入分析，這有助於偵錯工作流程、暫時性工作流程和AEM中Sling工作執行的其他工作的問題。

## Java套件

Java套件可讓您檢查Java套件和版本是否可用於AEM as a Cloud Service。 此功能與[AEM SDK在`/system/console/depfinder`的本機Quickstart相依性尋找器](http://localhost:4502/system/console/depfinder)相同。

![Developer Console - Java套件](./assets/developer-console/java-packages.png)

Java Packages用於疑難排解由於未解析的匯入或指令碼（HTL、JSP等）中的未解析類別而未啟動的套件組合。 如果Java套件報告沒有套件組合匯出Java套件（或版本不符合OSGi套件組合匯入的版本）：

+ 確保您的專案的AEM API Maven相依性的版本與環境的AEM版本相符（並在可能的情況下將所有內容更新到最新版本）。
+ 如果在Maven專案中使用額外的Maven相依性
   + 決定是否可改用AEM SDK API相依性提供的替代API。
   + 如果需要額外的相依性，請確保提供它作為OSGi套件（而不是純Jar），並且它內嵌在您的專案的程式碼套件(`ui.apps`)中，類似於核心OSGi套件內嵌在`ui.apps`套件中的方式。

## Servlet

Servlet可用來提供AEM如何將URL解析為最終處理請求的Java servlet或指令碼(HTL、JSP)的分析。 此功能與[AEM SDK在`/system/console/servletresolver`的本機Quickstart的Sling Servlet Resolver](http://localhost:4502/system/console/servletresolver)相同。

![Developer Console - Servlet](./assets/developer-console/servlets.png)

Servlet有助於偵錯判斷：

+ URL如何分解成其可定址部分（資源、選擇器、副檔名）。
+ URL解析到的servlet或指令碼，有助於識別格式錯誤的URL或註冊錯誤的servlet/指令碼。

## 查詢

查詢有助於提供在AEM上執行搜尋查詢的內容和方式的深入分析。 此功能與[AEM SDK的本機Quickstart的「工具>查詢效能](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html)主控台」相同。

只有在選取特定pod時，查詢才能運作，因為它會開啟該pod的查詢效能Web主控台，要求開發人員具有登入AEM服務的存取權。

![Developer Console — 查詢 — 說明查詢](./assets/developer-console/queries__explain-query.png)

查詢可透過以下方式協助偵錯：

+ 說明Oak如何解譯、分析和執行查詢。 在追蹤查詢緩慢的原因以及瞭解如何加快查詢速度時，這一點非常重要。
+ 列出在AEM中執行的最受歡迎查詢，並可加以說明。
+ 列出AEM中執行速度最慢的查詢，並可加以說明。
