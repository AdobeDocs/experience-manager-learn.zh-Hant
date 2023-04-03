---
title: 開發人員主控台
description: AEM as a Cloud Service針對每個環境提供開發人員主控台，可公開執行中AEM服務的各種有助於除錯的詳細資訊。
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

# 使用開發人員主控台除錯AEMas a Cloud Service

AEM as a Cloud Service針對每個環境提供開發人員主控台，可公開執行中AEM服務的各種有助於除錯的詳細資訊。

每個AEMas a Cloud Service環境都有專屬的開發人員主控台。

## 導覽至開發人員控制台

開發人員主控台可透過Cloud Manager根據AEMas a Cloud Service環境存取。

![導覽至開發人員控制台](./assets/developer-console/navigate.png)

1. 導覽至 __[Cloud Manager](https://my.cloudmanager.adobe.com/)__
2. 開啟 __方案__ 包含AEMas a Cloud Service環境以開啟開發人員控制台。
3. 找出 __環境__，然後選取 `...`.
4. 選擇 __開發人員控制台__ 從下拉式清單。


## 開發人員控制台存取

若要存取及使用開發人員控制台，必須透過以下方式將權限授予開發人員的Adobe ID [AdobeAdmin Console](https://adminconsole.adobe.com).

1. 確定已影響Cloud Manger和AEMas a Cloud Service產品的Adobe組織在Adobe組織切換器中處於作用中狀態。
1. 開發人員必須是 [Cloud Manager產品的 __開發人員 — Cloud Service__ 產品設定檔](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-cloud-manager.html#assign-developer).
   + 如果此會籍不存在，開發人員將無法登入開發人員控制台。
1. 開發人員必須是 [__AEM使用者__ 或 __AEM管理員__ AEM製作和/或發佈上的產品設定檔](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html#aem-product-profiles).
   + 如果此成員資格不存在，則 [狀態](#status) 轉儲將超時，出現401 Unauthorized錯誤。

### 疑難排解開發人員控制台存取

#### 401傾銷狀態時出現未授權錯誤

![開發人員控制台 — 401未授權](./assets/developer-console/troubleshooting__401-unauthorized.png)

如果傾棄任何狀態a 401未授權錯誤報告，表示您的使用者尚未存在AEMas a Cloud Service中具有必要權限，或登入代號的使用無效或已過期。

要解決401未授權問題：

1. 確認您的使用者是Developer Console相關聯AEMas a Cloud Service產品例項的適當Adobe IMS產品設定檔(AEM管理員或AEM使用者)的成員。
   + 請記住，開發人員控制台可存取2個Adobe IMS產品執行個體；AEMas a Cloud Service製作和發佈產品例項，因此請根據需要透過開發人員控制台存取的服務層級，確定使用正確的產品設定檔。
1. 登入AEMas a Cloud Service（製作或發佈），並確認您的使用者和群組已正確同步至AEM。
   + 開發人員控制台需要在對應的AEM服務層中建立您的使用者記錄，以便對該服務層進行驗證。
1. 清除瀏覽器Cookie以及應用程式狀態（本機儲存）並重新登入開發人員控制台，確保開發人員控制台所使用的存取權杖正確且未過期。

## Pod

AEMas a Cloud Service製作和發佈服務分別包含多個例項，以處理流量可變性和滾動式更新，而無需停機。 這些例項稱為Pod。 「開發人員控制台」中的Pod選取項目會定義透過其他控制項公開的資料範圍。

![開發人員主控台 — Pod](./assets/developer-console/pod.png)

+ Pod是屬於AEM服務（製作或發佈）一部分的獨立例項
+ Pod是暫時性的，這表示AEM as a Cloud Service會視需要建立並銷毀它們
+ 只有屬於相關AEMas a Cloud Service環境一部分的Pod，才會列出該環境的開發人員控制台的Pod切換器。
+ 在Pod切換器底部，方便選項可依服務類型選取Pod:
   + 所有作者
   + 所有發佈者
   + 所有例項

## 狀態

狀態提供以文字或JSON輸出輸出特定AEM執行階段狀態的選項。 開發人員控制台提供與AEM SDK的本機Quickstart的OSGi Web控制台類似的資訊，其差異之處在於開發人員控制台為唯讀。

![開發人員控制台 — 狀態](./assets/developer-console/status.png)

### 套件組合

套件組合會列出AEM中的所有OSGi套件組合。 此功能類似於 [AEM SDK的本機Quickstart的OSGi套件組合](http://localhost:4502/system/console/bundles) at `/system/console/bundles`.

套件組合有助於偵錯：

+ 列出部署至AEM as a Service的所有OSGi套件組合
+ 列出每個OSGi捆綁的狀態；包括是否處於活動狀態
+ 向未解析的依賴項提供詳細資訊，這些依賴項導致OSGi包變得活動

### 元件

元件會列出AEM中的所有OSGi元件。 此功能類似於 [AEM SDK的本機Quickstart的OSGi元件](http://localhost:4502/system/console/components) at `/system/console/components`.

元件有助於偵錯：

+ 列出部署至AEM as a Cloud Service的所有OSGi元件
+ 提供每個OSGi元件的狀態；包括活動或未滿足
+ 向未滿足的服務引用提供詳細資訊可能導致OSGi元件變得活動
+ 列出OSGi屬性及其與OSGi元件綁定的值。
   + 這會顯示透過 [OSGi環境設定變數](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values).

### 設定

Configurations會列出所有OSGi元件的設定（OSGi屬性和值）。 此功能類似於 [AEM SDK的本機Quickstart的OSGi Configuration Manager](http://localhost:4502/system/console/configMgr) at `/system/console/configMgr`.

設定有助於偵錯：

+ 按OSGi元件列出OSGi屬性及其值
   + 這「不會」顯示透過 [OSGi環境設定變數](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values). 請參閱 [元件](#components) 上，表示插入的值。
+ 查找和標識配置錯誤的屬性

### Oak Indexes

Oak索引提供下方定義之節點的傾印 `/oak:index`. 請記住，這不會顯示合併的索引，這會在修改AEM索引時發生。

Oak Indexes有助於偵錯：

+ 列出所有Oak索引定義，提供如何在AEM中執行搜尋查詢的深入分析。 請記住，此處不會反映修改為AEM索引的資訊。 此檢視僅適用於僅由AEM提供或僅由自訂程式碼提供的索引。

### OSGi服務

元件列出所有OSGi服務。 此功能類似於 [AEM SDK的本機Quickstart的OSGi服務](http://localhost:4502/system/console/services) at `/system/console/services`.

OSGi服務有助於偵錯：

+ 列出AEM中的所有OSGi服務，及其提供的OSGi套件，以及使用該套件的所有OSGi套件

### Sling 工作

Sling作業會列出所有Sling作業佇列。 此功能類似於 [AEM SDK的本機Quickstart作業](http://localhost:4502/system/console/slingevent) at `/system/console/slingevent`.

Sling作業有助於偵錯：

+ Sling作業佇列清單及其設定
+ 提供作用中、佇列和已處理Sling作業數的深入分析，有助於偵錯AEM中Sling作業所執行的工作流程、暫時工作流程和其他工作的問題。

## Java包

Java套件可讓您檢查Java套件和版本是否可用於AEMas a Cloud Service。 此功能與 [AEM SDK的本機Quickstart相依性尋找器](http://localhost:4502/system/console/depfinder) at `/system/console/depfinder`.

![開發人員主控台 — Java套件](./assets/developer-console/java-packages.png)

Java Packages用於疑難排解由於未解析的匯入，或指令碼（HTL、JSP等）中未解析的類別，導致套件組合無法啟動。 如果Java套件報表沒有套件組合會匯出Java套件（或版本與OSGi套件組合匯入的版本不符）:

+ 確定您專案的AEM API Maven相依性版本符合環境的AEM發行版本（若可能，請將所有項目更新為最新版本）。
+ 如果Maven專案中使用了額外的Maven相依性
   + 判斷是否可改用AEM SDK API相依性提供的替代API。
   + 如果需要額外的相依性，請確定它以OSGi套件組合（而非純Jar）的形式提供，且已內嵌於您專案的程式碼套件中(`ui.apps`)，類似核心OSGi套件組合內嵌於 `ui.apps` 包。

## Servlet

Servlet可用來深入分析AEM如何解析URL至最終處理請求的Java servlet或指令碼(HTL、JSP)。 此功能與 [AEM SDK的本機Quickstart的Sling Servlet解析程式](http://localhost:4502/system/console/servletresolver) at `/system/console/servletresolver`.

![開發人員控制台 — Servlet](./assets/developer-console/servlets.png)

Servlet有助於偵錯判斷：

+ 如何將URL分解為其可定址部分（資源、選取器、擴充功能）。
+ URL解析的Servlet或指令碼，有助於識別格式錯誤的URL或註冊錯誤的servlet/指令碼。

## 查詢

查詢有助於深入分析在AEM上執行搜尋查詢的內容及方式。 此功能與  [AEM SDK的本機Quickstart工具>查詢效能 ](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) 控制台。

查詢只有在選取特定Pod時才有效，因為Pod會開啟該Pod的「查詢效能」Web主控台，要求開發人員必須具備登入AEM服務的存取權。

![開發人員控制台 — 查詢 — 說明查詢](./assets/developer-console/queries__explain-query.png)

查詢有助於偵錯：

+ 說明Oak如何解譯、分析及執行查詢。 當追蹤查詢為何緩慢，並了解查詢如何加速時，這非常重要。
+ 列出AEM中執行的最熱門查詢，並提供「說明」功能。
+ 列出AEM中執行最慢的查詢，並提供「說明」功能。
