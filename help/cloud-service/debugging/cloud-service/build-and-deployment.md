---
title: 構建和部署
description: AdobeCloud Manager可方便代碼生成和部署到AEMas a Cloud Service。 在生成過程中的步驟中可能會發生故障，需要採取措施來解決這些故障。 本指南介紹了如何瞭解部署中的常見故障，以及如何最好地處理這些故障。
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
source-git-commit: 7a4585146b52d14f32645c6889c9c015e9991809
workflow-type: tm+mt
source-wordcount: '2524'
ht-degree: 0%

---

# 調試AEMas a Cloud Service生成和部署

AdobeCloud Manager可方便代碼生成和部署到AEMas a Cloud Service。 在生成過程中的步驟中可能會發生故障，需要採取措施來解決這些故障。 本指南介紹了如何瞭解部署中的常見故障，以及如何最好地處理這些故障。

![雲管理生成管道](./assets/build-and-deployment/build-pipeline.png)

## 驗證

驗證步驟只是確保基本的雲管理器配置有效。 常見驗證失敗包括：

### 環境處於無效狀態

+ __錯誤消息：__ 環境處於無效狀態。
   ![環境處於無效狀態](./assets/build-and-deployment/validation__invalid-state.png)
+ __原因：__ 管道的目標環境處於過渡狀態，此時它無法接受新生成。
+ __解決方案：__ 等待狀態解析為正在運行（或更新可用）狀態。 如果正在刪除環境，請重新建立該環境，或選擇要構建到的其他環境。

### 找不到與管道關聯的環境

+ __錯誤消息：__ 該環境被標籤為已刪除。
   ![該環境被標籤為已刪除](./assets/build-and-deployment/validation__environment-marked-as-deleted.png)
+ __原因：__ 已刪除配置為使用的管道的環境。
即使重新建立了同名的新環境，Cloud Manager也不會自動將管道重新關聯到該同名環境。
+ __解決方案：__ 編輯管道配置，然後重新選擇要部署到的環境。

### 找不到與管道關聯的Git分支

+ __錯誤消息：__ 無效的管道：XXXXX。 Reason=Branch=xxxx在儲存庫中找不到。
   ![無效的管道：XXXXX。 Reason=Branch=xxxx在儲存庫中找不到](./assets/build-and-deployment/validation__branch-not-found.png)
+ __原因：__ 已刪除管道配置為使用的Git分支。
+ __解決方案：__ 使用完全相同的名稱重新建立缺少的Git分支，或重新配置管道以從其他現有分支生成。

## 構建和單元測試

![構建和單元測試](./assets/build-and-deployment/build-and-unit-testing.png)

生成和單元測試階段執行Maven生成(`mvn clean package`)。

在此階段中發現的錯誤應可在本地構建項目，但有以下例外：

+ Maven依賴關係不可用於 [中馬文](https://search.maven.org/) 已使用，且包含依賴項的Maven儲存庫為：
   + 無法從雲管理器（如私有內部Maven儲存庫）訪問，或者Maven儲存庫需要身份驗證，並且提供了不正確的憑據。
   + 未在項目的 `pom.xml`。 請注意，由於Maven儲存庫會增加生成時間，因此不鼓勵使用它。
+ 設備test因計時問題而失敗。 當設備test對時間敏感時，可能會發生這種情況。 一個強勁指標正依賴 `.sleep(..)` test。
+ 使用不支援的Maven插件。

## 代碼掃描

![代碼掃描](./assets/build-and-deployment/code-scanning.png)

代碼掃描使用Java和特定於的最佳實踐的AEM混合執行靜態代碼分析。

如果代碼中存在嚴重安全漏洞，則代碼掃描會導致生成失敗。 可以覆蓋較小的違規，但建議修復這些違規。 請注意，代碼掃描不完善，可能導致 [誤報](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/test-results/overview-test-results.html#dealing-with-false-positives)。

要解決代碼掃描問題，請通過 **下載詳細資訊** 按鈕並查看所有條目。

有關詳細資訊，請AEM參閱特定規則，請參閱Cloud Manager文檔。 [自定義AEM特定代碼掃描規則](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/custom-code-quality-rules.html)。

## 生成映像

![生成映像](./assets/build-and-deployment/build-images.png)

生成映像負責將在「生成和單元測試」步驟中建立的生成代碼對象與「版本」組合AEM，以形成單個可部署的對象。

雖然在生成和單元測試期間發現了任何代碼生成和編譯問題，但在嘗試將自定義生成對象與版本組合時可能會發現配置或結構AEM問題。

### 重複的OSGi配置

當多個OSGi配置通過目標環境的運行模式解AEM析時，生成映像步驟將失敗，並出現以下錯誤：

```
[ERROR] Unable to convert content-package [/tmp/packages/enduser.all-1.0-SNAPSHOT.zip]: 
Configuration ‘com.example.ExampleComponent’ already defined in Feature Model ‘com.example.groupId:example.all:slingosgifeature:xxxxx:X.X’, 
set the ‘mergeConfigurations’ flag to ‘true’ if you want to merge multiple configurations with same PID
```

#### 原因1

+ __原因：__ 項AEM目的所有包，包含多個代碼包，並且由多個代碼包提供相同的OSGi配置，導致衝突，導致生成映像步驟無法決定應使用哪個，因此生成失敗。 請注意，這不適用於OSGi工廠配置，只要它們具有唯一的名稱。
+ __解決方案：__ 查看作為應用程式一部分部署的所有代碼包（包括任何包括的第三方代碼包）AEM，查找通過運行模式解析到目標環境的重複OSGi配置。 作為雲服務，不能提供錯誤消息的「將mergeConfigurations標誌設定為true」AEM指導，應忽略該指導。

#### 原因2

+ __原因：__ 項AEM目錯誤地兩次包含相同的代碼包，導致重複包中包含的任何OSGi配置。
+ __解決方案：__ 查看所有項目中嵌入的包的所有pom.xml，並確保它們具有 `filevault-package-maven-plugin` [配置](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html#cloud-manager-target) 設定為 `<cloudManagerTarget>none</cloudManagerTarget>`。

### 錯誤的重定點指令碼

重定位指令碼定義基線內容、用戶、ACL等。 在AEMas a Cloud Service中，在生成映像期間應用重新點擊指令碼，但AEM是在SDK的本地快速啟動上，在激活OSGi重新點擊出廠配置時應用這些指令碼。 因此， Repoinit指令碼可能會在AEMSDK的本地快速啟動上悄悄地失敗（通過日誌記錄），但會導致生成映像步驟失敗，從而停止部署。

+ __原因：__ 重點指令碼格式不正確。 請注意，這可能會使儲存庫處於不完整狀態，因為在對儲存庫執行失敗指令碼後，任何重新點分指令碼都會處於不完整狀態。
+ __解決方案：__ 在部署AEM重點指令碼OSGi配置時查看SDK的本地快速啟動，以確定錯誤是否和錯誤。

### 未滿足的重定位內容依賴項

重定位指令碼定義基線內容、用戶、ACL等。 在AEMSDK的本地快速啟動中，當激活重新指向OSGi工廠配置時，或者換句話說，在儲存庫處於活動狀態且可能直接或通過內容包發生了內容更改後，將應用重新指向指令碼。 在AEMas a Cloud Service中，在生成映像期間，對可能不包含重新點擊指令碼所依賴內容的儲存庫應用重新點擊指令碼。

+ __原因：__ 重新點擊指令碼依賴於不存在的內容。
+ __解決方案：__ 確保重新點擊指令碼所依賴的內容存在。 通常，這表示定義不充分的重新點分指令碼缺少定義這些缺少但必需的內容結構的指令。 這可以通過刪除、解AEM包Jar和將包含重新點分指令碼的重新點分OSGi配置添加到安裝資料夾並啟動來在本地複製AEM。 錯誤將自身出現在AEMSDK本地快速啟動的error.log中。


### 應用程式的核心元件版本大於部署的版本

_此問題僅影響不自動更新到最新版本的非生產環AEM境。_

AEMas a Cloud Service在每個版本中自動包括最新的核心元件AEM版本，這意味AEM在as a Cloud Service環境被自動或手動更新後，將最新的核心元件版本部署到其中。

在以下情況下，生成映像步驟可能會失敗：

+ 部署應用程式將更新中核心元件的依賴關係版本 `core` （OSGi捆綁）項目
+ 然後，部署應用程式將部署到沙盒（非生產）AEMas a Cloud Service環境，該環境尚未更新為使用包含該新核AEM心元件版本的版本。

為防止出現此故障，AEM只要as a Cloud Service環境的更新可用，就將更新作為下一生成/部署的一部分，並始終確保在應用程式碼庫中增加核心元件版本後包括更新。

+ __症狀：__
生成映像步驟失敗，錯誤報告 
`com.adobe.cq.wcm.core.components...` 無法由 `core` 項目。

   ```
   [ERROR] Bundle com.example.core:0.0.3-SNAPSHOT is importing package(s) Package com.adobe.cq.wcm.core.components.models;version=[12.13,13) in start level 20 but no bundle is exporting these for that start level in the required version range.
   [ERROR] Analyser detected errors on feature 'com.adobe.granite:aem-ethos-app-image:slingosgifeature:aem-runtime-application-publish-dev:1.0.0-SNAPSHOT'. See log output for error messages.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   ```

+ __原因：__  應用程式的OSGi捆綁包(在 `core` project)從核心元件核心依賴關係中導入Java類，其版本級別與部署到as a Cloud Service的版本級別AEM不同。
+ __解析度:__
   + 使用Git，還原到核心元件版本增量之前存在的工作提交。 將此提交推送到Cloud Manager Git分支，並從此分支執行環境更新。 這將將AEMas a Cloud Service升級到最AEM新版本，其中將包括更新的核心元件版本。 將AEMas a Cloud Service更新為最新AEM版本（將具有最新核心元件版本）後，重新部署最初失敗的代碼。
   + 要在本地再現此問題，AEM請確保SDK版本與as a Cloud Service環境AEM所使用的AEM版本相同。


### 建立Adobe支援案例

如果上述故障排除方法無法解決問題，請通過以下方式建立Adobe支援案例：

+ [Adobe Admin Console](https://adminconsole.adobe.com) >支援頁籤>建立案例

   _如果您是多個Adobe組織的成員，請確保在建立案例之前，在Adobe組織切換器中選擇了管道失敗的Adobe組織。_

## 部署到

「部署到」步驟負責獲取生成映像中生成的代碼對象，使用它啟動新的AEM作者和發佈服務，並在成功後刪除任何舊的AEM作者和發佈服務。 此步驟中還安裝和更新了可變內容包和索引。

熟悉 [AEMas a Cloud Service日誌](./logs.md) 在調試「部署到」步驟之前。 的 `aemerror` 日誌包含有關啟動和關閉POD的資訊，這些資訊可能與「部署到問題」相關。 請注意，通過Cloud Manager的「部署到」步驟中的「下載日誌」按鈕可用的日誌不是 `aemerror` 日誌，並且不包含有關應用程式啟動的詳細資訊。

![部署到](./assets/build-and-deployment/deploy-to.png)

「部署到」步驟可能失敗的三個主要原因：

### Cloud Manager管道保存舊版AEM本

+ __原因：__ Cloud Manager管道保存的版本比部署到AEM目標環境的版本舊。 當重新使用管線並指向運行更高版本的新環境時，可能會發生這AEM種情況。 可通過檢查環境版本是否大AEM於管道版本來識別AEM此值。
   ![Cloud Manager管道保存舊版AEM本](./assets/build-and-deployment/deploy-to__pipeline-holds-old-aem-version.png)
+ __解析度:__
   + 如果目標環境具有可用更新，請從環境的操作中選擇更新，然後重新運行生成。
   + 如果目標環境沒有可用的更新，則表示它正在運行的最新版本AEM。 要解決此問題，請刪除管線並重新建立它。


### 雲管理器超時

在新部署的服務啟動期間運行的代AEM碼花費的時間太長，以致Cloud Manager在部署完成之前超時。 在這些情況下，即使認為Cloud Manager狀態報告失敗，部署也可能最終成功。

+ __原因：__ 自定義代碼可以執行在OSGi捆綁包或元件生命週期早期觸發的操作，如大型查詢或內容遍歷，這會顯著延遲的啟動時AEM間。
+ __解決方案：__ 查看OSGi捆綁包生命週期早期運行的代碼的實施，並查看 `aemerror` AEM作者和發佈服務的日誌（以GMT為單位），如雲管理器所示，並查找指示任何自定義日誌運行進程的日誌消息。

### 代碼或配置不相容

大多數代碼和配置違規都在生成中較早捕獲，但是，自定義代碼或配置可能與as a Cloud Service不相容AEM，並且在容器中執行之前未被檢測到。

+ __原因：__ 自定義代碼可以調用長時間的操作，如在OSGi捆綁包或元件生命週期早期觸發的大量查詢或內容遍歷，這會顯著延遲啟動時AEM間。
+ __解決方案：__ 查看 `aemerror` AEM作者和發佈服務的日誌（以GMT為日誌），如雲管理器所示。
   1. 查看自定義應用程式提供的Java類引發的任何ERRORS的日誌。 如果發現任何問題，請解決、推送固定代碼並重新生成管道。
   1. 查看日誌，查看您在自定義應用程式中擴展/AEM與之交互的方面報告的任何錯誤，並調查這些錯誤；這些錯誤可能不直接歸因於Java類。 如果發現任何問題，請解決、推送固定代碼並重新生成管道。

### 在內容包中包括/var

`/var` 是可變的，包含各種瞬時運行時內容。 包括 `/var` 內容包(例如 `ui.content`)部署到雲管理器可能導致「部署」步驟失敗。

此問題難以識別，因為它不會導致初始部署失敗，只會導致後續部署失敗。 明顯的症狀包括：

+ 初始部署成功，但是作為部署一部分的新內容或更改內容似乎不存在於AEM發佈服務上。
+ 阻止激活/取消激活AEM作者中的內容
+ 後續部署在「部署到步驟」中失敗，「部署到步驟」在大約60分鐘後失敗。

驗證此問題是導致行為失敗的原因：

1. 確定至少一個作為部署一部分的內容包寫入 `/var`。
1. 驗證主分發隊列是否在以下位置被阻止：
   + AEM作者>工具>部署>分發
      ![阻止的分發隊列](./assets/build-and-deployment/deploy-to__var--distribution.png)
1. 如果後續部署失敗，請使用「下載日誌」按鈕下載Cloud Manager的「部署到」日誌：

   ![下載部署到日誌](./assets/build-and-deployment/deploy-to__var--download-logs.png)

   ...並驗證日誌語句之間大約有60分鐘：

   ```
   2020-01-01T01:01:02+0000 Begin deployment in aem-program-x-env-y-dev [CorrelationId: 1234]
   ```

   ... 和 ...

   ```
   2020-01-01T02:04:10+0000 Failed deployment in aem-program-x-env-y-dev
   ```

   請注意，此日誌在報告成功的初始部署中不包含這些指示符，而只包含在後續失敗的部署中。

+ __原因：__ 用於將AEM內容包部署到AEM發佈服務的複製服務用戶無法寫入 `/var` 在AEM發佈上。 這將導致將內容包部署到AEM發佈服務失敗。
+ __解決方案：__ 以下解決此問題的方法按優先順序列出：
   1. 如果 `/var` 資源不必刪除 `/var` 從作為應用程式一部分部署的內容包中。
   2. 如果 `/var` 資源是必需的，請使用 [重點](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit)。 通過OSGi運行模式，可以將重點指令碼定向到AEM作者、AEM發佈或兩者。
   3. 如果 `/var` 資源僅對作者AEM需要，不能使用 [重點](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit)，將其移動到僅通過安裝在AEM Author上的離散內容包 [嵌入](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html#embeddeds) 在 `all` AEM Author運行模式資料夾中的包(`<target>/apps/example-packages/content/install.author</target>`)。
   4. 為 `sling-distribution-importer` 服務用戶，如本文所述 [AdobeKB](https://helpx.adobe.com/in/experience-manager/kb/cm/cloudmanager-deploy-fails-due-to-sling-distribution-aem.html)。

### 建立Adobe支援案例

如果上述故障排除方法無法解決問題，請通過以下方式建立Adobe支援案例：

+ [Adobe Admin Console](https://adminconsole.adobe.com) >支援頁籤>建立案例

   _如果您是多個Adobe組織的成員，請確保在建立案例之前，在Adobe組織切換器中選擇了管道失敗的Adobe組織。_
