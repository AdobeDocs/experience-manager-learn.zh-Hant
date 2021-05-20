---
title: 在AEM中開發專案
description: 說明如何為AEM專案開發的開發教學課程。  在本教學課程中，我們將建立自訂專案範本，可用來在AEM中建立新專案，以管理內容製作工作流程和工作。
version: 6.3, 6.4, 6.5
feature: 專案，工作流程
topics: collaboration, development, governance
activity: develop
audience: developer, implementer, administrator
doc-type: tutorial
topic: 開發
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '4652'
ht-degree: 0%

---


# 在AEM中開發專案

本開發教學課程說明如何為[!DNL AEM Projects]開發。  在本教學課程中，我們將建立自訂專案範本，可用來在AEM中建立新專案，以管理內容製作工作流程和工作。

>[!VIDEO](https://video.tv.adobe.com/v/16904/?quality=12&learn=on)

*本影片提供以下教學課程中建立的已完成工作流程的簡短示範。*

## 簡介 {#introduction}

[[!DNL AEM Projects]](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html) 是AEM的一項功能，可讓您在AEM Sites或資產實作中，輕鬆管理及分組所有與內容建立相關的工作流程和工作。

AEM專案隨附數個[OOTB專案範本](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#ProjectTemplates)。 建立新專案時，作者可從這些可用的範本中選擇。 具有獨特業務需求的大型AEM實作項目將需要建立自訂專案範本，並量身打造以符合其需求。 透過建立自訂專案範本，開發人員可以設定專案控制面板、連結至自訂工作流程，以及為專案建立其他業務角色。 我們將查看專案範本的結構，並建立範例範本。

![自訂專案卡](./assets/develop-aem-projects/custom-project-card.png)

## 設定

本教學課程將逐步說明建立自訂專案範本所需的程式碼。 您可以下載[附加的套件](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)並安裝到本機環境，以遵循本教學課程。 您也可以存取托管於[GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)的完整Maven專案。

* [已完成的教學課程套件](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [GitHub上的完整程式碼存放庫](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)

本教學課程假設您具備[AEM開發實務](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/the-basics.html)的一些基本知識，以及對[AEM Maven專案設定](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ht-projects-maven.html)的一些熟悉程度。 所提及的所有程式碼都打算作為參考使用，且應僅部署至[本機開發AEM例項](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingStarted)。

## 專案範本的結構

專案範本應置於原始碼控制之下，且應位於/apps下的應用程式資料夾下方。 理想情況下，應將它們放置在命名慣例為&#x200B;***/projects/templates/**&lt;my-template>的子資料夾中。 遵照此命名慣例，任何新的自訂範本將在建立專案時自動開放供作者使用。 可用項目模板的配置設定如下：**/content/projects/jcr:content**&#x200B;節點，由&#x200B;**cq:allowedTemplates**&#x200B;屬性設定。 依預設，這是規則運算式：**/(apps|libs)/。*/projects/templates/.***

專案範本的根節點將具有&#x200B;**jcr:primaryType**，其值為&#x200B;**cq:Template**。 根節點下方有3個節點：**gadgets**、**角色**&#x200B;和&#x200B;**工作流**。 這些節點都是&#x200B;**nt:unstructured**。 根節點下方也可以是thumbnail.png檔案，在「建立專案」精靈中選取範本時，該檔案會顯示。

完整節點結構：

```shell
/apps/<my-app>
    + projects (nt:folder)
         + templates (nt:folder)
              + <project-template-root> (cq:Template)
                   + gadgets (nt:unstructured)
                   + roles (nt:unstructured)
                   + workflows (nt:unstructured)
```

### 專案範本根

專案範本的根節點類型為&#x200B;**cq:Template**。 在此節點上，可以配置屬性&#x200B;**jcr:title**&#x200B;和&#x200B;**jcr:description**，這些屬性將顯示在「建立項目嚮導」中。 還有一個名為&#x200B;**wizard**&#x200B;的屬性，指向將填充項目屬性的表單。 預設值為：**/libs/cq/core/content/projects/wizard/steps/defaultproject.html**&#x200B;在大多數情況下都可正常運作，因為它可讓使用者填入基本的專案屬性並新增群組成員。

**請注意，「建立專案精靈」不使用SlingPOSTServlet。而是將值發佈至自訂Servlet:**com.adobe.cq.projects.impl.servlet.ProjectServlet**。 新增自訂欄位時，應考量此事項。*

可以找到翻譯專案範本的自訂精靈範例：**/libs/cq/core/content/projects/wizard/translationproject/defaultproject**。

### 小工具 {#gadgets}

此節點上沒有其他屬性，但小工具節點的子項控制建立新項目時，項目圖磚將填充項目的儀表板。 [專案圖磚](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#ProjectTiles) （也稱為小工具或Pod）是填入專案工作區的簡單卡片。您可以在下方找到ootb圖磚的完整清單：**/libs/cq/gui/components/projects/admin/pod。 **專案擁有者一律可在專案建立後新增/移除圖磚。

### 角色 {#roles}

每個專案皆有3個[預設角色](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#UserRolesinaProject):**Obsertors**、**Editors**&#x200B;和&#x200B;**Owners**。 通過在角色節點下添加子節點，可以為模板添加其他特定於業務的項目角色。 然後，您可以將這些角色連結至與專案相關聯的特定工作流程。

### 工作流程 {#workflows}

建立自訂專案範本最誘人的原因之一，是它可讓您設定可與專案搭配使用的可用工作流程。 這些工作流程可以是OOTB工作流程或自訂工作流程。 在&#x200B;**workflows**&#x200B;節點下方，需要有&#x200B;**models**&#x200B;節點（也有`nt:unstructured`）和子節點在指定可用的工作流模型下。 屬性**modelId **指向/etc/workflow下的工作流模型，而屬性&#x200B;**wizard**&#x200B;指向啟動工作流時使用的對話框。 「專案」的一大優點，是能新增自訂對話方塊（精靈），以在工作流程開始時擷取業務特定中繼資料，進而推動工作流程中的進一步動作。

```shell
<projects-template-root> (cq:Template)
    + workflows (nt:unstructured)
         + models (nt:unstructured)
              + <workflow-model> (nt:unstructured)
                   - modelId = points to the workflow model
                   - wizard = dialog used to start the workflow
```

## 建立項目模板{#creating-project-template}

由於我們將主要複製/配置節點，因此我們將使用CRXDE Lite。 在您的本機AEM例項中開啟[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)。

1. 首先，在`/apps/&lt;your-app-folder&gt;`下建立名為`projects`的新資料夾。 在名為`templates`的下面建立另一個資料夾。

   ```shell
   /apps/aem-guides/projects-tasks/
                       + projects (nt:folder)
                                + templates (nt:folder)
   ```

1. 為了更輕鬆，我們將從現有的「簡單專案」範本開始使用自訂範本。

   1. 複製節點&#x200B;**/libs/cq/core/content/projects/templates/default**&#x200B;並貼到在步驟1中建立的&#x200B;*templates*&#x200B;資料夾下。

   ```shell
   /apps/aem-guides/projects-tasks/
                + templates (nt:folder)
                     + default (cq:Template)
   ```

1. 您現在應該有一個路徑，如&#x200B;**/apps/aem-guides/projects-tasks/projects/templates/authoring-project**。

   1. 將作者 — 專案節點的&#x200B;**jcr:title**&#x200B;和&#x200B;**jcr:description**&#x200B;屬性編輯為自訂標題和說明值。

      1. 將&#x200B;**嚮導**&#x200B;屬性保留為指向預設的Project屬性。

   ```shell
   /apps/aem-guides/projects-tasks/projects/
            + templates (nt:folder)
                 + authoring-project (cq:Template)
                      - jcr:title = "Authoring Project"
                      - jcr:description = "A project to manage approval and publish process for AEM Sites or Assets"
                      - wizard = "/libs/cq/core/content/projects/wizard/steps/defaultproject.html"
   ```

1. 對於此項目模板，我們要使用任務。
   1. 在名為&#x200B;**tasks**&#x200B;的authoring-project/gadgets下添加新的&#x200B;**nt:unstructured**&#x200B;節點。
   1. 為&#x200B;**cardWeight** = &quot;100&quot;、**jcr:title**=&quot;Tasks&quot;和&#x200B;**sling:resourceType**=&quot;cq/gui/components/projects/admin/pod/taskpod&quot;的任務節點新增字串屬性。

   現在，當建立新專案時，預設會顯示[工作方塊](https://docs.adobe.com/docs/en/aem/6-3/author/projects.html#Tasks)。

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
            + team (nt:unstructured)
            + asset (nt:unstructured)
            + work (nt:unstructured)
            + experiences (nt:unstructured)
            + projectinfo (nt:unstructured)
            ..
            + tasks (nt:unstructured)
                 - cardWeight = "100"
                 - jcr:title = "Tasks"
                 - sling:resourceType = "cq/gui/components/projects/admin/pod/taskpod"
   ```

1. 我們將新增自訂核准者角色至專案範本。

   1. 在項目模板(authoring-project)節點下添加標有&#x200B;**角色**&#x200B;的新&#x200B;**nt:unstructured**&#x200B;節點。
   1. 添加另一個&#x200B;**nt:unstructured**&#x200B;節點，標籤為「批准者」的節點是角色節點的子節點。
   1. 添加字串屬性&#x200B;**jcr:title** = &quot;**Approvers**&quot;, **roleclass** =&quot;**owner**&quot;, **roleid**=&quot;**a11/>&quot;。**
      1. 批准者節點的名稱，以及jcr:title和roleid可以是任何字串值（只要roleid是唯一的）。
      1. **** roleclass根據3個OOTB角色 [](https://docs.adobe.com/docs/en/aem/6-3/author/projects.html#User專案中的角色)控管套用該角色的權限： **擁有者**、 **編輯者**&#x200B;和觀 **察者**。
      1. 一般而言，如果自訂角色更像管理角色，則角色類可以是&#x200B;**所有者；**&#x200B;如果它是更具體的創作角色，如Photogreher或Designer，則&#x200B;**editor**&#x200B;角色類就足夠了。 **owner**&#x200B;和&#x200B;**editor**&#x200B;之間的最大差異是項目所有者可以更新項目屬性並向項目添加新用戶。

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
           + approvers (nt:unstructured)
                - jcr:title = "Approvers"
                - roleclass = "owner"
                - roleid = "approver"
   ```

1. 複製「簡單專案」範本後，您就能設定4個OOTB工作流程。 工作流程/模型下方的每個節點都指向特定工作流程，以及該工作流程的啟動對話方塊精靈。 在本教學課程的稍後部分，我們將為此專案建立自訂工作流程。 目前，請刪除工作流程/模型下方的節點：

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
            + models (nt:unstructured)
               - (remove ootb models)
   ```

1. 為了讓內容作者輕鬆識別專案範本，您可以新增自訂縮圖。 建議的大小為319x319像素。
   1. 在「CRXDE Lite」中，建立新檔案，作為小工具、角色和工作流節點的同級節點，該節點名為&#x200B;**thumbnail.png**。
   1. 儲存，然後導覽至`jcr:content`節點，並連按兩下`jcr:data`屬性（避免按一下「檢視」）。
      1. 這應該會以編輯`jcr:data`檔案對話方塊提示您，而且您可以上傳自訂縮圖。

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
       + thumbnail.png (nt:file)
   ```

已完成項目模板的XML表示：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
    jcr:description="A project to manage approval and publish process for AEM Sites or Assets"
    jcr:primaryType="cq:Template"
    jcr:title="Authoring Project"
    ranking="{Long}1"
    wizard="/libs/cq/core/content/projects/wizard/steps/defaultproject.html">
    <jcr:content
        jcr:primaryType="nt:unstructured"
        detailsHref="/projects/details.html"/>
    <gadgets jcr:primaryType="nt:unstructured">
        <team
            jcr:primaryType="nt:unstructured"
            jcr:title="Team"
            sling:resourceType="cq/gui/components/projects/admin/pod/teampod"
            cardWeight="60"/>
        <tasks
            jcr:primaryType="nt:unstructured"
            jcr:title="Tasks"
            sling:resourceType="cq/gui/components/projects/admin/pod/taskpod"
            cardWeight="100"/>
        <work
            jcr:primaryType="nt:unstructured"
            jcr:title="Workflows"
            sling:resourceType="cq/gui/components/projects/admin/pod/workpod"
            cardWeight="80"/>
        <experiences
            jcr:primaryType="nt:unstructured"
            jcr:title="Experiences"
            sling:resourceType="cq/gui/components/projects/admin/pod/channelpod"
            cardWeight="90"/>
        <projectinfo
            jcr:primaryType="nt:unstructured"
            jcr:title="Project Info"
            sling:resourceType="cq/gui/components/projects/admin/pod/projectinfopod"
            cardWeight="100"/>
    </gadgets>
    <roles jcr:primaryType="nt:unstructured">
        <approvers
            jcr:primaryType="nt:unstructured"
            jcr:title="Approvers"
            roleclass="owner"
            roleid="approvers"/>
    </roles>
    <workflows
        jcr:primaryType="nt:unstructured"
        tags="[]">
        <models jcr:primaryType="nt:unstructured">
        </models>
    </workflows>
</jcr:root>
```

## 測試自訂專案範本

現在，我們可以建立新專案來測試專案範本。

1. 您應會將自訂範本視為專案建立選項之一。

   ![選擇模板](./assets/develop-aem-projects/choose-template.png)

1. 選取自訂範本後，按一下「下一步」，並注意在填入專案成員時，您可以將他們新增為核准者角色。

   ![批准](./assets/develop-aem-projects/user-approver.png)

1. 按一下「建立」以完成根據自訂範本建立專案。 您會在「項目儀表板」上注意到，小工具下配置的任務表徵圖和其他表徵圖會自動顯示。

   ![任務表徵圖](./assets/develop-aem-projects/tasks-tile.png)


## 為什麼選擇工作流程？

傳統上，以核准程式為中心的AEM工作流程都使用參與者工作流程步驟。 AEM收件匣包含工作和工作流程的詳細資訊，並增強與AEM專案的整合。 這些功能使使用「項目建立任務」流程步驟更具吸引力。

### 為什麼要做任務？

將任務建立步驟用於傳統參與者步驟提供了以下幾項優勢：

* **開始日期和到期日**  — 新的日曆功能可讓作者輕鬆管理其時間，並運用這些日期。
* **優先順序**  — 內建的低、正常和高優先順序，讓作者可排定工作的優先順序
* **串接注釋**  — 當作者處理某項任務時，他們可留下注釋，以增加協作
* **可見性**  — 透過「專案」將任務圖磚和檢視加以顯示，讓管理員可檢視逗留時間的方式
* **專案整合**  — 工作已與專案角色和控制面板整合

與參與者步驟一樣，任務也可以動態分配和路由。 任務元資料（如標題、優先順序）也可以根據先前的動作動態設定，如下列教學課程所示。

雖然任務比參與者步驟具有一些優勢，但它們確實會帶來額外的開銷，在項目之外沒有這樣的用處。 此外，必須使用ecma指令碼對Task的所有動態行為進行編碼，這些指令碼有其自身的限制。

## 使用案例要求範例{#goals-tutorial}

![工作流程程式圖](./assets/develop-aem-projects/workflow-process-diagram.png)

上圖概述範例核准工作流程的高階需求。

第一步是建立任務以完成內容的編輯。 我們將允許工作流啟動器選擇此第一個任務的受託人。

完成第一項任務後，受託人將有三個用於傳送工作流的選項：

**正常** — 正常工藝路線將建立分配給項目審批人組以複查和審批的任務。 任務的優先順序為「正常」，到期日為建立後的5天。

**快速**  — 快速工藝路線還會建立分配給項目審批者組的任務。任務的優先順序為「高」，到期日僅為1天。

**略過**  — 在此範例工作流程中，初始參與者有選項可略過核准群組。（是的，這可能會挫敗「批准」工作流的目的，但它允許我們說明其他路由功能）

核准者群組可以核准內容，或將內容傳回給初始受託人重新工作。 如果被發回以重新工作，則會建立新任務，並將其相應地標籤為「返回以重新工作」。

工作流程的最後一個步驟會使用ootb啟動頁面/資產處理步驟並複製裝載。

## 建立工作流模型

1. 從「AEM開始」菜單導航至「工具」 — >「工作流」 — >「模型」。 按一下右上角的「建立」，建立新的「工作流模型」。

   為新模型命名標題：「內容核准工作流程」和URL名稱：&quot;content-approval-workflow&quot;。

   ![工作流程建立對話方塊](./assets/develop-aem-projects/workflow-create-dialog.png)

   如需與[建立工作流程相關的詳細資訊，請參閱這裡](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/workflows-models.html)。

1. 最佳做法是將自訂工作流程分組在其位於/etc/workflow/models下方的資料夾中。 在「CRXDE Lite」中，在名為&#x200B;**&quot;aem-guides&quot;**&#x200B;的/etc/workflow/models下方建立新的&#x200B;**&#39;nt:folder&#39;**。 新增子資料夾可確保在升級或Service Pack安裝期間不會意外覆寫自訂工作流程。

   *請注意，絕不要將資料夾或自訂工作流程放在ootb子資料夾（例如/etc/workflow/models/dam或/etc/workflow/models/projects）下，因為整個子資料夾也可能會被升級或服務包覆蓋。

   ![工作流程模型在6.3中的位置](./assets/develop-aem-projects/custom-workflow-subfolder.png)

   工作流程模型在6.3中的位置

   >[!NOTE]
   >
   >如果使用AEM 6.4+，工作流程的位置已變更。 如需詳細資訊，請參閱[這裡。](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/workflows-best-practices.html#LocationsWorkflowModels)

   如果使用AEM 6.4+，則將在`/conf/global/settings/workflow/models`下建立工作流模型。 使用/conf目錄重複上述步驟，並添加名為`aem-guides`的子資料夾，並將`content-approval-workflow`移到其下。

   ![現代工作流定](./assets/develop-aem-projects/modern-workflow-definition-location.png)
義位置6.4+中工作流模型的位置

1. AEM 6.3推出了將工作流程階段新增至指定工作流程的功能。 這些階段將從「工作流資訊」(Workflow Info)頁簽的「收件箱」(Inbox)中對用戶顯示。 它會向使用者顯示工作流程中的目前階段，以及其前後的階段。

   要配置階段，請從SideKick開啟「頁面屬性」對話框。 第四個標籤標籤為「階段」。 新增下列值，以設定此工作流程的三個階段：

   1. 編輯內容
   1. 批准
   1. 發佈

   ![工作流階段配置](./assets/develop-aem-projects/workflow-model-stage-properties.png)

   從「頁面屬性」對話方塊設定工作流程階段。

   ![工作流程進度列](./assets/develop-aem-projects/workflow-info-progress.png)

   工作流程進度列，如AEM收件匣中所示。

   您可以選擇將&#x200B;**影像**&#x200B;上傳至頁面屬性，以在使用者選取時作為工作流程縮圖。 影像尺寸應為319x319像素。 當使用者前往選取工作流程時，也會將&#x200B;**Description**&#x200B;新增至頁面屬性。

1. 「建立項目任務」工作流流程旨在作為工作流中的步驟建立任務。 工作流程只有在完成工作後才會前進。 「建立項目任務」步驟的一個功能強大的方面是，它可以讀取工作流元資料值並使用這些值動態建立任務。

   首先刪除預設建立的參與者步驟。 從「元件」功能表的「側腳」中，展開&#x200B;**&quot;Projects&quot;**&#x200B;子標題，並拖放&#x200B;**&quot;Create Project Task&quot;**&#x200B;至模型。

   按兩下「建立項目任務」步驟以開啟工作流對話框。 設定下列屬性：

   此索引標籤在所有工作流程處理步驟中都很常見，我們會設定「標題」和「說明」（這些將不會顯示給一般使用者）。 我們將設定的重要屬性是下拉式功能表中的「工作流程階段」至&#x200B;**「編輯內容」**。

   ```shell
   Common Tab
   -----------------
       Title = "Start Task Creation"
       Description = "This the first task in the Workflow"
       Workflow Stage = "Edit Content"
   ```

   「建立項目任務」工作流流程旨在作為工作流中的步驟建立任務。 「任務」(Task)頁簽允許我們設定任務的所有值。 在本例中，我們希望受託人是動態的，因此我們將其保留為空白。 其餘的屬性值：

   ```shell
   Task Tab
   -----------------
       Name* = "Edit Content"
       Task Priority = "Medium"
       Description = "Edit the content and finalize for approval. Once finished submit for approval."
       Due In - Days = "2"
   ```

   路由頁簽是一個可選對話框，它可以指定完成任務的用戶的可用操作。 這些動作只是字串值，會儲存至工作流程的中繼資料。 這些值可由指令碼讀取，並/或稍後在工作流程中處理步驟，以動態地「路由」工作流程。 我們將根據[工作流程目標](#goals-tutorial)將三個動作新增至此索引標籤：

   ```shell
   Routing Tab
   -----------------
       Actions =
           "Normal Approval"
           "Rush Approval"
           "Bypass Approval"
   ```

   此頁簽允許我們配置「預建任務指令碼」，在該指令碼建立之前，我們可以利用寫程式方式決定任務的各種值。 我們可以選擇將指令碼指向外部檔案或直接在對話框中嵌入短指令碼。 在本例中，我們將將「預建任務指令碼」指向外部檔案。 在步驟5中，我們將建立該指令碼。

   ```shell
   Advanced Settings Tab
   -----------------
      Pre-Create Task Script = "/apps/aem-guides/projects/scripts/start-task-config.ecma"
   ```

1. 在上一步中，我們引用了預建任務指令碼。 我們現在將建立該指令碼，在該指令碼中，我們將根據工作流元資料值&quot;**assignee**&quot;的值設定任務的受託人。 當工作流程啟動時，將設定&#x200B;**&quot;受託人&quot;**&#x200B;值。 我們還將讀取工作流元資料，以通過讀取工作流元資料的「**taskPriority」**&#x200B;值以及在第一個任務到期時動態設定的**&quot;taskDueDate&quot; **來動態選擇任務的優先順序。

   為了組織目的，我們已在應用程式資料夾下方建立資料夾，以存放所有與專案相關的指令碼：**/apps/aem-guides/projects-tasks/projects/scripts**。 在此資料夾下建立名為&#x200B;**&quot;start-task-config.ecma&quot;**&#x200B;的新檔案。 *請注意，請確定您start-task-config.ecma檔案的路徑符合步驟4的「進階設定」標籤中設定的路徑。

   將下列項目新增為檔案的內容：

   ```
   // start-task-config.ecma
   // Populate the task using values stored as workflow metadata originally posted by the start workflow wizard
   
   // set the assignee based on start workflow wizard
   var assignee = workflowData.getMetaDataMap().get("assignee", Packages.java.lang.String);
   task.setCurrentAssignee(assignee);
   
   //Set the due date for the initial task based on start workflow wizard
   var dueDate = workflowData.getMetaDataMap().get("taskDueDate", Packages.java.util.Date);
   if (dueDate != null) {
       task.setProperty("taskDueDate", dueDate);
   }
   
   //Set the priority based on start workflow wizard
   var taskPriority = workflowData.getMetaDataMap().get("taskPriority", "Medium");
   task.setProperty("taskPriority", taskPriority);
   ```

1. 導覽回「內容核准工作流程」。 拖放&#x200B;**Start Task**&#x200B;步驟下的&#x200B;**OR Split**&#x200B;元件（位於「Workflow」類別下的Sidekick中）。 在「常見」對話方塊中，選取3個分支的選項按鈕。 OR Split將讀取工作流元資料值&#x200B;**&quot;lastTaskAction&quot;**&#x200B;以確定工作流的路由。 **&quot;lastTaskAction&quot;**&#x200B;屬性將設定為步驟4中配置的「路由」頁簽中的值之一。 對於每個分支頁簽，請用以下值填寫&#x200B;**Script**&#x200B;文本區域：

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Normal Approval") {
       return true;
   }
   
   return false;
   }
   ```

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Rush Approval") {
       return true;
   }
   
   return false;
   }
   ```

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Bypass Approval") {
       return true;
   }
   
   return false;
   }
   ```

   *請注意，我們正在執行直接字串匹配以確定路由，因此，在分支指令碼中設定的值必須與在步驟4中設定的路由值相匹配。

1. 將另一個「**建立項目任務**」步驟拖放到模型的最左側（分支1），位於OR拆分下。 使用下列屬性填入對話方塊：

   ```
   Common Tab
   -----------------
       Title = "Approval Task Creation"
       Description = "Create a an approval task for Project Approvers. Priority is Medium."
       Workflow Stage = "Approval"
   
   Task Tab
   ------------
       Name* = "Approve Content for Publish"
       Task Priority = "Medium"
       Description = "Approve this content for publication."
       Days = "5"
   
   Routing Tab - Actions
   ----------------------------
       "Approve and Publish"
       "Send Back for Revision"
   ```

   由於這是「正常審批」工藝路線，因此任務的優先順序設定為「中」。 此外，我們為「批准者」組提供5天時間來完成任務。 由於我們將在「進階設定」標籤中動態指派，因此「任務」標籤上的受託人會保留空白。 完成此任務時，我們為「批准者」組提供兩條可能的路由：**&quot;核准並發佈&quot;**&#x200B;如果他們核准內容且可發佈，而&#x200B;**&quot;傳回以供修訂&quot;**&#x200B;如果原始編輯器有需要修正的問題，則可以發佈。 核准者可以留下留言，讓原始編輯者查看工作流是否傳回給他/她。

在本教程的前面，我們建立了包含批准者角色的項目模板。 每次從此模板建立新項目時，都會為批准者角色建立特定於項目的組。 就像參與者步驟一樣，任務只能分配給用戶或組。 我們要將此任務分配給與批准者組對應的項目組。 從專案中啟動的所有工作流程都會有將專案角色對應至專案特定群組的中繼資料。

複製並貼上下列程式碼至**進階設定標籤的&#x200B;**Script**&#x200B;文字區**。 此代碼將讀取工作流元資料並將任務分配給項目的「批准者」組。 如果找不到批准者組值，則它將返回給將任務分配給管理員組。

```
var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");

task.setCurrentAssignee(projectApproverGrp);
```

1. 將另一個「**建立項目任務**」步驟拖放到模型中OR拆分下的中間分支（分支2）。 使用下列屬性填入對話方塊：

   ```
   Common Tab
   -----------------
       Title = "Rush Approval Task Creation"
       Description = "Create a an approval task for Project Approvers. Priority is High."
       Workflow Stage = "Approval"
   
   Task Tab
   ------------
       Name* = "Rush Approve Content for Publish"
       Task Priority = "High"
       Description = "Rush approve this content for publication."
       Days = "1"
   
   Routing Tab - Actions
   ----------------------------
       "Approve and Publish"
       "Send Back for Revision"
   ```

   由於這是緊急批准路線，因此任務的優先順序設定為高。 此外，我們僅為「批准者」組提供一天時間來完成任務。 由於我們將在「進階設定」標籤中動態指派，因此「任務」標籤上的受託人會保留空白。

   我們可以重新使用與步驟7中相同的指令碼片段，在** Advanced Settings **標籤上填入&#x200B;**Script**&#x200B;文字區域。 複製並貼上下列程式碼：

   ```
   var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");
   
   task.setCurrentAssignee(projectApproverGrp);
   ```

1. 拖放a**無操作**元件到最右邊的分支（分支3）。 無操作元件不執行任何操作，它將立即進行，表示原始編輯者希望繞過批准步驟。 技術上，我們可以不執行任何工作流程步驟而離開此分支，但作為最佳實務，我們將新增「無操作」步驟。 這向其他開發人員清楚說明Branch 3的用途。

   連按兩下工作流程步驟並設定「標題」和「說明」：

   ```
   Common Tab
   -----------------
       Title = "Bypass Approval"
       Description = "Placeholder step to indicate that the original editor decided to bypass the approver group."
   ```

   ![工作流模型或分割](./assets/develop-aem-projects/workflow-stage-after-orsplit.png)

   在OR分割中的所有三個分支皆已設定後，「工作流程模型」應該會如此。

1. 由於「批准者」組可以選擇將工作流發回原始編輯器以進行進一步修訂，因此我們將依靠&#x200B;**Goto**&#x200B;步驟來讀取最後執行的操作，並將工作流路由到起始處或讓其繼續。

   拖放Goto步驟元件（位於「工作流」下的Sidekick中）到OR split（重新聯接的位置）下。 按兩下並在對話方塊中設定下列屬性：

   ```
   Common Tab
   ----------------
       Title = "Goto Step"
       Description = "Based on the Approver groups action route the workflow to the beginning or continue and publish the payload."
   
   Process Tab
   ---------------
       The step to go to. = "Start Task Creation"
   ```

   我們將配置的最後一部分是「轉到」過程步驟中的指令碼。 指令碼值可透過對話方塊內嵌，或設定為指向外部檔案。 如果工作流應轉到指定的步驟，則轉到指令碼必須包含&#x200B;**函式check()**&#x200B;並返回true。 傳回錯誤結果，導致工作流程繼續進行。

   如果批准者組選擇&#x200B;**&quot;Send Back for Revision&quot;**&#x200B;動作（在步驟7和8中配置），則我們要將工作流返回到&#x200B;**&quot;Start Task Creation&quot;**&#x200B;步驟。

   在「流程」頁簽上，將以下代碼段添加到「指令碼」文本區域：

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Send Back for Revision") {
       return true;
   }
   
   return false;
   }
   ```

1. 若要發佈裝載，我們將使用ootb **啟動頁面/資產**&#x200B;處理步驟。 此程式步驟幾乎不需要設定，且會將工作流程的裝載新增至復寫佇列，以便啟用。 我們會在「轉至」步驟下方新增步驟，這樣只有在核准者群組已核准要發佈的內容，或原始編輯器選擇略過核准路由時，才能到達此步驟。

   拖放&#x200B;**啟動頁面/資產**&#x200B;處理步驟（位於模型中「轉至步驟」下的「WCM工作流程」下的Sidekick中）。

   ![工作流程模型完成](assets/develop-aem-projects/workflow-model-final.png)

   新增「前往」步驟和「啟動頁面/資產」步驟後，工作流程模型應該是什麼樣子。

1. 如果核准者群組傳回內容以供修訂，我們會讓原始編輯器知道。 我們可以通過動態更改任務建立屬性來完成此操作。 我們將關閉&#x200B;**&quot;Send Back for Revision&quot;**&#x200B;的lastActionTaked屬性值。 如果存在該值，我們將修改標題和說明，以指明此任務是內容被發回以供修訂的結果。 我們也會將優先順序更新為&#x200B;**&quot;High&quot;**，使它成為編輯器運作的第一個項目。 最後，我們將任務到期日設定為從工作流被返回以進行修訂的一天開始。

   將開始`start-task-config.ecma`指令碼（在步驟5中建立）替換為：

   ```
   // start-task-config.ecma
   // Populate the task using values stored as workflow metadata originally posted by the start workflow wizard
   
   // set the assignee based on start workflow wizard
   var assignee = workflowData.getMetaDataMap().get("assignee", Packages.java.lang.String);
   task.setCurrentAssignee(assignee);
   
   //Set the due date for the initial task based on start workflow wizard
   var dueDate = workflowData.getMetaDataMap().get("taskDueDate", Packages.java.util.Date);
   if (dueDate != null) {
       task.setProperty("taskDueDate", dueDate);
   }
   
   //Set the priority based on start workflow wizard
   var taskPriority = workflowData.getMetaDataMap().get("taskPriority", "Medium");
   task.setProperty("taskPriority", taskPriority);
   
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   //change the title and priority if the approver group sent back the content
   if(lastAction == "Send Back for Revision") {
     var taskName = "Review and Revise Content";
   
     //since the content was rejected we will set the priority to High for the revison task
     task.setProperty("taskPriority", "High"); 
   
     //set the Task name (displayed as the task title in the Inbox) 
     task.setProperty("name", taskName);
     task.setProperty("nameHierarchy", taskName);
   
     //set the due date of this task 1 day from current date
     var calDueDate = Packages.java.util.Calendar.getInstance();
     calDueDate.add(Packages.java.util.Calendar.DATE, 1);
     task.setProperty("taskDueDate", calDueDate.getTime());
   
   }
   ```

## 建立「啟動工作流」嚮導{#start-workflow-wizard}

從專案內啟動工作流程時，您必須指定精靈以啟動工作流程。 預設嚮導：`/libs/cq/core/content/projects/workflowwizards/default_workflow`允許用戶輸入要運行的工作流的「工作流標題」、「啟動注釋」和「裝載路徑」。 下列也提供其他幾個範例：`/libs/cq/core/content/projects/workflowwizards`。

建立自訂精靈的功能非常強大，因為您可以在工作流程開始之前收集重要資訊。 資料儲存為工作流元資料的一部分，工作流進程可讀取此內容並根據輸入的值動態更改行為。 我們將建立一個自定義嚮導，以根據啟動嚮導值動態分配工作流中的第一個任務。

1. 在CRXDE-Lite中，我們將在`/apps/aem-guides/projects-tasks/projects`資料夾下建立一個名為「wizards」的子資料夾。 從以下位置複製預設嚮導：`/libs/cq/core/content/projects/workflowwizards/default_workflow`在新建立的嚮導資料夾下，將其更名為&#x200B;**content-approval-start**。 完整路徑現在應為：`/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start`。

   預設嚮導是兩列嚮導，第一列顯示選定的工作流模型的「標題」、「說明」和「縮圖」。 第二欄包含「工作流程標題」、「開始註解」和「裝載路徑」的欄位。 精靈是標準觸控式UI表單，可使用標準[Granite UI表單元件](https://docs.adobe.com/docs/en/aem/6-5/develop/ref/granite-ui/api/jcr_root/libs/granite/ui/components/coral/foundation/form/index.html)來填入欄位。

   ![內容核准工作流程精靈](./assets/develop-aem-projects/content-approval-start-wizard.png)

1. 我們將在嚮導中添加一個附加欄位，該欄位將用於設定工作流中第一個任務的受託人(請參閱[建立工作流模型](#create-workflow-model):步驟5)。

   在`../content-approval-start/jcr:content/items/column2/items`下面建立名為&#x200B;**&quot;assign&quot;**&#x200B;的`nt:unstructured`類型的新節點。 我們將使用「專案使用者選擇器」元件（以[Granite使用者選擇器元件](https://docs.adobe.com/docs/en/aem/6-5/develop/ref/granite-ui/api/jcr_root/libs/granite/ui/components/coral/foundation/form/userpicker/index.html)為基礎）。 此表單欄位可讓您輕鬆將使用者和群組選取限制為僅限屬於目前專案的使用者。

   以下是&#x200B;**assign**&#x200B;節點的XML表示：

   ```xml
   <assign
       granite:class="js-cq-project-user-picker"
       jcr:primaryType="nt:unstructured"
       sling:resourceType="cq/gui/components/projects/admin/userpicker"
       fieldLabel="Assign To"
       hideServiceUsers="{Boolean}true"
       impersonatesOnly="{Boolean}true"
       showOnlyProjectMembers="{Boolean}true"
       name="assignee"
       projectPath="${param.project}"
       required="{Boolean}true"/>
   ```

1. 我們還將添加一個優先順序選擇欄位，該欄位將確定工作流中第一個任務的優先順序(請參閱[建立工作流模型](#create-workflow-model):步驟5)。

   在`/content-approval-start/jcr:content/items/column2/items`下面建立名為&#x200B;**priority**&#x200B;的`nt:unstructured`類型的新節點。 我們將使用[Granite UI選取元件](https://docs.adobe.com/docs/en/aem/6-2/develop/ref/granite-ui/api/jcr_root/libs/granite/ui/components/coral/foundation/form/select/index.html)來填入表單欄位。

   在&#x200B;**priority**&#x200B;節點下，我們將添加&#x200B;**nt:unstructured**&#x200B;的&#x200B;**items**&#x200B;節點。 在&#x200B;**items**&#x200B;節點下方添加3個節點，以填充「高」、「中」和「低」的選擇選項。 每個節點的類型為&#x200B;**nt:unstructured**，應該具有&#x200B;**text**&#x200B;和&#x200B;**value**&#x200B;屬性。 文字和值都應為相同值：

   1. 高
   1. 中
   1. 低

   對於「中」節點，添加一個名為&quot;**selected&quot;**&#x200B;的附加布爾屬性，其值設定為&#x200B;**true**。 這將確保選擇欄位中的預設值為「中」。

   以下是節點結構和屬性的XML表示：

   ```xml
   <priority
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/select"
       fieldLabel="Task Priority"
       name="taskPriority">
           <items jcr:primaryType="nt:unstructured">
               <high
                   jcr:primaryType="nt:unstructured"
                   text="High"
                   value="High"/>
               <medium
                   jcr:primaryType="nt:unstructured"
                   selected="{Boolean}true"
                   text="Medium"
                   value="Medium"/>
               <low
                   jcr:primaryType="nt:unstructured"
                   text="Low"
                   value="Low"/>
               </items>
   </priority>
   ```

1. 我們將允許工作流啟動器設定初始任務的到期日。 我們將使用[Granite UI DatePicker](https://docs.adobe.com/docs/en/aem/6-5/develop/ref/granite-ui/api/jcr_root/libs/granite/ui/components/coral/foundation/form/datepicker/index.html)表單欄位來擷取此輸入。 我們也將新增含有[TypeHint](https://sling.apache.org/documentation/bundles/manipulating-content-the-slingpostservlet-servlets-post.html#typehint)的隱藏欄位，以確保輸入儲存為JCR中的Date類型屬性。

   添加兩個&#x200B;**nt:unstructured**&#x200B;節點，其屬性如下所示：

   ```xml
   <duedate
       granite:rel="project-duedate"
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/datepicker"
       displayedFormat="YYYY-MM-DD HH:mm"
       fieldLabel="Due Date"
       minDate="today"
       name="taskDueDate"
       type="datetime"/>
   <duedatetypehint
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/hidden"
       name="taskDueDate@TypeHint"
       type="datetime"
       value="Calendar"/>
   ```

1. 您可以在[此處](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/projects-tasks-guide/ui.apps/src/main/content/jcr_root/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start/.content.xml)查看啟動嚮導對話框的完整代碼。

## 連接工作流和項目模板{#connecting-workflow-project}

我們最不需要做的就是確保可從其中一個專案中啟動工作流程模型。 為此，我們需要重新訪問在此系列第1部分中建立的項目模板。

「工作流配置」是「項目模板」的一個區域，它指定要與該項目一起使用的可用工作流。 配置還負責在啟動工作流時指定「啟動工作流嚮導」（我們在[前面的步驟中建立）](#start-workflow-wizard)。 專案範本的「工作流程」設定為「即時」，這表示更新工作流程設定將會影響已建立的新專案和使用範本的現有專案。

1. 在CRXDE-Lite中，導覽至先前於`/apps/aem-guides/projects-tasks/projects/templates/authoring-project/workflows/models`建立的製作專案範本。

   在模型節點下面，添加一個名為&#x200B;**contentapproval**&#x200B;的新節點，節點類型為&#x200B;**nt:unstructured**。 將下列屬性新增至節點：

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/etc/workflow/models/aem-guides/content-approval-workflow/jcr:content/model"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

   >[!NOTE]
   >
   >如果使用AEM 6.4，工作流程的位置已變更。 將`modelId`屬性指向`/var/workflow/models/aem-guides/content-approval-workflow`下運行時工作流模型的位置
   >
   >
   >請參閱[這裡以取得有關工作流程位置變更的詳細資訊。](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/workflows-best-practices.html#LocationsWorkflowModels)

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/var/workflow/models/aem-guides/content-approval-workflow"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

1. 將「內容核准」工作流程新增至「專案範本」後，應可從專案的「工作流程圖磚」中開始使用。 繼續，啟動，並運用我們建立的各種路線。

## 支援材料

* [下載已完成的教學課程套件](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [GitHub上的完整程式碼存放庫](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)
* [AEM專案檔案](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html)
