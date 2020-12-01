---
title: 在AEM中開發專案
description: 此開發教學課程說明如何針對AEM Projects進行開發。  在本教學課程中，我們將建立自訂的「專案」範本，可用來在AEM中建立新的「專案」，以管理內容製作工作流程和工作。
version: 6.3, 6.4, 6.5
feature: projects, workflow
topics: collaboration, development, governance
activity: develop
audience: developer, implementer, administrator
doc-type: tutorial
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '4649'
ht-degree: 0%

---


# 在AEM中開發專案

本開發教學課程說明如何針對[!DNL AEM Projects]進行開發。  在本教學課程中，我們將建立自訂的「專案」範本，可用來在AEM中建立新的「專案」，以管理內容製作工作流程和工作。

>[!VIDEO](https://video.tv.adobe.com/v/16904/?quality=12&learn=on)

*本影片提供在以下教學課程中建立的已完成工作流程的簡短示範。*

## 簡介 {#introduction}

[[!DNL AEM Projects]](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html) 是AEM的一項功能，可讓您更輕鬆地管理和群組所有與內容建立相關的工作流程和工作，做為AEM Sites或Assets實作的一部分。

AEM Projects隨附數個[OOTB專案範本](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#ProjectTemplates)。 建立新專案時，作者可從這些可用範本中選擇。 具有獨特商業需求的大型AEM實作將會想要建立自訂專案範本，以符合其需求。 建立自訂專案範本後，開發人員就可以設定專案儀表板、連結至自訂工作流程，以及為專案建立其他商業角色。 我們將檢視專案範本的結構，並建立範例範本。

![自訂專案卡](./assets/develop-aem-projects/custom-project-card.png)

## 設定

本教學課程將逐步執行建立自訂專案範本所需的程式碼。 您可以下載[附加的軟體包](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)並安裝到本地環境，以便隨附教程。 您也可以存取裝載在[GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)上的完整Maven專案。

* [完成的教學課程套件](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [GitHub上的完整程式碼存放庫](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)

本教學課程假設您對[AEM開發實務](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/the-basics.html)有一些基本知識，並熟悉[AEM Maven專案設定](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ht-projects-maven.html)。 所有提及的程式碼都會當做參考使用，且應僅部署至[本機開發AEM例項](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingStarted)。

## 專案範本的結構

專案範本應置於來源控制之下，且應位於/apps下的應用程式資料夾下方。 最理想的情況是，應將它們放置在具有&#x200B;***/projects/templates/**&lt;my-template>命名約定的子資料夾中。 依照此命名慣例，任何新的自訂範本都會在建立專案時自動可供作者使用。 可用項目模板的配置設定為：**/content/projects/jcr:content**&#x200B;節點，由&#x200B;**cq:allowedTemplates**&#x200B;屬性提供。 依預設，此為規則運算式：**/(apps|libs)/。*/projects/templates/.***

項目模板的根節點將具有&#x200B;**jcr:primaryType**&#x200B;的&#x200B;**cq:Template**。 在的根節點下面有3個節點：**gadgets**、**角色**&#x200B;和&#x200B;**工作流**。 這些節點都是&#x200B;**nt:antructured**。 根節點下方也可以是thumbnail.png檔案，在「建立專案」精靈中選取範本時，會顯示該檔案。

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

### 專案範本根目錄

項目模板的根節點將為&#x200B;**cq:Template**&#x200B;類型。 在此節點上，您可以配置屬性&#x200B;**jcr:title**&#x200B;和&#x200B;**jcr:description**，這些屬性將顯示在「建立項目嚮導」中。 還有一個名為&#x200B;**wizard**&#x200B;的屬性，它指向將填充項目屬性的表單。 預設值為：**/libs/cq/core/content/projects/wizard/steps/defaultproject.html**&#x200B;在大多數情況下都能正常運作，因為它可讓使用者填入基本的專案屬性並新增群組成員。

**請注意，Create Project Wizard不使用Sling POST servlet。值會張貼至自訂servlet:**com.adobe.cq.projects.impl.servlet.ProjectServlet**。 在新增自訂欄位時，應考量到這一點。*

可以為翻譯項目模板找到自定義嚮導的示例：**/libs/cq/core/content/projects/wizard/translationproject/defaultproject**。

### 小工具 {#gadgets}

此節點上沒有其他屬性，但小工具節點的子節點控制建立新項目時，項目表徵圖在項目儀表板中填充的項目。 [Project Tiles](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#ProjectTiles) （也稱為小工具或pod）是填入專案工作場所的簡單卡片。A full list of ootb tiles can be found under:**/libs/cq/gui/components/projects/admin/pod。 **專案擁有者在建立專案後，一律可以新增／移除圖格。

### 角色 {#roles}

每個項目有3個[預設角色](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#UserRolesinaProject):**觀察員**、**編輯**&#x200B;和&#x200B;**擁有者**。 通過在角色節點下添加子節點，可以為模板添加其他特定於業務的項目角色。 然後，您可以將這些角色關聯到與項目關聯的特定工作流。

### 工作流程 {#workflows}

建立自訂專案範本最吸引人的原因之一，就是它可讓您設定可用的工作流程，以便與專案搭配使用。 這些工作流程可以是OOTB工作流程或自訂工作流程。 在&#x200B;**工作流**&#x200B;節點下，需要有&#x200B;**模型**&#x200B;節點（也有`nt:unstructured`）和指定可用工作流模型下的子節點。 屬性**modelId **指向/etc/workflow下的工作流模型，屬性&#x200B;**wizard**&#x200B;指向啟動工作流時使用的對話框。 「專案」的一大優勢是能夠新增自訂對話方塊（精靈），以在工作流程開始時擷取特定商業中繼資料，以推動工作流程中的進一步動作。

```shell
<projects-template-root> (cq:Template)
    + workflows (nt:unstructured)
         + models (nt:unstructured)
              + <workflow-model> (nt:unstructured)
                   - modelId = points to the workflow model
                   - wizard = dialog used to start the workflow
```

## 建立項目模板{#creating-project-template}

由於我們將主要複製／配置節點，因此我們將使用CRXDE Lite。 在您的本機AEM例項中開啟[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)。

1. 首先，在`/apps/&lt;your-app-folder&gt;`下面建立一個名為`projects`的新資料夾。 在名稱為`templates`的下方建立另一個資料夾。

   ```shell
   /apps/aem-guides/projects-tasks/
                       + projects (nt:folder)
                                + templates (nt:folder)
   ```

1. 為了讓工作更輕鬆，我們將從現有的「簡單專案」範本開始自訂範本。

   1. 將節點&#x200B;**/libs/cq/core/content/projects/templates/default**&#x200B;複製並貼上到在步驟1中建立的&#x200B;*templates*&#x200B;資料夾下。

   ```shell
   /apps/aem-guides/projects-tasks/
                + templates (nt:folder)
                     + default (cq:Template)
   ```

1. 您現在應該有類似&#x200B;**/apps/aem-guides/projects-tasks/projects/templates/authoring-project**&#x200B;的路徑。

   1. 將author-project節點的&#x200B;**jcr:title**&#x200B;和&#x200B;**jcr:description**&#x200B;屬性編輯為自訂標題和說明值。

      1. 將&#x200B;**wizard**&#x200B;屬性保留為指向預設項目屬性。

   ```shell
   /apps/aem-guides/projects-tasks/projects/
            + templates (nt:folder)
                 + authoring-project (cq:Template)
                      - jcr:title = "Authoring Project"
                      - jcr:description = "A project to manage approval and publish process for AEM Sites or Assets"
                      - wizard = "/libs/cq/core/content/projects/wizard/steps/defaultproject.html"
   ```

1. 對於此項目模板，我們希望使用Tasks。
   1. 在名為&#x200B;**tasks**&#x200B;的authoring-project/gadgets下添加新&#x200B;**nt:unstructured**&#x200B;節點。
   1. 將字串屬性新增至&#x200B;**cardWeight** = &quot;100&quot;、**jcr:title**=&quot;Tasks&quot;和&#x200B;**sling:resourceType**=&quot;cq/gui/components/projects/admin/pod/taskpod&quot;的任務節點。

   現在，當建立新專案時，預設會顯示[Tasks tile](https://docs.adobe.com/docs/en/aem/6-3/author/projects.html#Tasks)。

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

1. 我們將在項目模板中添加一個自定義批准者角色。

   1. 在項目模板(authoring-project)節點下添加一個新的&#x200B;**nt:unstructured**&#x200B;節點，標有&#x200B;**角色**。
   1. 添加另一個&#x200B;**nt:unstructured**&#x200B;節點，將批准者標籤為角色節點的子節點。
   1. 新增字串屬性&#x200B;**jcr:title** = &quot;**Approvers**&quot;, **Roleclass**=&quot;**owner**&quot;, **roleid**=&quot;**approvers**&quot;&quot;。
      1. 批准者節點的名稱以及jcr:title和roleid可以是任何字串值（只要roleid是唯一的）。
      1. **** roleclass根據 [3個OOTB角色(https://docs.adobe.com/docs/en/aem/6-3/author/projects.html#User項目中的角色])管理該角色所應用的權限： **擁有者**、編輯 **者**，以及觀 **察者**。
      1. 一般而言，如果自訂角色更像管理角色，則角色類別可以是&#x200B;**擁有者；如果是更具特定的編寫角色（例如攝影師或設計人員），則** editor **角色類別應已足夠。****owner**&#x200B;和&#x200B;**editor**&#x200B;的最大區別在於，專案擁有者可以更新專案屬性並新增使用者至專案。

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
           + approvers (nt:unstructured)
                - jcr:title = "Approvers"
                - roleclass = "owner"
                - roleid = "approver"
   ```

1. 複製「簡單專案」範本後，您就會獲得4個OOTB工作流程的設定。 工作流／模型下的每個節點都指向特定工作流和該工作流的啟動對話框嚮導。 在本教學課程的稍後部分，我們將為此專案建立自訂工作流程。 現在，請刪除工作流／模型下的節點：

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
            + models (nt:unstructured)
               - (remove ootb models)
   ```

1. 為了讓內容作者輕鬆識別專案範本，您可以新增自訂縮圖。 建議的大小為319x319像素。
   1. 在CRXDE中，Lite會以小工具、角色和工作流程節點的同級形式建立新檔案，該節點名為&#x200B;**thumbnail.png**。
   1. 儲存並導覽至`jcr:content`節點，然後按兩下`jcr:data`屬性（避免按一下「檢視」）。
      1. 這應該會提示您編輯`jcr:data`檔案對話方塊，而您可以上傳自訂縮圖。

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
       + thumbnail.png (nt:file)
   ```

完成專案範本的XML表示：

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

現在，我們可以建立新的專案來測試我們的專案範本。

1. 您應將自訂範本視為專案建立的選項之一。

   ![選擇範本](./assets/develop-aem-projects/choose-template.png)

1. 選擇自訂範本後，按一下「下一步」，並注意在填入專案成員時，您可以將他們新增為核准者角色。

   ![批准](./assets/develop-aem-projects/user-approver.png)

1. 按一下「建立」，以根據自訂範本完成專案建立。 在「項目儀表板」上，您會注意到小工具下配置的任務表徵圖和其它表徵圖會自動顯示。

   ![任務表徵圖](./assets/develop-aem-projects/tasks-tile.png)


## 為何選擇工作流程？

傳統上，以核准程式為中心的AEM工作流程都會使用「參與者」工作流程步驟。 AEM的「收件匣」包含有關「工作」和「工作流程」的詳細資訊，並增強與AEM Projects的整合。 這些功能使使用「項目建立任務」流程步驟更有吸引力。

### 為什麼選擇任務？

使用任務建立步驟優於傳統的參與者步驟，提供以下兩項優勢：

* **開始和到期日** -讓作者輕鬆管理其時間，新的日曆功能會使用這些日期。
* **優先順序** -內建於低、正常和高的優先順序，讓作者可以排定工作的優先順序
* **線程化注釋** -當作者在工作時，他們可以留下注釋，以增加協作
* **可見性** -使用「專案」的任務圖格和檢視可讓管理員檢視逗留時間
* **項目整合** -任務已與項目角色和儀表板整合

與「參與者」步驟一樣，「任務」也可以動態指派和路由。 任務中繼資料（例如標題、優先順序）也可以根據先前的動作動態設定，就像我們在下列教學課程中看到的一樣。

雖然任務比參與者步驟有一些優勢，但它們確實會帶來額外的開銷，在項目外沒有這樣的用處。 此外，必須使用ecma指令碼對Task的所有動態行為進行編碼，這些指令碼具有自己的限制。

## 使用案例要求示例{#goals-tutorial}

![工作流程流程圖](./assets/develop-aem-projects/workflow-process-diagram.png)

上圖概述了我們範例核准工作流程的高階需求。

第一步是建立Task以完成對內容的編輯。 我們將允許工作流啟動器選擇第一個任務的受託人。

第一個任務完成後，受託人將有三個選項用於傳送工作流：

**正常**-正常路由建立分配給項目審批人組的任務，以進行審核和審批。 任務的優先順序為「正常」，到期日為建立後5天。

**快速** -快速傳送路由也會建立分配給項目審批人組的任務。任務的優先順序為「高」，到期日僅為1天。

**略過** -在此範例工作流程中，初始參與者可以選擇略過核准群組。（是的，這可能會挫敗「批准」工作流的目的，但它允許我們說明其他路由功能）

核准者群組可以核准內容，或將內容傳回給初始受託人，以便重新工作。 如果要返回以進行重新工作，則會建立新任務並相應地標籤為「返回以進行重新工作」。

工作流程的最後一個步驟會使用「啟動頁面／資產」程式的otb步驟並複製裝載。

## 建立工作流程模型

1. 從「AEM開始」選單導覽至「工具->工作流程->模型」。 按一下右上角的「建立」，以建立新的「工作流程模型」。

   為新模型提供標題：「內容核准工作流程」和URL名稱：「內容——核准——工作流程」。

   ![工作流建立對話框](./assets/develop-aem-projects/workflow-create-dialog.png)

   有關建立工作流的詳細資訊，請參閱此處](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/workflows-models.html)。[

1. 作為最佳實務，自訂工作流程應分組在/etc/workflow/models下方的自訂資料夾中。 在CRXDE Lite中，在名為&#x200B;**&quot;aem-guides&quot;**&#x200B;的/etc/workflow/models下方建立新的&#x200B;**&#39;nt:folder&#39;**。 新增子資料夾可確保自訂工作流程不會在升級或安裝Service Pack時意外覆寫。

   *請注意，請勿將資料夾或自訂工作流程置於ootb子資料夾（例如/etc/workflow/models/dam或/etc/workflow/models/projects）下方，因為整個子資料夾也可能會被升級或服務套件覆寫。

   ![6.3中工作流模型的位置](./assets/develop-aem-projects/custom-workflow-subfolder.png)

   6.3中工作流模型的位置

   >[!NOTE]
   >
   >如果使用AEM 6.4+,「工作流程」的位置已變更。 如需詳細資訊，請參閱[這裡。](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/workflows-best-practices.html#LocationsWorkflowModels)

   如果使用AEM 6.4+，將在`/conf/global/settings/workflow/models`下建立工作流程模型。 使用/conf目錄重複上述步驟，並添加名為`aem-guides`的子資料夾，並在其下移動`content-approval-workflow`。

   ![現代工作流定](./assets/develop-aem-projects/modern-workflow-definition-location.png)
義位置6.4+中工作流模型的位置

1. AEM 6.3中引進的功能是將「工作流程階段」新增至指定的工作流程。 用戶將從「工作流資訊」頁籤的「收件箱」中看到這些階段。 它將向用戶顯示工作流中的當前階段以及其前後階段。

   若要設定階段，請從SideKick開啟「頁面屬性」對話方塊。 第四個標籤標示為「階段」。 新增下列值，以設定此工作流程的三個階段：

   1. 編輯內容
   1. 批准
   1. 發佈

   ![工作流階段配置](./assets/develop-aem-projects/workflow-model-stage-properties.png)

   從「頁面屬性」對話方塊設定「工作流程階段」。

   ![工作流程進度列](./assets/develop-aem-projects/workflow-info-progress.png)

   工作流程進度列，如「AEM收件匣」中所示。

   （可選）您可以將&#x200B;**Image**&#x200B;上傳到頁面屬性，當使用者選取它時，這些屬性將用作工作流程縮圖。 影像尺寸應為319x319像素。 將&#x200B;**Description**&#x200B;新增至「頁面屬性」，也會在使用者開始選取工作流程時顯示。

1. 「建立項目任務」工作流流程的設計目的是在工作流中建立任務作為步驟。 工作完成後，工作流程才會向前推進。 「建立項目任務」步驟的一個強大方面是，它可以讀取工作流元資料值並使用這些值動態建立任務。

   首先刪除預設建立的「參與者步驟」。 從元件功能表的Sidekick展開&#x200B;**「Projects」**&#x200B;子標題，並拖放&#x200B;**「Create Project Task」**&#x200B;至模型。

   按兩下「建立專案工作」步驟，以開啟工作流程對話方塊。 設定下列屬性：

   此頁籤對於所有工作流進程步驟都很常見，我們將設定「標題」(Title)和「說明」(Description)（最終用戶將看不到這些）。 我們將要設定的重要屬性是從下拉式選單將「工作流程階段」設為&#x200B;**「編輯內容」**。

   ```shell
   Common Tab
   -----------------
       Title = "Start Task Creation"
       Description = "This the first task in the Workflow"
       Workflow Stage = "Edit Content"
   ```

   「建立項目任務」工作流流程的設計目的是在工作流中建立任務作為步驟。 「任務」(Task)頁籤允許我們設定任務的所有值。 在我們的案例中，我們希望受託人保持動態，因此我們將其保留為空白。 其餘的屬性值：

   ```shell
   Task Tab
   -----------------
       Name* = "Edit Content"
       Task Priority = "Medium"
       Description = "Edit the content and finalize for approval. Once finished submit for approval."
       Due In - Days = "2"
   ```

   路由頁籤是一個可選對話框，可為完成任務的用戶指定可用操作。 這些動作只是字串值，將儲存至工作流程的中繼資料。 這些值可由工作流程稍後的指令碼和／或處理步驟讀取，以動態「路由」工作流。 根據[工作流目標](#goals-tutorial)，我們將向此頁籤添加三個操作：

   ```shell
   Routing Tab
   -----------------
       Actions =
           "Normal Approval"
           "Rush Approval"
           "Bypass Approval"
   ```

   此頁籤允許我們配置「預建立任務指令碼」，在該指令碼中，我們可以在建立任務之前以寫程式方式決定該任務的各種值。 您可以選擇將指令碼指向外部檔案，或直接在對話方塊中內嵌短指令碼。 在本例中，我們會將「預建立任務指令碼」指向外部檔案。 在步驟5中，我們將建立該指令碼。

   ```shell
   Advanced Settings Tab
   -----------------
      Pre-Create Task Script = "/apps/aem-guides/projects/scripts/start-task-config.ecma"
   ```

1. 在上一步中，我們參考了「預建立任務指令碼」。 我們現在將建立該指令碼，在該指令碼中，我們將根據工作流元資料值&quot;**assignee**&quot;的值設定任務的受託人。 當工作流程啟動時，將會設定&#x200B;**&quot;assignee&quot;**&#x200B;值。 我們還將讀取工作流元資料，以動態選擇任務的優先順序，方法是讀取工作流元資料的「**taskPriority」**&#x200B;值以及**&quot;taskDueDate&quot; **，以在第一個任務到期時動態設定。

   出於組織目的，我們已在應用程式資料夾下方建立資料夾，以存放所有與專案相關的指令碼：**/apps/aem-guides/projects-tasks/projects/scripts**。 在此資料夾下建立一個名為&#x200B;**&quot;start-task-config.ecma&quot;**&#x200B;的新檔案。 *請注意，請確定start-task-config.ecma檔案的路徑與步驟4中「進階設定」標籤中設定的路徑相符。

   將下列內容新增為檔案內容：

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

1. 導覽回「內容核准工作流程」。 將&#x200B;**OR Split**&#x200B;元件（位於「Workflow」類別下的Sidekick中）拖放到&#x200B;**Start Task**&#x200B;步驟下。 在「常見對話框」中，選擇「3個分支」的單選按鈕。 OR Split將讀取工作流元資料值&#x200B;**&quot;lastTaskAction&quot;**&#x200B;以確定工作流的路由。 **&quot;lastTaskAction&quot;**&#x200B;屬性將設定為步驟4中配置的「路由頁籤」中的值之一。 對於每個「分支」頁籤，請用以下值填寫&#x200B;**Script**&#x200B;文本區域：

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

   *請注意，我們正在進行直接字串匹配以確定路由，因此，Branch指令碼中設定的值必須與步驟4中設定的路由值匹配。

1. 將另一個「**建立項目任務**」步驟拖放到OR拆分下最左側（分支1）。 使用下列屬性填寫對話方塊：

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

   由於這是「正常批准」路由，因此任務的優先順序設定為「中」。 此外，我們為批准者組提供5天的時間來完成Task。 Assignee在Task Tab上留空，因為我們將在Advanced Settings Tab中動態指派此工作。 完成此任務時，我們為「批准者」組提供兩條可能的路由：**「批准並發佈」**（如果內容已核准且可發佈）；如果原始編輯器有需要修正的問題，則&#x200B;**「傳回修訂版本」**（如果有問題）。 核准者可以留下注釋，讓原始編輯者看到工作流程是否傳回給其。

在本教程的前面，我們建立了一個包含批准者角色的項目模板。 每次從此模板建立新項目時，將為「批准者」角色建立特定於項目的組。 就像「參與者步驟」一樣，「任務」只能指派給「用戶」或「組」。 我們要將此任務分配給與批准者組對應的項目組。 從專案中啟動的所有工作流程都會有中繼資料，將專案角色對應至專案特定群組。

複製+在**進階設定**標籤的&#x200B;**Script**&#x200B;文字區中貼上下列程式碼。 此代碼將讀取工作流元資料並將任務分配給項目的「批准者」組。 如果找不到批准者組值，則返回給「管理員」組分配任務。

```
var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");

task.setCurrentAssignee(projectApproverGrp);
```

1. 將另一個「**建立項目任務**」步驟拖放到OR拆分下的中間分支（分支2）。 使用下列屬性填寫對話方塊：

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

   由於這是「快速批准」路由，因此任務的優先順序設定為「高」。 此外，我們僅為「批准者」組提供一天完成任務。 Assignee在Task Tab上留空，因為我們將在Advanced Settings Tab中動態指派此工作。

   我們可以重複使用步驟7中的相同指令碼片段，在**進階設定**標籤上填入&#x200B;**Script**&#x200B;文字區域。 複製+貼上下列程式碼：

   ```
   var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");
   
   task.setCurrentAssignee(projectApproverGrp);
   ```

1. 將Drag+Drop a** No Operation**元件拖放到最右側的分支(Branch 3)。 「無操作」元件不執行任何操作，它將立即進行，表示原始編輯希望繞過批准步驟。 從技術上講，我們可以不執行任何工作流步驟，但作為最佳做法，我們將添加「不操作」步驟。 這就向其他開發人員說明了Branch 3的用途。

   連按兩下工作流程步驟並設定「標題」和「說明」:

   ```
   Common Tab
   -----------------
       Title = "Bypass Approval"
       Description = "Placeholder step to indicate that the original editor decided to bypass the approver group."
   ```

   ![工作流模型OR拆分](./assets/develop-aem-projects/workflow-stage-after-orsplit.png)

   在OR拆分中的三個分支都已配置完畢後，「工作流模型」應該如下所示。

1. 由於「批准者」組可以選擇將工作流發送回原始編輯器以進行進一步修訂，因此我們將依賴&#x200B;**Goto**&#x200B;步驟來讀取最後採取的操作，並將工作流路由到開始或繼續。

   將Goto Step元件（位於「工作流」下的「側腳」中）拖放到OR分割下，重新連接到該元件。 按兩下並在對話框中配置以下屬性：

   ```
   Common Tab
   ----------------
       Title = "Goto Step"
       Description = "Based on the Approver groups action route the workflow to the beginning or continue and publish the payload."
   
   Process Tab
   ---------------
       The step to go to. = "Start Task Creation"
   ```

   我們將配置的最後一段內容是Goto流程步驟中的指令碼。 Script值可透過對話方塊內嵌，或設定為指向外部檔案。 Goto Script必須包含&#x200B;**函式check()**，如果工作流應轉到指定的步驟，則返回true。 在工作流程中傳回錯誤結果。

   如果批准者組選擇&#x200B;**「返回修訂版」**&#x200B;操作（在步驟7和8中配置），則我們希望將工作流返回到&#x200B;**「開始建立任務」**&#x200B;步驟。

   在「流程」標籤中，將下列程式碼片段新增至「指令碼」文字區：

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Send Back for Revision") {
       return true;
   }
   
   return false;
   }
   ```

1. 若要發佈裝載，我們將使用ootb **啟用頁面／資產**&#x200B;處理步驟。 此過程步驟只需少量配置，並將工作流的裝載添加到複製隊列中以進行激活。 我們將在「轉至」步驟下添加步驟，這樣，只有在批准者組批准了要發佈的內容或原始編輯選擇了「繞過批准」路由時，才能到達該步驟。

   拖放模型中Goto步驟下方的&#x200B;**啟動頁面／資產**&#x200B;處理步驟（位於WCM工作流程下的Sidekick中）。

   ![工作流模型完整](assets/develop-aem-projects/workflow-model-final.png)

   新增「goto」步驟和「啟動頁面／資產」步驟後，工作流程模型應該是什麼樣子。

1. 如果「核准者」群組將內容傳回給要讓原始編輯者知道的修訂版本。 我們可以通過動態更改任務建立屬性來完成此任務。 我們將鍵入&#x200B;**&quot;Send Back for Revision&quot;**&#x200B;的lastActionTaken屬性值。 如果存在該值，我們將修改標題和說明，以指出此任務是內容被送回修訂版的結果。 我們也會將優先順序更新為&#x200B;**&quot;High&quot;**，以便它是編輯器使用的第一個項目。 最後，我們將任務到期日設定為從返回工作流進行修訂的一天開始。

   使用以下命令替換開始`start-task-config.ecma`指令碼（在步驟5中建立）:

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

從專案內啟動工作流程時，您必須指定精靈來啟動工作流程。 預設嚮導：`/libs/cq/core/content/projects/workflowwizards/default_workflow`可讓使用者輸入工作流程標題、開始注釋和裝載路徑，以執行工作流程。 下面還提供了幾個其他示例：`/libs/cq/core/content/projects/workflowwizards`。

建立自訂精靈的功能十分強大，因為您可以在工作流程開始之前收集重要資訊。 資料會儲存為工作流程中的中繼資料，而工作流程程式可讀取此資料並根據輸入的值動態變更行為。 我們將建立一個自定義嚮導，根據啟動嚮導值動態分配工作流中的第一個任務。

1. 在CRXDE-Lite中，我們將在`/apps/aem-guides/projects-tasks/projects`檔案夾下方建立名為「精靈」的子檔案夾。 從以下位置複製預設嚮導：`/libs/cq/core/content/projects/workflowwizards/default_workflow`位於新建立的精靈資料夾下方，並將它重新命名為&#x200B;**content-approval-start**。 完整路徑現在應為：`/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start`。

   預設嚮導是雙列嚮導，第一列顯示選定的工作流模型的「標題」(Title)、「說明」(Description)和「縮圖」(Thumbnail)。 第二欄包含「工作流程標題」、「開始注釋」和「裝載路徑」的欄位。 精靈是標準的Touch UI表單，並使用標準[Granite UI表單元件](https://docs.adobe.com/docs/en/aem/6-5/develop/ref/granite-ui/api/jcr_root/libs/granite/ui/components/coral/foundation/form/index.html)填入欄位。

   ![內容核准工作流程精靈](./assets/develop-aem-projects/content-approval-start-wizard.png)

1. 我們將向嚮導添加一個附加欄位，該欄位將用於設定工作流中第一個任務的受託人(請參閱[建立工作流模型](#create-workflow-model):步驟5)。

   在`../content-approval-start/jcr:content/items/column2/items`下面建立一個名為&#x200B;**&quot;assign&quot;**&#x200B;的新節點類型`nt:unstructured`。 我們將使用「項目用戶選擇器」元件（基於[Granite用戶選擇器元件](https://docs.adobe.com/docs/en/aem/6-5/develop/ref/granite-ui/api/jcr_root/libs/granite/ui/components/coral/foundation/form/userpicker/index.html)）。 此表單欄位可讓您輕鬆將使用者和群組選擇限制為僅限屬於目前專案的使用者和群組。

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

   在`/content-approval-start/jcr:content/items/column2/items`下面建立一個名為&#x200B;**priority**&#x200B;的類型`nt:unstructured`的新節點。 我們將使用[Granite UI Select元件](https://docs.adobe.com/docs/en/aem/6-2/develop/ref/granite-ui/api/jcr_root/libs/granite/ui/components/coral/foundation/form/select/index.html)填充表單欄位。

   在&#x200B;**priority**&#x200B;節點下，我們將添加&#x200B;**items**&#x200B;節點，該節點為&#x200B;**nt:unstructured**。 在&#x200B;**items**&#x200B;節點下面添加3個節點，以填充「高」、「中」和「低」的選擇選項。 每個節點的類型為&#x200B;**nt:unstructured**，應具有&#x200B;**text**&#x200B;和&#x200B;**value**&#x200B;屬性。 文字和值都應是相同的值：

   1. 高
   1. 中
   1. 低

   對於「中」節點，添加名為&quot;**selected&quot;**&#x200B;的附加布爾屬性，其值設定為&#x200B;**true**。 這將確保「中」是選取欄位中的預設值。

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

1. 我們將允許工作流啟動器設定初始任務的到期日。 我們將使用[Granite UI DatePicker](https://docs.adobe.com/docs/en/aem/6-5/develop/ref/granite-ui/api/jcr_root/libs/granite/ui/components/coral/foundation/form/datepicker/index.html)表單欄位來擷取此輸入。 我們還將添加一個帶有[TypeHint](https://sling.apache.org/documentation/bundles/manipulating-content-the-slingpostservlet-servlets-post.html#typehint)的隱藏欄位，以確保將輸入儲存為JCR中的Date type屬性。

   添加兩個&#x200B;**nt:antructured**&#x200B;節點，其屬性如下：

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

我們最不需要做的就是確保工作流程模型可從其中一個專案中啟動。 為此，我們需要重新造訪我們在本系列第1部分中建立的專案範本。

「工作流」配置是「項目模板」的一個區域，它指定要與該項目一起使用的可用工作流。 此配置還負責在啟動工作流時指定「啟動工作流嚮導」（我們在[前面的步驟中建立了該嚮導）](#start-workflow-wizard)。 「項目模板」的「工作流」配置是「即時」的，這意味著更新工作流配置將影響建立的新項目以及使用該模板的現有項目。

1. 在CRXDE-Lite中，導覽至先前在`/apps/aem-guides/projects-tasks/projects/templates/authoring-project/workflows/models`建立的編寫專案範本。

   在模型節點下添加名為&#x200B;**contentapproval**&#x200B;的新節點，節點類型為&#x200B;**nt:unstructured**。 將以下屬性添加到節點：

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/etc/workflow/models/aem-guides/content-approval-workflow/jcr:content/model"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

   >[!NOTE]
   >
   >如果使用AEM 6.4,「工作流程」的位置已變更。 將`modelId`屬性指向`/var/workflow/models/aem-guides/content-approval-workflow`下運行時工作流模型的位置
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

1. 將「內容核准」工作流程新增至「專案範本」後，就可從專案的「工作流程」方塊開始使用。 繼續，使用我們建立的各種路線來啟動和播放。

## 支援材料

* [下載完成的教學課程套件](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [GitHub上的完整程式碼存放庫](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)
* [AEM Projects檔案](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html)
