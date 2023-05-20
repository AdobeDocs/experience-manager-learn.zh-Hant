---
title: 內AEM容片段控制台擴展
description: 瞭解如何構建和部署AEMas a Cloud Service內容片段控制台擴展
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
kt: 11603
thumbnail: KT-11603.png
last-substantial-update: 2022-12-09T00:00:00Z
exl-id: 093c8d83-2402-4feb-8a56-267243d229dd
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '745'
ht-degree: 4%

---

# 內AEM容片段控制台擴展

[內AEM容片段控制台](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html) 可通過兩個擴展點添加擴展：按鈕 [內容片段控制台](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html) 標題菜單或操作欄。 這些擴展以JavaScript編寫，作為App Builder應用程式運行，並可以實現自定義Web UI和無伺服器的Adobe I/O Runtime操作，以執行更密集、長時間運行的工作。

![內AEM容片段控制台擴展](./assets/overview/example.png){align="center"}

| 擴展類型 | 說明 | 參數 |
| :--- | :--- | :--- |
| 標題菜單 | 將按鈕添加到在 __零__ 已選擇內容片段。 | 無。 |
| 操作欄 | 將按鈕添加到在 __一個或多個__ 已選擇內容片段。 | 所選內容片段路徑的陣列。 |

單個內AEM容片段控制台擴展可以包括零或一個標題菜單，以及零或一個操作欄擴展類型。 如果需要同一類型的多個擴展類型，則必AEM須建立多個Content Fragments Console擴展。

內AEM容片段控制台擴展，需要 [Adobe Developer控制台項目](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#create-a-project-in-adobe-developer-console) 和 [應用生成器應用](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation) 使用 `@adobe/aem-cf-admin-ui-ext-tpl` 模板，與Adobe Developer控制台項目關聯。

根據擴展的功能生成App Builder應用時，從以下功能中進行選擇。 任何選項組合都可用於擴展。

|  | 將按鈕添加到 [標題菜單](./header-menu.md) | 將按鈕添加到 [操作欄](./action-bar.md) | 顯示 [模式](./modal.md) | 添加 [伺服器端處理程式](./runtime-action.md) |
| ------------------------------------------ | :-----------------------: | :----------------------: | :--------: | :--------------------:  |
| 未選擇內容片段時可用 | ✔ |  |  |  |
| 選擇一個或多個內容片段時可用 |  | ✔ |  |  |
| 從用戶收集自定義輸入 |  |  | ✔️ |  |
| 向用戶顯示自定義反饋 |  |  | ✔️ |  |
| 調用HTTP請AEM求 |  |  |  | ✔ |
| 調用HTTP請求到Adobe/第三方服務 |  |  |  | ✔ |


## Adobe Developer文檔

Adobe Developer包含有關內容片段控AEM制台擴展的開發人員詳細資訊。 請查看 [Adobe Developer內容，進一步技術詳細資訊](https://developer.adobe.com/uix/docs/)。

## 開發擴展

按照下面介紹的步驟瞭解如何生成、開發和部署用於AEMas a Cloud Service的內容片段控制台AEM擴展。

<div class="columns is-multiline">
    <!-- Create Adobe Developer Project -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create Adobe Developer Project">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./adobe-developer-console-project.md" title="建立Adobe Developer項目" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/project/card.png" alt="建立Adobe Developer項目">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">1。建立項目</p>
                    <p class="is-size-6">建立一個Adobe Developer控制台項目，該項目定義了對其他Adobe服務的訪問並管理其部署。</p>
                    <a href="./adobe-developer-console-project.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">建立Adobe Developer項目</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Generate an Extension app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Generate an Extension app">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./app-initialization.md" title="生成擴展應用" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/initialize-app/card.png" alt="初始化擴展應用">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">2.初始化擴展應用</p>
                    <p class="is-size-6">初始化內AEM容片段控制台擴展App Builder應用程式，該應用程式定義擴展的顯示位置及其執行的工作。</p>
                    <a href="./app-initialization.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">初始化擴展應用</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Extension registration -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Extension registration">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./extension-registration.md" title="分機註冊" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/extension-registration/card.png" alt="分機註冊">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">3.分機註冊</p>
                    <p class="is-size-6">將內容片段控制台AEM中的擴展註冊為標題菜單或操作欄擴展類型。</p>
                    <a href="./extension-registration.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">註冊擴展</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Header Menu -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header menu">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./header-menu.md" title="標題菜單" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/header-menu/card.png" alt="標題菜單">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">4a. 標題菜單</p>
                    <p class="is-size-6">瞭解如何建立內AEM容片段控制台標題菜單擴展。</p>
                    <a href="./header-menu.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">擴展標題菜單</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Action Bar -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Action Bar">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./action-bar.md" title="動作列" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/action-bar/card.png" alt="動作列">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">4b. 動作列</p>
                    <p class="is-size-6">瞭解如何建立內容AEM片段控制台操作欄擴展。</p>
                    <a href="./action-bar.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">擴展操作欄</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Modal -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Modal">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./modal.md" title="模型" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/modal/card.png" alt="模型">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">5.模式</p>
                    <p class="is-size-6">將自定義模式添加到擴展中，該擴展可用於為用戶建立定制體驗。 模型通常收集用戶的輸入並顯示操作的結果。</p>
                    <a href="./modal.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">添加模式</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Adobe I/O Runtime action -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adobe I/O Runtime action">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./runtime-action.md" title="Adobe I/O Runtime行動" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/runtime-action/card.png" alt="Adobe I/O Runtime行動">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">6。Adobe I/O Runtime行動</p>
                    <p class="is-size-6">添加擴展可以調用的無伺服器Adobe I/O Runtime操作，以與內容片段交互AEM並執行自定義業務操作。</p>
                    <a href="./runtime-action.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">添加Adobe I/O Runtime操作</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Test -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Test">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./test.md" title="測試" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/test/card.png" alt="測試">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">7。Test</p>
                    <p class="is-size-6">在開發過程中test擴展，並使用特殊URL共用QA或UAT測試人員的完成擴展。</p>
                    <a href="./test.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Test擴展</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Extension deployment -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Extension deployment">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./deploy.md" title="擴展部署" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/deploy/card.png" alt="擴展部署">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">8.生產部署</p>
                    <p class="is-size-6">將擴展部署到Adobe I/O，使用戶可AEM用。 擴展可以更新，也可以刪除。</p>
                    <a href="./deploy.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">部署到生產</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
</div>

## 擴展示例

內容片AEM段控制台擴展示例。

<div class="columns is-multiline">
    <!-- Bulk property update extension -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Bulk property update extension">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./example-extensions/bulk-property-update.md" title="批量屬性更新擴展" tabindex="-1">
                        <img class="is-bordered-r-small" src="./example-extensions/assets/bulk-property-update/card.png" alt="批量屬性更新擴展">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">批量屬性更新擴展</p>
                    <p class="is-size-6">瀏覽批量更新所選內容片段的屬性的示例操作欄擴展。</p>
                    <a href="./example-extensions/bulk-property-update.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">瀏覽示例擴展</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Image Generartion update extension -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="OpenAI-based image generation and upload to AEM extension">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./example-extensions/image-generation-and-image-upload.md" title="基於OpenAI的影像生成和上傳到擴展AEM名" tabindex="-1">
                        <img class="is-bordered-r-small" src="./example-extensions/assets/digital-image-generation/screenshot.png" alt="基於OpenAI的影像生成和上傳到擴展AEM名">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">基於OpenAI的影像生成和上傳到擴展AEM名</p>
                    <p class="is-size-6">瀏覽使用OpenAI生成影像的示例操作欄擴展，將其上載到所AEM選內容片段上並更新影像屬性。</p>
                    <a href="./example-extensions/image-generation-and-image-upload.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">瀏覽示例擴展</span>
                    </a>
                </div>
            </div>
        </div>
    </div>



</div>
