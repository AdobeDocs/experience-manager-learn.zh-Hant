---
title: 開發人員主控台
description: AEM as a Cloud Service針對每個環境提供開發人員主控台，可公開執行中AEM服務的各種詳細資訊，有助於除錯。
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
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '1348'
ht-degree: 0%

---

# 使用開發人員主控台除錯AEM作為Cloud Service

AEM as a Cloud Service針對每個環境提供開發人員主控台，可公開執行中AEM服務的各種詳細資訊，有助於除錯。

每個AEM作為Cloud Service環境都有專屬的開發人員主控台。

## 開發人員控制台存取

若要存取及使用開發人員主控台，必須透過[Adobe的Admin Console](https://adminconsole.adobe.com)，為開發人員的Adobe ID授予下列權限。

1. 確認已影響Cloud Manger和AEM作為Cloud Service產品的Adobe組織在Adobe組織切換器中處於作用中狀態。
1. 開發人員必須是Cloud Manager產品&#x200B;__開發人員 — Cloud Service__&#x200B;產品設定檔的成員。
   + 如果此會籍不存在，開發人員將無法登入開發人員控制台。
1. 開發人員必須是&#x200B;__AEM Users__&#x200B;或&#x200B;__AEM Administrators__ Product Profile on AEM Author and/or Publish的成員。
   + 如果此成員資格不存在， [status](#status)轉儲將超時，並出現401 Unauthorized錯誤。

### 疑難排解開發人員控制台存取

#### 401傾銷狀態時出現未授權錯誤

![開發人員控制台 — 401未授權](./assets/developer-console/troubleshooting__401-unauthorized.png)

如果系統回報401未授權錯誤，表示您的使用者尚未存在AEM中具有Cloud Service所需權限，或登入代號的使用無效或已過期。

要解決401未授權問題：

1. 確認您的使用者是Developer Console相關聯的AEM(Adobe產品例項)之適當Cloud ServiceIMS產品設定檔(AEM管理員或AEM使用者)的成員。
   + 請記住，開發人員控制台可存取2個AdobeIMS產品例項；AEM作為Cloud Service製作和發佈產品例項，因此請根據需要透過開發人員控制台存取的服務層級，確定使用正確的產品設定檔。
1. 以Cloud Service身分登入AEM（製作或發佈），並確認您的使用者和群組已正確同步至AEM。
   + 開發人員控制台需要在對應的AEM服務層中建立您的使用者記錄，以便對該服務層進行驗證。
1. 清除瀏覽器Cookie以及應用程式狀態（本機儲存）並重新登入開發人員控制台，確保開發人員控制台所使用的存取權杖正確且未過期。

## Pod

AEM as a Cloud Service製作和發佈服務分別由多個執行個體組成，以處理流量可變性和滾動式更新，不會造成停機。 這些例項稱為Pod。 「開發人員控制台」中的Pod選取項目會定義透過其他控制項公開的資料範圍。

![開發人員主控台 — Pod](./assets/developer-console/pod.png)

+ Pod是屬於AEM服務（製作或發佈）一部分的獨立例項
+ Pod為暫時性，這表示AEM as aCloud Service會視需要建立及銷毀
+ 該環境的「開發人員控制台」的Pod切換器僅會列出屬於相關AEM作為Cloud Service環境一部分的Pod。
+ 在Pod切換器底部，方便選項可依服務類型選取Pod:
   + 所有作者
   + 所有發佈者
   + 所有例項

## 狀態

狀態提供以文字或JSON輸出輸出特定AEM執行階段狀態的選項。 開發人員控制台提供與AEM SDK的本機Quickstart的OSGi Web控制台類似的資訊，其差異之處在於開發人員控制台為唯讀。

![開發人員控制台 — 狀態](./assets/developer-console/status.png)

### 套件組合

套件組合會列出AEM中的所有OSGi套件組合。 此功能與[AEM SDK本機Quickstart的OSGi套件組合](http://localhost:4502/system/console/bundles)(`/system/console/bundles`)類似。

套件組合有助於偵錯：

+ 列出部署至AEM as a Service的所有OSGi套件組合
+ 列出每個OSGi捆綁的狀態；包括是否處於活動狀態
+ 向未解析的依賴項提供詳細資訊，這些依賴項導致OSGi包變得活動

### 元件

元件會列出AEM中的所有OSGi元件。 此功能與[AEM SDK本機Quickstart的OSGi元件](http://localhost:4502/system/console/components)(`/system/console/components`)類似。

元件有助於偵錯：

+ 列出部署至AEM作為Cloud Service的所有OSGi元件
+ 提供每個OSGi元件的狀態；包括活動或未滿足
+ 向未滿足的服務引用提供詳細資訊可能導致OSGi元件變得活動
+ 列出OSGi屬性及其與OSGi元件綁定的值

### 設定

Configurations會列出所有OSGi元件的設定（OSGi屬性和值）。 此功能與[AEM SDK本機Quickstart的OSGi Configuration Manager](http://localhost:4502/system/console/configMgr)(`/system/console/configMgr`)類似。

設定有助於偵錯：

+ 按OSGi元件列出OSGi屬性及其值
+ 查找和標識配置錯誤的屬性

### Oak Indexes

Oak索引提供`/oak:index`下定義之節點的傾印。 請記住，這不會顯示合併的索引，這會在修改AEM索引時發生。

Oak Indexes有助於偵錯：

+ 列出所有Oak索引定義，提供如何在AEM中執行搜尋查詢的深入分析。 請記住，此處不會反映修改為AEM索引的資訊。 此檢視僅適用於僅由AEM提供或僅由自訂程式碼提供的索引。

### OSGi服務

元件列出所有OSGi服務。 此功能與[AEM SDK本機Quickstart的OSGi服務](http://localhost:4502/system/console/services)(`/system/console/services`)類似。

OSGi服務有助於偵錯：

+ 列出AEM中的所有OSGi服務，及其提供的OSGi套件，以及使用該套件的所有OSGi套件

### Sling 工作

Sling作業會列出所有Sling作業佇列。 此功能與[AEM SDK本機Quickstart的](http://localhost:4502/system/console/slingevent)作業`/system/console/slingevent`相似。

Sling作業有助於偵錯：

+ Sling作業佇列清單及其設定
+ 提供作用中、佇列和已處理Sling作業數的深入分析，有助於偵錯AEM中Sling作業所執行的工作流程、暫時工作流程和其他工作的問題。

## Java包

Java套件可讓您檢查Java套件和版本是否可在AEM as aCloud Service中使用。 此功能與[AEM SDK的本機Quickstart相依性尋找器](http://localhost:4502/system/console/depfinder)位於`/system/console/depfinder`相同。

![開發人員主控台 — Java套件](./assets/developer-console/java-packages.png)

Java Packages用於疑難排解由於未解析的匯入，或指令碼（HTL、JSP等）中未解析的類別，導致套件組合無法啟動。 如果Java套件報表沒有套件組合會匯出Java套件（或版本與OSGi套件組合匯入的版本不符）:

+ 確定您專案的AEM API Maven相依性版本符合環境的AEM發行版本（若可能，請將所有項目更新為最新版本）。
+ 如果Maven專案中使用了額外的Maven相依性
   + 判斷是否可改用AEM SDK API相依性提供的替代API。
   + 如果需要額外的相依性，請確保它以OSGi套件組合（而非純Jar）的形式提供，並且它嵌入到您項目的代碼包(`ui.apps`)中，類似於`ui.apps`套件中嵌入核心OSGi套件的方式。

## Servlet

Servlet可用來深入分析AEM如何解析URL至最終處理請求的Java servlet或指令碼(HTL、JSP)。 此功能與[AEM SDK本機Quickstart的Sling Servlet Resolver](http://localhost:4502/system/console/servletresolver) at `/system/console/servletresolver`相同。

![開發人員控制台 — Servlet](./assets/developer-console/servlets.png)

Servlet有助於偵錯判斷：

+ 如何將URL分解為其可定址部分（資源、選取器、擴充功能）。
+ URL解析的Servlet或指令碼，有助於識別格式錯誤的URL或註冊錯誤的servlet/指令碼。

## 查詢

查詢有助於深入分析在AEM上執行搜尋查詢的內容及方式。 此功能與[AEM SDK的本機Quickstart的「工具」>「查詢效能」](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html)控制台相同。

查詢只有在選取特定Pod時才有效，因為Pod會開啟該Pod的「查詢效能」Web主控台，要求開發人員必須具備登入AEM服務的存取權。

![開發人員控制台 — 查詢 — 說明查詢](./assets/developer-console/queries__explain-query.png)

查詢有助於偵錯：

+ 說明Oak如何解譯、分析及執行查詢。 當追蹤查詢為何緩慢，並了解查詢如何加速時，這非常重要。
+ 列出AEM中執行的最熱門查詢，並提供「說明」功能。
+ 列出AEM中執行最慢的查詢，並提供「說明」功能。
