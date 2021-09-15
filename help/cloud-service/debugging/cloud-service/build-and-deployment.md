---
title: 建置和部署
description: AdobeCloud Manager可協助程式碼建立和部署至AEM作為Cloud Service。 建置程式的步驟期間可能會發生失敗，需要採取行動加以解決。 本指南會逐步說明部署中的常見故障，以及如何以最佳方式處理這些故障。
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5434
thumbnail: kt-5424.jpg
topic: Development
role: Developer
level: Beginner
exl-id: b4985c30-3e5e-470e-b68d-0f6c5cbf4690
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '2526'
ht-degree: 0%

---

# 將AEM作為Cloud Service建置和部署進行偵錯

AdobeCloud Manager可協助程式碼建立和部署至AEM作為Cloud Service。 建置程式的步驟期間可能會發生失敗，需要採取行動加以解決。 本指南會逐步說明部署中的常見故障，以及如何以最佳方式處理這些故障。

![雲端管理建置管道](./assets/build-and-deployment/build-pipeline.png)

## 驗證

驗證步驟只需確保基本Cloud Manager配置有效。 常見的驗證失敗包括：

### 環境處於無效狀態

+ __錯誤訊息：__ 環境處於無效狀態。
   ![環境處於無效狀態](./assets/build-and-deployment/validation__invalid-state.png)
+ __原因：__ 管道的目標環境處於過渡狀態，此時它無法接受新組建。
+ __解決方法：__ 等待狀態解析為執行中（或更新為可用）狀態。如果正在刪除環境，請重新建立環境，或選擇要建置的不同環境。

### 找不到與管道相關聯的環境

+ __錯誤訊息：__ 環境已標示為已刪除。
   ![環境會標示為已刪除](./assets/build-and-deployment/validation__environment-marked-as-deleted.png)
+ __原因：__ 管道配置為使用的環境已被刪除。即使重新建立同名的新環境，Cloud Manager也不會自動將管道重新關聯至該同名環境。
+ __解決方法：__ 編輯管道設定，並重新選取要部署的環境。

### 找不到與管道相關聯的Git分支

+ __錯誤訊息：__ 管道無效：XXXXXX。在儲存庫中找不到Reason=Branch=xxxx。
   ![管道無效：XXXXXX。在儲存庫](./assets/build-and-deployment/validation__branch-not-found.png)中找不到Reason=Branch=xxxx
+ __原因：__ 管道設定要使用的Git分支已遭刪除。
+ __解決方法：__ 使用完全相同的名稱重新建立遺失的Git分支，或重新設定管道以從不同的現有分支建立。

## 構建和單元測試

![構建和單元測試](./assets/build-and-deployment/build-and-unit-testing.png)

「建置」和「單元測試」階段會執行從管道設定的Git分支中結帳之專案的Maven建置(`mvn clean package`)。

本階段查明的錯誤應可在本地重建項目，但有下列例外：

+ 使用的是[Maven Central](https://search.maven.org/)上不可用的Maven依存關係，包含該相依性的Maven存放庫為：
   + 無法從Cloud Manager（例如私有內部Maven存放庫）或Maven存放庫存取，需要驗證且提供的憑證不正確。
   + 未在項目的`pom.xml`中顯式註冊。 請注意，不鼓勵使用Maven存放庫，因為它會增加建置時間。
+ 設備測試由於計時問題而失敗。 當單元測試對時間敏感時，可能會發生這種情況。 強指示器依賴測試代碼中的`.sleep(..)`。
+ 使用不支援的Maven外掛程式。

## 程式碼掃描

![程式碼掃描](./assets/build-and-deployment/code-scanning.png)

程式碼掃描會混合使用Java和AEM專用的最佳實務來執行靜態程式碼分析。

如果程式碼中存在嚴重安全漏洞，程式碼掃描會導致建置失敗。 可以覆蓋較小的違規，但建議修正這些違規。 請注意，程式碼掃描不完善，可能導致[誤判](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/test-results/overview-test-results.html#dealing-with-false-positives)。

若要解決程式碼掃描問題，請透過&#x200B;**下載詳細資料**&#x200B;按鈕下載Cloud Manager提供的CSV格式報表，並檢閱任何項目。

如需更多詳細資訊，請參閱AEM特定規則，請參閱Cloud Manager檔案的[自訂AEM特定程式碼掃描規則](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/custom-code-quality-rules.html)。

## 建立影像

![建立影像](./assets/build-and-deployment/build-images.png)

「建置」影像負責結合在「建置與單元測試」步驟中建立的程式碼成品與AEM版本，以形成單一可部署的成品。

雖然在「建置與單元測試」期間發現任何程式碼建置和編譯問題，但嘗試將自訂建置工件與AEM版本結合時，可能會發現設定或結構問題。

### 重複OSGi配置

當多個OSGi設定透過目標AEM環境的執行模式解析時，「建置影像」步驟會失敗，並出現錯誤：

```
[ERROR] Unable to convert content-package [/tmp/packages/enduser.all-1.0-SNAPSHOT.zip]: 
Configuration ‘com.example.ExampleComponent’ already defined in Feature Model ‘com.example.groupId:example.all:slingosgifeature:xxxxx:X.X’, 
set the ‘mergeConfigurations’ flag to ‘true’ if you want to merge multiple configurations with same PID
```

#### 原因1

+ __原因：__ AEM專案的所有套件包含多個程式碼套件，且多個程式碼套件提供相同的OSGi設定，導致衝突，導致「建置影像」步驟無法決定應使用哪個項目，進而導致建置失敗。請注意，這不適用於OSGi工廠設定，只要這些設定的名稱是唯一的。
+ __解決方法：__ 檢閱部署為AEM應用程式一部分的所有程式碼套件（包括任何包含的第三方程式碼套件），尋找可透過執行模式解析至目標環境的重複OSGi設定。在AEM as a Cloud Service中無法使用錯誤訊息的「將mergeConfigurations標幟設為true」指引，因此應予以忽略。

#### 原因2

+ __原因：__ AEM專案的錯誤包含相同的程式碼套件兩次，導致重複該套件中包含的任何OSGi設定。
+ __解決方法：__ 檢閱所有專案中內嵌的所有pom.xml套件，並確定其設定 `filevault-package-maven-plugin` [](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html#cloud-manager-target) 已設 `<cloudManagerTarget>none</cloudManagerTarget>`為。

### 錯誤的重新指向指令碼

重新指向指令碼定義基線內容、使用者、ACL等。 在AEM as aCloud Service中，重新指向指令碼會在建置影像期間套用，不過，在AEM SDK的本機快速入門上，這些指令碼會在啟動OSGi重新指向工廠設定時套用。 因此，AEM SDK的本機Quickstart上的重新指向指令碼可能會悄悄地失敗（隨著記錄），但會導致「建置影像」步驟失敗，導致部署停止。

+ __原因：__ 重新指向指令碼的格式不正確。請注意，這可能會使您的存放庫處於不完整狀態，因為在對存放庫執行失敗指令碼之後，會執行任何重新指向指令碼。
+ __解決方法：__ 部署重新指令碼OSGi設定時，請檢閱AEM SDK的本機Quickstart，以判斷錯誤是否及錯誤為何。

### 未滿足的重新指向內容依賴

重新指向指令碼定義基線內容、使用者、ACL等。 在AEM SDK的本機Quickstart中，當重新指向OSGi工廠配置啟動時，或換句話說，在存放庫處於作用中狀態且可能直接或透過內容套件發生內容變更後，會套用重新指向指令碼。 在AEM as aCloud Service中，在建置影像期間，會對可能不包含重新指向指令碼所依賴內容的存放庫套用重新指向指令碼。

+ __原因：__ 重新指向指令碼取決於不存在的內容。
+ __解決方法：__ 確保重新指向指令碼所依賴的內容存在。這通常表示定義不充分的重新指向指令碼，這些指令缺少定義這些缺失但需要的內容結構的指令。 您可以刪除AEM、解壓縮Jar並將包含重新指向指令碼的重新指向OSGi配置添加到安裝資料夾，然後啟動AEM，在本地重新生成此指令碼。 此錯誤會出現在AEM SDK本機Quickstart的error.log中。


### 應用程式的核心元件版本大於部署的版本

_此問題只會影響不會自動更新至最新AEM版本的非生產環境。_

AEM as aCloud Service會在每個AEM版本中自動加入最新的核心元件版本，這表示AEM as aCloud Service環境會自動或手動更新，並部署最新版核心元件後。

在下列情況下，「建置影像」步驟可能會失敗：

+ 部署應用程式會更新`core`（OSGi套件組合）專案中的核心元件maven相依性版本
+ 接著，部署應用程式會部署至沙箱（非生產）AEM作為Cloud Service環境，而此環境尚未更新，以使用包含該新核心元件版本的AEM版本。

為避免此錯誤，只要AEM as aCloud Service環境有更新可用，請將更新納入下一個建置/部署的一部分，並一律確保在應用程式程式碼基底中遞增核心元件版本後包含更新。

+ __症狀：__
「建置影像」步驟失敗，並出現錯誤報告，指出 
`com.adobe.cq.wcm.core.components...` 專案無法匯入特定版本範圍的套 `core` 件。

   ```
   [ERROR] Bundle com.example.core:0.0.3-SNAPSHOT is importing package(s) Package com.adobe.cq.wcm.core.components.models;version=[12.13,13) in start level 20 but no bundle is exporting these for that start level in the required version range.
   [ERROR] Analyser detected errors on feature 'com.adobe.granite:aem-ethos-app-image:slingosgifeature:aem-runtime-application-publish-dev:1.0.0-SNAPSHOT'. See log output for error messages.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   ```

+ __原因：__  應用程式的OSGi套件組合(在專案中定 `core` 義)會從核心元件核心相依性匯入Java類別，其版本層級與部署至AEM作為Cloud Service的版本層級不同。
+ __解析度:__
   + 使用Git時，還原至核心元件版本增加前存在的工作提交。 將此提交推送至Cloud Manager Git分支，並從此分支執行環境更新。 這會將AEM作為Cloud Service升級至最新的AEM版本，其中包含更新的核心元件版本。 將AEM as aCloud Service更新至最新的AEM版本（將有最新的核心元件版本）後，重新部署原本失敗的程式碼。
   + 若要在本機重制此問題，請確定AEM SDK版本與Cloud Service環境所使用的AEM版本相同。


### 建立Adobe支援案例

如果上述的疑難排解方法無法解決問題，請透過以下方式建立Adobe支援案例：

+ [Adobe Admin Console](https://adminconsole.adobe.com)  >支援標籤>建立案例

   _如果您是多個Adobe組織的成員，請在建立案例之前，先在Adobe組織切換器中選取管道失敗的Adobe組織。_

## 部署至

「部署至」步驟負責取用「建置影像」中產生的程式碼工件、使用它啟動新的AEM製作和發佈服務，並在成功時移除任何舊的AEM製作和發佈服務。 可在此步驟中安裝和更新可變內容包和索引。

在調試「部署至」步驟之前，請熟悉[AEM as aCloud Service日誌](./logs.md)。 `aemerror`記錄檔包含與Deploy to issues（部署到問題）相關的Pod啟動和關閉資訊。 請注意，Cloud Manager「部署至」步驟中「下載日誌」按鈕提供的日誌不是`aemerror`日誌，不包含與應用程式啟動相關的詳細資訊。

![部署至](./assets/build-and-deployment/deploy-to.png)

「部署至」步驟可能失敗的三個主要原因：

### Cloud Manager管道保有舊版AEM

+ __原因：__  Cloud Manager管道保有的AEM版本比部署至目標環境的舊版本。當重複使用管道並指向執行較新版本AEM的新環境時，就可能會發生此情況。 這可透過檢查環境的AEM版本是否大於管道的AEM版本來識別。
   ![Cloud Manager管道保有舊版AEM](./assets/build-and-deployment/deploy-to__pipeline-holds-old-aem-version.png)
+ __解析度:__
   + 如果目標環境有可用更新，請從環境的動作中選取更新，然後重新執行組建。
   + 如果目標環境沒有可用的更新，表示它執行的是最新版AEM。 若要解決此問題，請刪除管道並重新建立它。


### Cloud Manager逾時

新部署的AEM服務啟動期間執行的程式碼需花費很長時間，使得Cloud Manager在部署完成之前逾時。 在這些情況下，即使認為Cloud Manager狀態報告為「失敗」，部署最終也可能成功。

+ __原因：__ 自訂程式碼可能會執行OSGi套件或元件生命週期早期觸發的大型查詢或內容周遊等作業，大幅延遲AEM的啟動時間。
+ __解決方法：__ 檢閱OSGi套件組合生命週期早期執行之程式碼的實作，並檢閱 `aemerror` AEM製作和發佈服務在失敗（以GMT為記錄時間）前後的記錄（如Cloud Manager所示），以及尋找指出任何自訂記錄執行程式的記錄訊息。

### 代碼或配置不相容

大部分的程式碼和設定違反行為都會在組建的較早階段中擷取，不過自訂程式碼或設定可能與AEM as aCloud Service不相容，且未偵測到，直到在容器中執行為止。

+ __原因：__ 自訂程式碼可能會叫用長時間作業，例如大型查詢或內容周遊，在OSGi套件或元件生命週期早期觸發，大幅延遲AEM的啟動時間。
+ __解決方__ 法： `aemerror` 檢閱AEM製作和發佈服務在失敗的時間（以GMT為記錄檔）內的記錄，如Cloud Manager所示。
   1. 檢閱記錄檔，了解自訂應用程式提供的Java類所擲回的任何錯誤。 若發現任何問題，請解決、推送修正的程式碼，然後重新建置管道。
   1. 查看您在自訂應用程式中延伸/互動的AEM各方面所回報的任何錯誤記錄，並調查這些錯誤；這些錯誤可能不直接歸因於Java類。 若發現任何問題，請解決、推送修正的程式碼，然後重新建置管道。

### 在內容套件中包含/var

`/var` 可變，包含各種暫時的執行階段內容。在內容套件中包括`/var`(例如 `ui.content`)部署時，可能會導致部署步驟失敗。

此問題難以識別，因為它不會在初始部署（僅在後續部署）中導致失敗。 明顯的症狀包括：

+ 初始部署成功，不過新內容或已變更的可變內容（屬於部署的一部分）似乎不存在於AEM Publish服務中。
+ AEM Author中內容的啟用/停用已遭封鎖
+ 後續部署在「部署至」步驟中失敗，「部署至」步驟在約60分鐘後失敗。

驗證此問題是失敗行為的原因：

1. 確定至少一個內容包是部署的一部分，寫入`/var`。
1. 驗證主（粗體）分發隊列是否在以下位置被阻止：
   + AEM製作>工具>部署>發佈
      ![已阻止的分發隊列](./assets/build-and-deployment/deploy-to__var--distribution.png)
1. 在後續部署失敗時，請使用「下載記錄」按鈕下載Cloud Manager的「部署至」記錄：

   ![下載部署到日誌](./assets/build-and-deployment/deploy-to__var--download-logs.png)

   ...並確認記錄陳述式之間大約有60分鐘：

   ```
   2020-01-01T01:01:02+0000 Begin deployment in aem-program-x-env-y-dev [CorrelationId: 1234]
   ```

   ... 和 ...

   ```
   2020-01-01T02:04:10+0000 Failed deployment in aem-program-x-env-y-dev
   ```

   請注意，此記錄檔不會在初次部署報告為成功的指標，而只會包含在後續失敗的部署。

+ __原因：__ AEM復寫服務使用者用來將內容套件部署至AEM Publish服務，無法在AEM  `/var` Publish上寫入。這會導致將內容套件部署至AEM Publish服務失敗。
+ __解決方法：__ 以下是解決此問題的方法，依優先順序列出：
   1. 如果不需要`/var`資源，請從作為應用程式一部分部署的內容包中刪除`/var`下的任何資源。
   2. 如果需要`/var`資源，請使用[repoint](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit)定義節點結構。 可透過OSGi執行模式，將重新指向指令碼鎖定在AEM製作、AEM發佈或兩者。
   3. 如果`/var`資源僅需AEM作者，且無法使用[repoinit](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit)合理建模，請將資源移至離散內容套件，而此套件僅由[embedding](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html#embeddeds)安裝於AEM作者執行模式資料夾(`<target>/apps/example-packages/content/install.author</target>`)的`all`套件中。
   4. 如本[AdobeKB](https://helpx.adobe.com/in/experience-manager/kb/cm/cloudmanager-deploy-fails-due-to-sling-distribution-aem.html)中所述，為`sling-distribution-importer`服務用戶提供適當的ACL。

### 建立Adobe支援案例

如果上述的疑難排解方法無法解決問題，請透過以下方式建立Adobe支援案例：

+ [Adobe Admin Console](https://adminconsole.adobe.com)  >支援標籤>建立案例

   _如果您是多個Adobe組織的成員，請在建立案例之前，先在Adobe組織切換器中選取管道失敗的Adobe組織。_
