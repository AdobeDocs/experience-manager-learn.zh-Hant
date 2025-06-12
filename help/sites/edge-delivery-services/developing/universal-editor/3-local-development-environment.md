---
title: 設定本機開發環境
description: 針對透過 Edge Delivery Services 傳遞，且可使用通用編輯器進行編輯的網站，設定本機開發環境。
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 700
exl-id: 187c305a-eb86-4229-9896-a74f5d9d822e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '994'
ht-degree: 100%

---

# 設定本機開發環境

本機開發環境對於快速開發以 Edge Delivery Services 進行傳遞的網站來說相當重要。此環境從 Edge Delivery Services 取得內容的同時使用本機開發的程式碼，讓開發人員能夠立即檢視程式碼變更。這樣的設定可以支援快速、反覆的開發與測試。

Edge Delivery Services 網站專案的開發工具和流程設計，旨在維持網頁開發人員所熟悉的部分，並提供快速且有效率的開發體驗。

## 開發拓撲

此影片內容概述可使用通用編輯器進行編輯之 Edge Delivery Services 網站專案的開發拓撲。

>[!VIDEO](https://video.tv.adobe.com/v/3443989/?learn=on&enablevpops&captions=chi_hant)

+++查看其他的開發拓撲詳細資訊

- **GitHub 存放庫**：
   - **用途**：託管網站的程式碼 (CSS 和 JavaScript)。
   - **結構**：**主要分支**&#x200B;包含生產就緒的程式碼，而其他分支則包含工作程式碼。
   - **功能**：任何分支的程式碼都可以針對&#x200B;**生產**&#x200B;或&#x200B;**預覽**&#x200B;環境執行，而不會影響上線網站。

- **AEM Author 服務**：
   - **用途**：用作編輯和管理網站內容的標準內容存放庫。
   - **功能**：使用&#x200B;**通用編輯器**&#x200B;讀取及編寫內容。編輯後的內容會發佈至&#x200B;**生產**&#x200B;或&#x200B;**預覽**&#x200B;環境中的 **Edge Delivery Services**。

- **通用編輯器**：
   - **用途**：提供所見即所得的介面來編輯網站內容。
   - **功能**：從中 **AEM Author 服務** 讀取或寫入至其中。可以設定為使用來自 **GitHub 存放庫**&#x200B;任何分支的程式碼進行測試和驗證變更。

- **Edge Delivery Services**：
   - **生產環境**：
      - **用途**：將上線網站的內容和程式碼傳遞給一般使用者。
      - **功能**：使用來自 **GitHub 存放庫****主要分支**&#x200B;的程式碼，提供由 **AEM Author 服務**&#x200B;發佈的內容。
   - **預覽環境**：
      - **用途**：提供一個中繼環境，在進行部署之前測試和預覽內容及程式碼。
      - **功能**：使用來自 **GitHub 存放庫**&#x200B;任何分支的程式碼，提供由 **AEM Author 服務**&#x200B;發佈的內容，可在不影響上線網站的情況下完成全面測試。

- **本機開發環境**：
   - **用途**：讓開發人員能夠在本機編寫及測試程式碼 (CSS 和 JavaScript)。
   - **結構**：
      - **GitHub 存放庫**&#x200B;原地複製的本機複本，用於分支型開發。
      - **AEM CLI** (作為開發伺服器) 會將本機程式碼變更套用至&#x200B;**預覽環境**，以提供即時重新載入的體驗。
   - **工作流程**：開發人員於本機編寫程式碼，將變更提交至工作分支，將分支推送到 GitHub，在&#x200B;**通用編輯器**&#x200B;中 (使用指定的分支) 進行驗證，並在做好準備進行生產部署時將其合併至&#x200B;**主要分支**&#x200B;內。

+++

## 先決條件

開始進行開發之前，請在您的機器上安裝下列項目：

1. [Git](https://git-scm.com/)
1. [Node.js 和 npm](https://nodejs.org)
1. [Microsoft Visual Studio Code](https://code.visualstudio.com/) (或類似的程式碼編輯器)

## 原地複製 GitHub 存放庫

將[新程式碼專案章節中建立的 GitHub 存放庫](./1-new-code-project.md) (其中包含 AEM Edge Delivery Services 程式碼專案)，原地複製到您的本機開發環境。

![GitHub 存放庫原地複製](./assets/3-local-development-environment/github-clone.png)

```bash
$ cd ~/Code
$ git clone git@github.com:<YOUR_ORG>/aem-wknd-eds-ue.git
```

在 `Code` 目錄中建立一個新的 `aem-wknd-eds-ue` 資料夾，作為專案的根目錄。雖然專案可以原地複製到機器上的任何位置，但本教學課程使用 `~/Code` 作為根目錄。

## 安裝專案相依性

導覽至專案資料夾，並透過 `npm install` 安裝所需的相依性。雖然 Edge Delivery Services 專案不使用 Webpack 或 Vite 等傳統 Node.js 建構系統，但進行本機開發時仍需要一些相依性。

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install
```

## 安裝 AEM CLI

AEM CLI 是一項 Node.js 命令列工具，用於簡化以 Edge Delivery Services 為基礎的 AEM 網站開發過程，其提供本機開發伺服器，可以快速開發和測試您的網站。

若要安裝 AEM CLI，請執行：

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install @adobe/aem-cli
```

也可以使用 `npm install --global @adobe/aem-cli` 全域安裝 AEM CLI。

## 啟動本機 AEM 開發伺服器

`aem up` 命令會啟動本機開發伺服器，並自動開啟連結至伺服器 URL 的瀏覽器視窗。此伺服器會作為 Edge Delivery Services 環境的反向 Proxy，在您使用本機程式碼基底進行開發時，從那裡提供內容。

```bash
$ cd ~/Code/aem-wknd-eds-ue 
$ aem up

    ___    ________  ___                          __      __ 
   /   |  / ____/  |/  /  _____(_)___ ___  __  __/ /___ _/ /_____  _____
  / /| | / __/ / /|_/ /  / ___/ / __ `__ \/ / / / / __ `/ __/ __ \/ ___/
 / ___ |/ /___/ /  / /  (__  ) / / / / / / /_/ / / /_/ / /_/ /_/ / /
/_/  |_/_____/_/  /_/  /____/_/_/ /_/ /_/\__,_/_/\__,_/\__/\____/_/

info: Starting AEM dev server version x.x.x
info: Local AEM dev server up and running: http://localhost:3000/
info: Enabled reverse proxy to https://main--aem-wknd-eds-ue--<YOUR_ORG>.aem.page
```

AEM CLI 會在瀏覽器中開啟網站 `http://localhost:3000/`。專案中的變更會在網頁瀏覽器中自動即時重新載入，而內容變更則[需要發佈至預覽環境](./6-author-block.md)並重新整理網頁瀏覽器。

如果網站開啟時出現 404 頁面，可能是在[新程式碼專案](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project)中更新的 [fstab.yaml 或 paths.json](./1-new-code-project.md) 設定不正確，或變更尚未提交至 `main` 分支。

## 建置 JSON 片段

使用 [AEM 樣板專案 XWalk 範本](https://github.com/adobe-rnd/aem-boilerplate-xwalk)建立的 Edge Delivery Services 專案，倚賴啟用通用編輯器之區塊製作功能的 JSON 設定。

- **JSON 片段**：與其關聯的區塊一起儲存，並定義區塊模型、定義和篩選器。
   - **模型片段**：儲存在 `/blocks/example/_example.json`。
   - **定義片段**：儲存在 `/blocks/example/_example.json`。
   - **篩選器片段**：儲存在 `/blocks/example/_example.json`。


[AEM 樣板專案 XWalk 專案範本](https://github.com/adobe-rnd/aem-boilerplate-xwalk)包含一個 [Husky](https://typicode.github.io/husky/) 預先提交 Hook，其能偵測 JSON 片段的變更，並在 `git commit` 時將其編譯成適當的 `component-*.json` 檔案。

雖然可以透過 `npm run` 手動執行以下 NPM 指令碼來建置 JSON 檔案，但通常不需這麼做，因為 Husky 預先提交 Hook 會自動處理。

```bash
# ~/Code/aem-wknd-eds-ue

npm run build:json
```

| NPM 指令碼 | 說明 |
|--------------------|-----------------------------------------------------------------------------|
| `build:json` | 使用片段建置所有 JSON 檔案，並將其加入到適當的 `component-*.json` 檔案中。 |
| `build:json:models` | 建置區塊 JSON 片段並將其編譯成 `/component-models.json`。 |
| `build:json:definitions` | 建置頁面 JSON 片段並將其編譯成 `/component-definitions.json`。 |
| `build:json:filters` | 建置頁面 JSON 片段並將其編譯成 `/component-filters.json`。 |

>[!TIP]
>
> 對片段檔案進行任何變更後，執行 `npm run build:json` 來重新產生主要的 JSON 檔案。

## Linting

Linting 能確保程式碼品質和一致性，而這是 Edge Delivery Services 專案將變更合併至 `main` 分支之前所必需的。

NPM 指令碼可以透過 `npm run` 執行，例如：

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

| NPM 指令碼 | 說明 |
|------------------|--------------------------------------------------|
| `lint:js` | 執行 JavaScript 的 linting 檢查。 |
| `lint:css` | 執行 CSS 的 linting 檢查。 |
| `lint` | 執行 JavaScript 和 CSS 的 linting 檢查。 |

### 自動修正 linting 問題

您可以在專案的 `package.json` 中新增以下 `scripts`，以便自動解決 linting 問題，且能透過 `npm run` 執行：

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint:fix
```

這些指令碼沒有預先完成 AEM 樣板專案 XWalk 範本相關設定，但可以新增至 `package.json` 檔案中：

>[!BEGINTABS]

>[!TAB 其他指令碼]

| NPM 指令碼 | 命令 | 說明 |
|------------------|------------------------------------------------|-------------------------------------------------------|
| `lint:js:fix` | `npm run lint:js -- --fix` | 自動修正 JavaScript 的 linting 問題。 |
| `lint:css:fix` | `stylelint blocks/**/*.css styles/*.css -- --fix` | 自動修正 CSS 的 linting 問題。 |
| `lint:fix` | `npm run lint:js:fix && npm run lint:css:fix` | 執行 JS 和 CSS 的修正指令碼以快速清理。 |

>[!TAB package.json 範例]

以下指令碼項目可以新增至 `package.json` `scripts` 陣列中。

```json
{
  ...
  "scripts": [
    ...,
    "lint:js:fix": "npm run lint:js -- --fix",
    "lint:css:fix": "npm run lint:css -- --fix",
    "lint:fix": "npm run lint:js:fix && npm run lint:css:fix",
    ...
  ]
  ...
}
```

>[!ENDTABS]
