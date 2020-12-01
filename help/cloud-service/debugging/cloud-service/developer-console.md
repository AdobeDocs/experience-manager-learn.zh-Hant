---
title: 開發人員主控台
description: AEM as a Cloud Service為每個環境提供「開發人員主控台」，可公開執行中有助於除錯的AEM服務的各種詳細資訊。
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5433
thumbnail: kt-5433.jpg
translation-type: tm+mt
source-git-commit: 1af3661e5c18206d58d339d51d5189834e843023
workflow-type: tm+mt
source-wordcount: '1344'
ht-degree: 0%

---


# 使用Developer Console將AEM除錯為雲端服務

AEM as a Cloud Service為每個環境提供「開發人員主控台」，可公開執行中有助於除錯的AEM服務的各種詳細資訊。

每個AEM都是雲端服務環境，有其專屬的Developer Console。

## Developer Console存取

若要存取和使用「開發人員主控台」，必須透過[Adobe的Admin Console](https://adminconsole.adobe.com)，為開發人員的Adobe ID提供下列權限。

1. 確定已將Cloud Manger和AEM當做雲端服務產品的Adobe組織在Adobe組織切換器中處於作用中。
1. 開發人員必須是Cloud Manager產品&#x200B;__的「開發人員——雲端服務」__&#x200B;產品設定檔的成員。
   + 如果此會籍不存在，開發人員將無法登入Developer Console。
1. 開發人員必須是AEM Author和Publish服務的&#x200B;__AEM Administrators__&#x200B;產品設定檔的成員。
   + 如果此會籍不存在， [status](#status)轉儲將逾時，並出現401未授權錯誤。

### 疑難排解Developer Console存取

#### 401轉儲狀態時出現未授權錯誤

![Developer Console - 401未授權](./assets/developer-console/troubleshooting__401-unauthorized.png)

如果報告401未授權錯誤的轉儲狀態，表示您的使用者尚未在AEM中擁有必要的雲端服務權限，或登入Token使用無效或已過期。

要解決401未授權問題：

1. 請確定您的使用者是Developer Console相關AEM的Cloud Service產品例項適當Adobe IMS產品設定檔（AEM管理員或AEM使用者）的成員。
   + 請記住，Developer Console可存取2個Adobe IMS產品執行個體；AEM為雲端服務作者和發佈產品例項，因此請確定使用正確的產品設定檔，視需要透過Developer Console存取的服務層而定。
1. 以雲端服務（「作者」或「發佈」）的身分登入AEM，並確保您的使用者和群組已正確同步至AEM。
   + Developer Console要求您在對應的AEM服務層中建立使用者記錄，以供其驗證至該服務層。
1. 清除您的瀏覽器Cookie以及應用程式狀態（本機儲存）並重新登入Developer Console，以確保Developer Console使用的存取Token正確且未過期。

## Pod

AEM（雲端服務作者）和Publish服務分別包含多個執行個體，以處理流量可變性和滾動更新，而不會造成停機。 這些例項稱為Pod。 Developer Console中的Pod選擇定義透過其他控制項公開的資料範圍。

![Developer Console - Pod](./assets/developer-console/pod.png)

+ pod是屬於AEM服務（「作者」或「發佈」）一部分的獨立例項
+ Pod是暫時性的，這表示AEM是雲端服務，會視需要建立並銷毀它們
+ 只有屬於相關AEM（雲端服務環境）一部分的Pod，才會列出該環境的Developer Console Pod切換器。
+ 在Pod切換器底部，方便的選項允許依服務類型選擇Pod:
   + 所有作者
   + 所有發佈者
   + 所有例項

## 狀態

狀態提供選項，可輸出文字或JSON輸出中的特定AEM執行階段狀態。 Developer Console提供類似AEM SDK本機快速入門的OSGi Web主控台的資訊，其中顯示的差異是Developer Console為唯讀。

![Developer Console —— 狀態](./assets/developer-console/status.png)

### 組合

Bundles會列出AEM中的所有OSGi bundles。 此功能類似於[AEM SDK的本機快速入門的OSGi Bundles](http://localhost:4502/system/console/bundles)，位於`/system/console/bundles`。

搭售除錯的說明，方法為：

+ 列出部署至AEM的所有OSGi搭售，視為服務
+ 列出每個OSGi捆綁包的狀態；包括是否活動
+ 提供未解決的依賴關係的詳細資訊，這些依賴關係導致OSGi捆綁包變為活動

### 元件

元件會列出AEM中的所有OSGi元件。 此功能類似於[AEM SDK的本機快速入門的OSGi元件](http://localhost:4502/system/console/components)，位於`/system/console/components`。

元件在除錯時的說明，請依下列方式：

+ 將部署至AEM的所有OSGi元件列為雲端服務
+ 提供每個OSGi元件的狀態；包括活動或未滿足
+ 向未滿足的服務引用提供詳細資訊可能導致OSGi元件變為活動狀態
+ 列出綁定到OSGi元件的OSGi屬性及其值

### 設定

配置列出了所有OSGi元件的配置（OSGi屬性和值）。 此功能類似於[AEM SDK的本機快速入門的OSGi Configuration Manager](http://localhost:4502/system/console/configMgr)，位於`/system/console/configMgr`。

配置有助於調試，方法是：

+ 按OSGi元件列出OSGi屬性及其值
+ 查找並標識配置錯誤的屬性

### Oak Indexes

Oak Indexes提供在`/oak:index`下定義的節點轉儲。 請記住，這不會顯示合併索引，這會在修改AEM索引時發生。

Oak Indexes在除錯時的說明：

+ 列出所有Oak Index定義，提供如何在AEM中執行搜尋查詢的深入資訊。 請記住，此處不會反映修改為AEM索引的內容。 此檢視僅適用於僅由AEM提供或僅由自訂程式碼提供的索引。

### OSGi服務

元件列出所有OSGi服務。 此功能類似於[AEM SDK的本機快速入門的OSGi Services](http://localhost:4502/system/console/services)，位於`/system/console/services`。

OSGi Services可協助除錯，方法如下：

+ 列出AEM中的所有OSGi服務，以及其提供的OSGi搭售，以及使用它的所有OSGi搭售

### Sling 工作

Sling Jobs會列出所有Sling Jobs佇列。 此功能類似於[AEM SDK的本機快速入門作業](http://localhost:4502/system/console/slingevent)（位於`/system/console/slingevent`）。

Sling Jobs在除錯中的說明：

+ Sling Job佇列清單及其組態
+ 提供有關作用中、佇列和處理之Sling工作數目的深入資訊，這有助於除錯Workflow、Transient Workflow和Sling Jobs在AEM中執行的其他工作的問題。

## Java包

Java套件可讓您檢查Java套件和版本是否可在AEM中以雲端服務的形式使用。 此功能與[AEM SDK的本機快速入門相依性搜尋器](http://localhost:4502/system/console/depfinder)在`/system/console/depfinder`的功能相同。

![Developer Console - Java套件](./assets/developer-console/java-packages.png)

Java Packages用於難以啟動Bundles，因為無法解析的導入或指令碼（HTL、JSP等）中的未解析類。 如果Java包報告沒有包導出Java包（或版本與OSGi包導入的版本不匹配）:

+ 請確定您專案的AEM API Maven相依性版本符合環境的AEM Release版本（若可能，請將所有內容更新為最新版本）。
+ 如果Maven項目中使用了額外的Maven依賴項
   + 判斷是否可改用AEM SDK API相依性提供的替代API。
   + 如果需要額外的相依性，請確定它是以OSGi包（而非純Jar）的形式提供，並且它嵌入到項目的代碼包(`ui.apps`)中，類似於`ui.apps`包中嵌入核心OSGi包的方式。

## Servlet

Servlet可用來分析AEM如何解析最終處理請求的Java servlet或指令碼(HTL、JSP)的URL。 此功能與[AEM SDK本機快速入門的Sling Servlet Resolver](http://localhost:4502/system/console/servletresolver)在`/system/console/servletresolver`的功能相同。

![Developer Console - Servlets](./assets/developer-console/servlets.png)

Servlet有助於調試確定：

+ 如何將URL分解為其可定址部分（資源、選擇器、擴充功能）。
+ URL解析的servlet或指令碼，有助於識別格式錯誤的URL或註冊錯誤的servlet/指令碼。

## 查詢

查詢可協助您深入瞭解在AEM上執行搜尋查詢的方式與方式。 此功能與[AEM SDK的本機快速入門工具>查詢效能](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html)主控台相同。

查詢僅在選取特定pod時運作，因為它會開啟該pod的Query Performance Web主控台，因此開發人員必須擁有登入AEM服務的存取權。

![Developer Console —— 查詢——說明查詢](./assets/developer-console/queries__explain-query.png)

查詢有助於調試，方法是：

+ 說明Oak如何解譯、分析和執行查詢。 當追蹤查詢的速度緩慢，並瞭解如何加快查詢時，這一點非常重要。
+ 列出在AEM中執行的最受歡迎查詢，並能夠「解釋」這些查詢。
+ 列出在AEM中執行的最慢查詢，並能夠「解釋」這些查詢。
