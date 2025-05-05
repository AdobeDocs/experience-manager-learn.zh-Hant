---
title: 檔案製作影片
description: 瞭解如何在AEM中設定及設定檔案製作。
version: Experience Manager as a Cloud Service
feature: Authoring
topic: Content Management
role: User
level: Beginner
jira: KT-14609
doc-type: Catalog
duration: 40
last-substantial-update: 2023-12-01T00:00:00Z
exl-id: 172a477f-d277-43c1-8e47-68870b02203c
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1047'
ht-degree: 9%

---

# 檔案製作影片

瞭解如何設定檔案編寫，以允許AEM作者使用Microsoft Word或Google Docs編輯和發佈檔案。

檢閱[檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/edge-delivery/overview.html?lang=zh-Hant)，以取得設定檔案編寫的完整詳細資料。

## 檔案製作快速入門

<div class="columns is-multiline">
    <!-- Setting up Edge Delivery: Document Authoring -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Setting up Edge Delivery: Document Authoring" tabindex="1">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="set-up.md" title="檔案製作設定"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3438875/?format=jpeg&captions=chi_hant"
                alt="檔案製作概觀">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">5 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="set-up.md" title="設定檔案製作">設定檔案製作</a>
            </p>
            <p class="is-size-6">瞭解如何設定檔案製作。</p>
            <a href="set-up.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>
    <!-- Previewing and Publishing Content-->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Previewing and Publishing Content" tabindex="1">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="preview-and-publish.md" title="預覽和發佈內容"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3441356/?format=jpeg&captions=chi_hant"
                alt="預覽和發佈內容">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">4 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="preview-and-publish.md" title="預覽和發佈內容">編輯器總覽</a>
            </p>
            <p class="is-size-6">使用AEM Sidekick預覽和發佈內容的簡要概觀。</p>
            <a href="preview-and-publish.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>
    <!-- Structure of a Document -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Exploring the Structure of a Document" tabindex="2">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="document-structure.md" title="檔案結構"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3438827/?format=jpeg&captions=chi_hant" alt="檔案結構">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">1 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="document-structure.md" title="檔案結構">檔案結構</a>
            </p>
            <p class="is-size-6">探索檔案的結構。</p>
            <a href="document-structure.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>
    <!-- Default Content and Sections -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Default Content and Sections" tabindex="3">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="default-content-and-sections.md" title="預設內容和區段"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3437986/?format=jpeg&captions=chi_hant" alt="預設內容和區段">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">1 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="default-content-and-sections.md" title="預設內容和區段">新編輯器切換</a>
            </p>
            <p class="is-size-6">探索預設內容和區段的檔案製作概念。</p>
            <a href="default-content-and-sections.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>
    <!-- Blocks and Autoblocks--->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Blocks and Autoblocks" tabindex="4">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="blocks-and-autoblocks.md" title="封鎖和自動封鎖" tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3439515/?format=jpeg&captions=chi_hant"
                alt="封鎖和自動封鎖">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">1 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="blocks-and-autoblocks.md" title="封鎖和自動封鎖">
                封鎖和自動封鎖</a>
            </p>
            <p class="is-size-6">探索檔案製作區塊和自動區塊中的概念。</p>
            <a href="blocks-and-autoblocks.md"
              class="spectrum-Button spectrum-Button--outline
              spectrum-Button--primary spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>
    <!-- Redirects -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Redirects" tabindex="5">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="redirects.md" title="重新導向"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3438554/?format=jpeg&captions=chi_hant" alt="重新導向">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">1 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="redirects.md" title="重新導向">建立重新導向</a>
            </p>
            <p class="is-size-6">探索如何使用檔案製作來建立重新導向。</p>
            <a href="redirects.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>
    <!-- Bulk Metadata -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Bulk Metadata" tabindex="6">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="bulk-metadata.md" title="大量中繼資料"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3438433/?format=jpeg&captions=chi_hant"
                alt="大量中繼資料">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">1 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="bulk-metadata.md" title="大量中繼資料">語言
                復本</a>
            </p>
            <p class="is-size-6">探索在檔案製作中如何處理大量中繼資料。</p>
            <a href="bulk-metadata.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>
     <!-- Page Level Metadata -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Page Level Metadata" tabindex="7">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="page-metadata.md" title="頁面繼資料"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3438079/?format=jpeg&captions=chi_hant"
                alt="頁面繼資料">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">2 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="page-metadata.md" title="頁面繼資料">頁面中繼資料</a>
            </p>
            <p class="is-size-6">探索檔案製作如何管理頁面中繼資料。</p>
            <a href="page-metadata.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>
    <!-- Author Authentication -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Author Authentication" tabindex="1">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="author-authentication.md" title="作者驗證"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3438189/?format=jpeg&captions=chi_hant"
                alt="作者驗證">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">2 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="author-authentication.md" title="作者驗證">作者驗證</a>
            </p>
            <p class="is-size-6">瞭解如何設定作者驗證。</p>
            <a href="author-authentication.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>    
</div>

## 操作說明影片

<div class="columns is-multiline">
    <!-- Responsive Navigation -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Responsive Navigation" tabindex="7">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./how-to/responsive-navigation.md" title="回應式導覽"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3438178/?format=jpeg&captions=chi_hant"
                alt="回應式導覽">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">2 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="./how-to/responsive-navigation.md" title="回應式導覽">回應式導覽</a>
            </p>
            <p class="is-size-6">探索檔案製作如何定義回應式導覽。</p>
            <a href="page-metadata.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>
  <!-- Document Audit -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Document Audit" tabindex="7">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./how-to/document-audit.md" title="檔案稽核"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3437722/?format=jpeg&captions=chi_hant"
                alt="檔案稽核">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">2 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="./how-to/document-audit.md" title="檔案稽核">檔案稽核</a>
            </p>
            <p class="is-size-6">探索檔案製作處理檔案稽核的方式。</p>
            <a href="page-metadata.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>
  <!-- Document Permissions -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Document Permissions" tabindex="7">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./how-to/document-permissions.md" title="檔案許可權"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3438112/?format=jpeg&captions=chi_hant"
                alt="檔案許可權">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">2 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="./how-to/document-permissions.md" title="檔案許可權">檔案許可權</a>
            </p>
            <p class="is-size-6">探索檔案編寫如何處理檔案許可權。</p>
            <a href="./how-to/document-permissions.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>
    <!-- Document Versions -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Document Versions" tabindex="7">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./how-to/document-versions.md" title="檔案版本"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3438807/?format=jpeg&captions=chi_hant"
                alt="檔案版本">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">2 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="./how-to/document-versions.md" title="檔案版本">檔案版本</a>
            </p>
            <p class="is-size-6">探索檔案製作處理檔案版本的方式。</p>
            <a href="./how-to/document-versions.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>
      <!-- Document Workflows -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Document Workflows" tabindex="7">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./how-to/document-workflows.md" title="檔案工作流程"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3438301/?format=jpeg&captions=chi_hant"
                alt="檔案工作流程">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">2 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="./how-to/document-workflows.md" title="檔案工作流程">檔案工作流程</a>
            </p>
            <p class="is-size-6">探索檔案製作處理檔案工作流程的方式。</p>
            <a href="./how-to/document-workflows.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>
      <!-- iFrames-->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="iFrames" tabindex="7">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./how-to/iframes.md" title="使用iFrame"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3438400/?format=jpeg&captions=chi_hant"
                alt="使用iFrame">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">2 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="./how-to/iframes.md" title="使用iFrame">使用iFrame</a>
            </p>
            <p class="is-size-6">探索檔案撰寫如何處理視訊和iFrame。</p>
            <a href="./how-to/iframes.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>
    <!-- Alt Text -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Alt Text" tabindex="7">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./how-to/image-alt-text.md" title="使用替代文字"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3438686/?format=jpeg&captions=chi_hant"
                alt="使用替代文字">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">2 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="./how-to/image-alt-text.md" title="使用替代文字">編寫影像替代文字</a>
            </p>
            <p class="is-size-6">探索檔案製作如何處理影像替代文字。</p>
            <a href="./how-to/image-alt-text.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>
    <!-- No Index -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="No Index" tabindex="7">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./how-to/no-index.md" title="防止索引"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3438130/?format=jpeg&captions=chi_hant"
                alt="防止索引">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">2 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="./how-to/no-index.md" title="防止索引">防止搜尋引擎索引</a>
            </p>
            <p class="is-size-6">探索如何防止搜尋引擎編制索引。</p>
            <a href="./how-to/no-index.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>
    <!-- Dynamic Media in Edge Delivery Services -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Dynamic Media" tabindex="7">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./how-to/using-dynamic-media.md" title="Dynamic Media"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3438917/?format=jpeg&captions=chi_hant"
                alt="Dynamic Media">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">2 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="./how-to/using-dynamic-media.md" title="Dynamic Media">動態媒體</a>
            </p>
            <p class="is-size-6">探索如何使用Dynamic Media來最佳化影像傳送。</p>
            <a href="./how-to/using-dynamic-media.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>   
    <!-- Site Migration using the Importer -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Site migration using Importer" tabindex="7">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./how-to/migration-using-importer.md" title="使用Importer進行網站移轉"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3443707/?format=jpeg&captions=chi_hant"
                alt="使用Importer進行網站移轉">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">2 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              使用Importer進行<a href="./how-to/migration-using-importer.md" title="使用Importer進行網站移轉">網站移轉</a>
            </p>
            <p class="is-size-6">探索如何使用AEM Importer工具將網站移轉至Edge Delivery Services。</p>
            <a href="./how-to/migration-using-importer.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>     
    <!-- Customizing the Importer -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Customizing the Importer" tabindex="7">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./how-to/customizing-importer.md" title="自訂匯入工具"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3444256/?format=jpeg&captions=chi_hant"
                alt="自訂匯入工具">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">3 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="./how-to/customizing-importer.md" title="自訂匯入工具">自訂匯入工具</a>
            </p>
            <p class="is-size-6">探索如何自訂AEM Importer工具以進行網站移轉。</p>
            <a href="./how-to/customizing-importer.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>
    <!-- Bulk importing using Importer -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Bulk importing using the Importer" tabindex="7">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./how-to/bulk-importing-using-importer.md" title="使用匯入工具大量匯入"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3445896/?format=jpeg&captions=chi_hant"
                alt="使用匯入工具大量匯入">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">3 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="./how-to/bulk-importing-using-importer.md" title="使用匯入工具大量匯入">使用匯入工具大量匯入</a>
            </p>
            <p class="is-size-6">探索如何在網站移轉期間使用AEM Importer工具大量匯入網頁。</p>
            <a href="./how-to/bulk-importing-using-importer.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>                
  </div>

### 產生變數影片

<div class="columns is-multiline">
    <!-- Intro Generate Variation -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Generate Variations" tabindex="7">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./how-to/generate-variations/overview.md" title="產生變化版本"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3441346/?format=jpeg&captions=chi_hant"
                alt="產生變化版本">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">2 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="./how-to/generate-variations/overview.md" title="產生變化版本">產生變化版本</a>
            </p>
            <p class="is-size-6">在Edge Delivery Services中產生變數簡介，並瞭解其對行銷人員的用處。</p>
            <a href="./how-to/generate-variations/overview.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>  
    <!--  Configure Sidekick for Generative Variations  -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Generate Variations - Configure Sidekick" tabindex="7">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./how-to/generate-variations/configure-sidekick.md" title="產生變數 — 設定Sidekick"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3437000/?format=jpeg&captions=chi_hant"
                alt="產生變數 — 設定Sidekick">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">1 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="./how-to/generate-variations/configure-sidekick.md" title="產生變數 — 設定Sidekick">產生變數 — 設定Sidekick</a>
            </p>
            <p class="is-size-6">探索如何在Edge Delivery Services檔案製作中設定Sidekick以產生變數。</p>
            <a href="./how-to/generate-variations/configure-sidekick.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>          
    <!-- GenAI Prompt Templates -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Generate Variations - Prompt templates" tabindex="7">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./how-to/generate-variations/prompt-templates.md" title="產生變數 — 提示範本"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3441346/?format=jpeg&captions=chi_hant"
                alt="產生變數 — 提示範本">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">2 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="./how-to/generate-variations/prompt-templates.md" title="產生變數 — 提示範本">產生變化 — 提示範本</a>
            </p>
            <p class="is-size-6">探索如何使用提示範本來產生變數。</p>
            <a href="./how-to/generate-variations/prompt-templates.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>    
    <!-- Custom Prompt Templates -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Generate Variations - Custom prompt templates" tabindex="7">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./how-to/generate-variations/custom-prompt-templates.md" title="產生變數 — 自訂提示範本"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3438510/?format=jpeg&captions=chi_hant"
                alt="產生變數 — 自訂提示範本">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">2 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="./how-to/generate-variations/custom-prompt-templates.md" title="產生變數 — 自訂提示範本">產生變化 — 自訂提示範本</a>
            </p>
            <p class="is-size-6">探索如何建立產生變異的自訂提示範本。</p>
            <a href="./how-to/generate-variations/custom-prompt-templates.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>    
    <!-- Saving Custom Prompt Templates -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Generate Variations - Save custom prompt templates" tabindex="7">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./how-to/generate-variations/custom-prompt-templates.md" title="產生變數 — 儲存自訂提示範本"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3437521/?format=jpeg&captions=chi_hant"
                alt="產生變數 — 儲存自訂提示範本">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">2 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="./how-to/generate-variations/custom-prompt-templates.md" title="產生變數 — 儲存自訂提示範本">產生變化 — 儲存自訂提示範本</a>
            </p>
            <p class="is-size-6">探索如何儲存產生變異的自訂提示範本。</p>
            <a href="./how-to/generate-variations/custom-prompt-templates.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>
    <!-- Using Adobe Target Audiences for Generate Variations -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Generate Variations - Using Adobe Target audiences" tabindex="7">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./how-to/generate-variations/using-target-audiences.md" title="產生變數 — 使用Adobe Target對象"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3437766/?format=jpeg&captions=chi_hant"
                alt="產生變數 — 使用Adobe Target對象">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">2 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="./how-to/generate-variations/using-target-audiences.md" title="產生變數 — 使用Adobe Target對象">產生變數 — 使用Adobe Target對象</a>
            </p>
            <p class="is-size-6">探索如何使用Adobe Target受眾，以針對您的內容變數鎖定正確的受眾。</p>
            <a href="./how-to/generate-variations/using-target-audiences.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>
    <!-- Using audience CSV files for Generate Variations -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Generate Variations - Using CSV file audiences" tabindex="7">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./how-to/generate-variations/using-csv-file-audiences.md" title="產生變數 — 使用CSV檔案對象"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3436898/?format=jpeg&captions=chi_hant"
                alt="產生變數 — 使用CSV檔案對象">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">1 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="./how-to/generate-variations/using-csv-file-audiences.md" title="產生變數 — 使用CSV檔案對象">產生變數 — 使用CSV檔案對象</a>
            </p>
            <p class="is-size-6">探索如何使用對象CSV檔案，以針對您的內容變數鎖定適當的對象。</p>
            <a href="./how-to/generate-variations/using-csv-file-audiences.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>   
    <!-- Use Adobe Firefly to create images -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Generate Variations - Use Adobe Firefly" tabindex="7">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./how-to/generate-variations/using-adobe-firefly-for-images.md" title="產生變數 — 使用Adobe Firefly"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3438367/?format=jpeg&captions=chi_hant"
                alt="產生變數 — 使用Adobe Firefly">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">1 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="./how-to/generate-variations/using-adobe-firefly-for-images.md" title="產生變數 — 使用Adobe Firefly">產生變數 — 使用Adobe Firefly</a>
            </p>
            <p class="is-size-6">探索如何使用Adobe Firefly建立用於內容變異的影像。</p>
            <a href="./how-to/generate-variations/using-adobe-firefly-for-images.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>  
    <!-- Generate Variations Actions -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Generate Variations - Actions on a generated variation" tabindex="7">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./how-to/generate-variations/actions.md" title="產生變數 — 對所產生變數的動作"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3437307/?format=jpeg&captions=chi_hant"
                alt="產生變數 — 對所產生變數的動作">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">1 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="./how-to/generate-variations/actions.md" title="產生變數 — 對所產生變數的動作">產生變化 — 對產生的變化執行的動作</a>
            </p>
            <p class="is-size-6">探索可用於產生內容變數的動作。</p>
            <a href="./how-to/generate-variations/actions.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>    
    <!-- Trust and privacy in Generative AI -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Generate Variations - Trust and Privacy" tabindex="7">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./how-to/generate-variations/trust-privacy.md" title="產生變數 — 信任和隱私"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3440025/?format=jpeg&captions=chi_hant"
                alt="產生變數 — 信任和隱私">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">2 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="./how-to/generate-variations/trust-privacy.md" title="產生變數 — 信任和隱私">產生變化 — 信任和隱私</a>
            </p>
            <p class="is-size-6">探索Adobe如何處理產生變異的信任和隱私權。</p>
            <a href="./how-to/generate-variations/trust-privacy.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>  
    <!-- Overview of experimentation framework -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Overview of experimentation framework" tabindex="7">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./how-to/experimentation-framework.md" title="實驗框架概觀"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3437870/?format=jpeg&captions=chi_hant"
                alt="實驗框架概觀">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">2 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="./how-to/experimentation-framework.md" title="實驗框架概觀">實驗架構概觀</a>
            </p>
            <p class="is-size-6">探索實驗架構，讓行銷人員測試哪些內容變數最有效。</p>
            <a href="./how-to/experimentation-framework.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>                        
    <!-- Setup experimentation framework -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Setting up experimentation framework" tabindex="7">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./how-to/setup-experimentation-framework.md" title="設定實驗架構"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3438939/?format=jpeg&captions=chi_hant"
                alt="設定實驗架構">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">2 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="./how-to/setup-experimentation-framework.md" title="設定實驗架構">正在設定實驗架構</a>
            </p>
            <p class="is-size-6">探索如何在Edge Delivery Services檔案製作中設定實驗架構。</p>
            <a href="./how-to/setup-experimentation-framework.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>
    <!-- Adding metadata for experimentation -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen"
      aria-label="Adding metadata for experimentation" tabindex="7">
      <div class="card">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./how-to/experimentation-add-metadata.md" title="新增用於實驗的中繼資料"
              tabindex="-1">
              <img class="is-bordered-r-small"
                src="https://video.tv.adobe.com/v/3440129/?format=jpeg&captions=chi_hant"
                alt="新增用於實驗的中繼資料">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p style="float: right;font-style: italic; color: #363636"
              class="is-size-6">2 分鐘</p>
            <p class="headline is-size-6 has-text-weight-bold">
              <a href="./how-to/experimentation-add-metadata.md" title="新增用於實驗的中繼資料">正在新增實驗的中繼資料</a>
            </p>
            <p class="is-size-6">探索新增實驗用的中繼資料</p>
            <a href="./how-to/experimentation-add-metadata.md" class="spectrum-Button
              spectrum-Button--outline spectrum-Button--primary
              spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap
                has-text-weight-bold">觀看影片</span>
            </a>
          </div>
        </div>
      </div>
    </div>   
  </div>