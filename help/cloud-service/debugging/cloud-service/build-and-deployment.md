---
title: 建置和部署
description: AdobeCloud Manager有助於程式碼構建和部署到AEM as a Cloud Service。 在建置流程的步驟中可能會發生失敗，需要採取行動來解決。 本指南會逐步引導您瞭解部署中的常見失敗，以及如何採取最佳方法。
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5434
thumbnail: kt-5424.jpg
topic: Development
role: Developer
level: Beginner
exl-id: b4985c30-3e5e-470e-b68d-0f6c5cbf4690
duration: 534
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '2476'
ht-degree: 0%

---

# 偵錯AEM as a Cloud Service建置和部署

AdobeCloud Manager有助於程式碼構建和部署到AEM as a Cloud Service。 在建置流程的步驟中可能會發生失敗，需要採取行動來解決。 本指南會逐步引導您瞭解部署中的常見失敗，以及如何採取最佳方法。

![雲端管理組建管道](./assets/build-and-deployment/build-pipeline.png)

## 驗證 

驗證步驟只會確保基本Cloud Manager設定有效。 常見的驗證失敗包括：

### 環境處於無效狀態

+ __錯誤訊息：__環境處於無效狀態。
  ![環境處於無效狀態](./assets/build-and-deployment/validation__invalid-state.png)
+ __原因：__&#x200B;管道的目標環境處於轉換狀態，無法接受新的組建。
+ __解析度：__&#x200B;等候狀態解析為執行中（或更新可用）狀態。 如果要刪除環境，請重新建立環境，或選擇要建置到的其他環境。

### 找不到與管道關聯的環境

+ __錯誤訊息：__環境已標籤為已刪除。
  ![環境已標籤為已刪除](./assets/build-and-deployment/validation__environment-marked-as-deleted.png)
+ __原因：__管道設定要使用的環境已刪除。
即使重新建立相同名稱的新環境，Cloud Manager也不會自動將管道重新關聯至該相同名稱環境。
+ __解決方法：__&#x200B;編輯管道設定，並重新選取要部署到的環境。

### 找不到與管道相關聯的Git分支

+ __錯誤訊息：__無效的管道： XXXXXX。 在存放庫中找不到Reason=Branch=xxxx。
  ![無效的管道： XXXXXX。 在存放庫](./assets/build-and-deployment/validation__branch-not-found.png)中找不到Reason=Branch=xxxx
+ __原因：__&#x200B;管道設定要使用的Git分支已刪除。
+ __解析度：__&#x200B;使用完全相同的名稱重新建立遺失的Git分支，或重新設定管道以從不同的現有分支建置。

## 建置和單元測試

![建置和單元測試](./assets/build-and-deployment/build-and-unit-testing.png)

建置和單位測試階段會執行從管道已設定的Git分支中籤出的專案的Maven建置(`mvn clean package`)。

在本機建置專案時，此階段發現的錯誤應可重新產生，但有下列例外：

+ [Maven Central](https://search.maven.org/)上未提供Maven相依性，且包含該相依性的Maven存放庫為：
   + 無法從Cloud Manager聯絡，例如私人內部Maven存放庫或Maven存放庫需要驗證並提供不正確的認證。
   + 未在專案的`pom.xml`中明確註冊。 請注意，不建議加入Maven存放庫，因為這會增加建置時間。
+ 由於時間問題，單元測試失敗。 當單元測試對時間敏感時，可能會發生這種情況。 強指標依賴測試程式碼中的`.sleep(..)`。
+ 使用不支援的Maven外掛程式。

## 程式碼掃描

![程式碼掃描](./assets/build-and-deployment/code-scanning.png)

程式碼掃描會混合使用Java和AEM專屬的最佳實務來執行靜態程式碼分析。

如果程式碼中存在嚴重安全性漏洞，程式碼掃描會導致組建失敗。 可以覆寫較小的違規，但建議修正這些違規。 請注意，程式碼掃描並不完美，可能會產生[誤判](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/test-results/overview-test-results.html#dealing-with-false-positives)。

若要解決程式碼掃描問題，請下載Cloud Manager透過&#x200B;**下載詳細資料**&#x200B;按鈕提供的CSV格式報告，並檢閱任何專案。

如需詳細資訊，請參閱AEM特定規則，請參閱Cloud Manager檔案的[自訂AEM特定程式碼掃描規則](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/custom-code-quality-rules.html)。

## 建置影像

![建置影像](./assets/build-and-deployment/build-images.png)

組建影像負責將在「建置和單位測試」步驟中建立的組建程式碼成品與AEM版本結合，以形成單一可部署成品。

雖然在建置和單元測試期間發現任何程式碼建置和編譯問題，但在嘗試將自訂建置成品與AEM版本結合時，可能會發現設定或結構問題。

### 複製OSGi設定

當目標AEM環境透過執行模式解析多個OSGi設定時，建置影像步驟會失敗並出現錯誤：

```
[ERROR] Unable to convert content-package [/tmp/packages/enduser.all-1.0-SNAPSHOT.zip]: 
Configuration 'com.example.ExampleComponent' already defined in Feature Model 'com.example.groupId:example.all:slingosgifeature:xxxxx:X.X', 
set the 'mergeConfigurations' flag to 'true' if you want to merge multiple configurations with same PID
```

#### 原因1

+ __原因：__ AEM專案的all套件包含多個程式碼套件，且多個程式碼套件提供相同的OSGi設定，導致衝突，導致Build Image步驟無法決定應該使用哪一個，進而導致Build失敗。 請注意，這不適用於OSGi原廠設定，只要它們有唯一名稱。
+ __解決方法：__&#x200B;檢閱部署為AEM應用程式一部分的所有程式碼套件（包括任何包含的第三方程式碼套件），尋找透過執行模式解析到目標環境的重複OSGi設定。 在AEM as a Cloud Service中不可能有錯誤訊息「將mergeConfigurations標幟設為true」的指引，應予以忽略。

#### 原因2

+ __原因：__ AEM專案不正確地包含相同的程式碼套件兩次，導致該套件中包含的任何OSGi設定重複。
+ __解析度：__&#x200B;檢閱所有專案中內嵌的所有pom.xml封裝，並確定它們的`filevault-package-maven-plugin` [組態](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html#cloud-manager-target)設定為`<cloudManagerTarget>none</cloudManagerTarget>`。

### 格式錯誤的repoinit指令碼

Repoinit指令碼可定義基準內容、使用者、ACL等。 在AEM as a Cloud Service中，Repoinit指令碼會在建置影像期間套用，但在AEM SDK的本機Quickstart中，這些指令碼會在OSGi Repoinit工廠設定啟動時套用。 因此，Repoinit指令碼可能會在AEM SDK的本機Quickstart上悄悄失敗（記錄），但會導致「建置影像」步驟失敗，並停止部署。

+ __原因：__ Repoinit指令碼格式錯誤。 這可能會導致您的存放庫處於不完整狀態，因為在失敗指令碼未對存放庫執行後，任何repoinit指令碼都會受到影響。
+ __解決方法：__&#x200B;部署repoinit指令碼OSGi設定時，請檢閱AEM SDK的本機Quickstart，以判斷錯誤是否及其內容。

### 不滿意的repoinit內容相依性

Repoinit指令碼可定義基準內容、使用者、ACL等。 在AEM SDK的本機Quickstart中，Repoinit指令碼會在Repoinit OSGi工廠設定啟動時套用，換言之，在存放庫啟動且可能直接或透過內容套件產生內容變更後。 在AEM as a Cloud Service中，Repoinit指令碼會在「建置影像」期間套用至存放庫，該存放庫可能未包含Repoinit指令碼所相依的內容。

+ __原因：__ Repoinit指令碼所依賴的內容不存在。
+ __解析度：__&#x200B;請確定repoinit指令碼所依賴的內容存在。 這通常表示未充分定義的repoinit指令碼遺漏了定義這些遺漏但必要的內容結構的指令。 可以透過刪除AEM、解壓縮Jar並將包含repoinit指令碼的repoinit OSGi設定新增到安裝資料夾，以及啟動AEM在本機重新產生這種情況。 錯誤會顯示在AEM SDK本機Quickstart的error.log中。


### 應用程式的核心元件版本高於已部署的版本

_此問題只會影響不會自動更新至最新AEM版本的非生產環境。_

AEM as a Cloud Service會自動在每個AEM發行版本中加入最新的核心元件版本，這表示AEM as a Cloud Service環境自動或手動更新且已部署最新版本的核心元件之後。

在以下情況下，「建置影像」步驟可能會失敗：

+ 部署應用程式會更新`core` （OSGi套件）專案中的核心元件Maven相依性版本
+ 然後，部署應用程式會部署到沙箱（非生產） AEM as a Cloud Service環境，該環境尚未更新為使用包含新核心元件版本的AEM版本。

為防止此失敗，每當有AEM as a Cloud Service環境更新可用，請將更新加入下一個建置/部署的一部分，並一律確保在增加應用程式程式碼庫中的核心元件版本之後加入更新。

+ __症狀：__
建置影像步驟失敗並出現ERROR報告，指出`core`專案無法匯入特定版本範圍的`com.adobe.cq.wcm.core.components...`個套件。

  ```
  [ERROR] Bundle com.example.core:0.0.3-SNAPSHOT is importing package(s) Package com.adobe.cq.wcm.core.components.models;version=[12.13,13) in start level 20 but no bundle is exporting these for that start level in the required version range.
  [ERROR] Analyser detected errors on feature 'com.adobe.granite:aem-ethos-app-image:slingosgifeature:aem-runtime-application-publish-dev:1.0.0-SNAPSHOT'. See log output for error messages.
  [INFO] ------------------------------------------------------------------------
  [INFO] BUILD FAILURE
  [INFO] ------------------------------------------------------------------------
  ```

+ __原因：__&#x200B;應用程式的OSGi套件（定義於`core`專案中）會從核心元件核心相依性匯入Java類別，且版本層級與部署至AEM as a Cloud Service的版本層級不同。
+ __解析度：__
   + 使用Git時，請還原成核心元件版本增加之前存在的工作認可。 將此認可推送至Cloud Manager Git分支，並從此分支執行環境更新。 這會將AEM as a Cloud Service升級至最新的AEM版本，其中包含較新的核心元件版本。 AEM as a Cloud Service更新至最新的AEM版本後（其中包含最新的核心元件版本），請重新部署原本失敗的程式碼。
   + 若要在本機重現此問題，請確定AEM SDK版本與AEM as a Cloud Service環境使用的AEM發行版本相同。


### 建立Adobe支援案例

如果上述的疑難排解方法皆無法解決問題，請透過以下方式建立Adobe支援案例：

+ [Adobe Admin Console](https://adminconsole.adobe.com) >支援標籤>建立案例

  _如果您是多個Adobe組織的成員，在建立案例之前，請先確定已在Adobe組織切換器中選取管道失敗的Adobe組織。_

## 部署至

「部署至」步驟負責取得在建置影像中產生的程式碼成品，並使用它啟動新的AEM Author和Publish服務，在成功後移除任何舊的AEM Author和Publish服務。 在此步驟中也會安裝及更新可變內容套件和索引。

在偵錯部署至步驟之前，請熟悉[AEM as a Cloud Service記錄](./logs.md)。 `aemerror`記錄檔包含啟動和關閉Pod的相關資訊，這些資訊可能與「部署到問題」相關。 請注意，可透過Cloud Manager「部署至」步驟中的「下載記錄」按鈕使用的記錄檔不是`aemerror`記錄檔，而且不包含與您的應用程式啟動相關的詳細資訊。

![部署至](./assets/build-and-deployment/deploy-to.png)

「部署至」步驟可能失敗的三個主要原因：

### Cloud Manager管道儲存舊的AEM版本

+ __原因：__ Cloud Manager管道儲存的AEM版本比目標環境部署的版本舊。 當重複使用管道並指向執行較新版本AEM的新環境時，可能會發生這種情況。 您可透過檢視環境的AEM版本是否大於管道的AEM版本來識別它。
  ![Cloud Manager管道儲存舊的AEM版本](./assets/build-and-deployment/deploy-to__pipeline-holds-old-aem-version.png)
+ __解析度：__
   + 如果目標環境有可用的更新，請從環境的動作中選取更新，然後重新執行組建。
   + 如果目標環境沒有可用的更新，這表示它執行的是最新版的AEM。 要解決此問題，請刪除管道並重新建立它。


### Cloud Manager逾時

在新部署的AEM服務啟動期間執行的程式碼需要很長時間，以至於Cloud Manager在部署完成之前就會逾時。 在這些情況下，部署最終可能會成功，甚至認為Cloud Manager狀態報告為失敗。

+ __原因：__&#x200B;自訂程式碼可能會執行作業，例如在OSGi套件組合或元件生命週期早期觸發的大型查詢或內容周遊，這會大幅延遲AEM的啟動時間。
+ __解決方法：__&#x200B;檢閱OSGi Bundle生命週期早期所執行程式碼的實作，並檢閱AEM Author和Publish服務失敗前後的`aemerror`記錄（記錄時間為GMT），如Cloud Manager所示，並尋找指出任何自訂記錄執行程式的記錄訊息。

### 不相容的程式碼或設定

大部分的程式碼和設定違規都會在組建的初期階段擷取，不過自訂程式碼或設定可能會與AEM as a Cloud Service不相容，並且一直未偵測到，直到在容器中執行為止。

+ __原因：__&#x200B;自訂程式碼可能會叫用冗長的作業，例如在OSGi套件組合或元件生命週期早期觸發的大型查詢或內容周遊，這會大幅延遲AEM的啟動時間。
+ __解決方法：__&#x200B;檢閱AEM Author和Publish服務在Cloud Manager所顯示失敗時間（以GMT記錄時間）前後的`aemerror`記錄。
   1. 檢閱記錄檔，瞭解自訂應用程式提供的Java類別所擲回的任何ERROR。 如果發現任何問題，請解決，推送修復的程式碼，然後重新建置管道。
   1. 檢閱記錄檔，以找出您在自訂應用程式中延伸/互動之AEM各方面所回報的錯誤，並加以調查；這些ERROR可能不會直接歸因於Java類別。 如果發現任何問題，請解決，推送修復的程式碼，然後重新建置管道。

### 在內容封裝中包含/var

`/var`是可變的，包含各種暫時性的執行階段內容。 在內容套件中包含`/var` (例如 `ui.content`)透過Cloud Manager部署可能會導致部署步驟失敗。

此問題很難識別，因為它不會導致初始部署失敗，只會在後續部署中失敗。 明顯的症狀包括：

+ 初始部署成功，但作為部署一部分的新增或變更的可變內容似乎不存在於AEM Publish服務中。
+ AEM Author中的內容啟用/停用功能已封鎖
+ 後續部署會在「部署至」步驟中失敗，而「部署至」步驟會在約60分鐘後失敗。

若要驗證此問題是失敗行為的原因：

1. 判斷至少有一個內容套件是部署的一部分，寫入至`/var`。
1. 驗證主要（粗體）散發佇列是否已在下列位置封鎖：
   + AEM作者>工具>部署>發佈
     ![已封鎖的散發佇列](./assets/build-and-deployment/deploy-to__var--distribution.png)
1. 後續部署失敗時，請使用「下載記錄」按鈕下載Cloud Manager的「部署到」記錄：

   ![下載部署至記錄檔](./assets/build-and-deployment/deploy-to__var--download-logs.png)

   ...並確認記錄陳述式之間約有60分鐘間隔：

   ```
   2020-01-01T01:01:02+0000 Begin deployment in aem-program-x-env-y-dev [CorrelationId: 1234]
   ```

   ...和……

   ```
   2020-01-01T02:04:10+0000 Failed deployment in aem-program-x-env-y-dev
   ```

   請注意，在報告為成功的初始部署中，此記錄不會包含這些指標，而只會在後續的失敗部署中包含。

+ __原因：__&#x200B;用於將內容套件部署至AEM Publish服務的AEM復寫服務使用者無法寫入AEM Publish上的`/var`。 這會導致將內容套件部署到AEM Publish服務時失敗。
+ __解決方法：__&#x200B;下列解決此問題的方式會依偏好設定順序列出：
   1. 如果不需要使用`/var`資源，請從部署為應用程式一部分的內容套件中移除`/var`下的任何資源。
   2. 若需要`/var`資源，請使用[repoinit](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit)定義節點結構。 Repoinit指令碼可以透過OSGi執行模式鎖定為AEM Author、AEM Publish或兩者。
   3. 如果`/var`資源僅在AEM Author上需要，且無法使用[repoinit](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit)建立合理的模型，請將它們移至分散式內容封裝，此封裝僅由[將其內嵌](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html#embeddeds)到AEM Author執行模式資料夾(`<target>/apps/example-packages/content/install.author</target>`)的`all`封裝中安裝在AEM Author上。
   4. 依照此[AdobeKB](https://helpx.adobe.com/in/experience-manager/kb/cm/cloudmanager-deploy-fails-due-to-sling-distribution-aem.html)的說明，提供適當的ACL給`sling-distribution-importer`服務使用者。

### 建立Adobe支援案例

如果上述的疑難排解方法皆無法解決問題，請透過以下方式建立Adobe支援案例：

+ [Adobe Admin Console](https://adminconsole.adobe.com) >支援標籤>建立案例

  _如果您是多個Adobe組織的成員，在建立案例之前，請先確定已在Adobe組織切換器中選取管道失敗的Adobe組織。_
