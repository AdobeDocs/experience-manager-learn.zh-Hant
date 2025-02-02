---
title: 設定本機開發環境
description: 為使用Edge Delivery Services提供且可透過通用編輯器編輯的網站設定本機開發環境。
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 700
source-git-commit: e8ce91b0be577ec6cf8f3ab07ba9ff09c7e7a6ab
workflow-type: tm+mt
source-wordcount: '925'
ht-degree: 1%

---


# 設定本機開發環境

本機開發環境對於快速開發Edge Delivery Services提供的網站至關重要。 此環境使用本機開發的程式碼，同時從Edge Delivery Services取得內容，讓開發人員可立即檢視程式碼變更。 這類設定可支援快速、反複的開發和測試。

Edge Delivery Services網站專案的開發工具和流程是專為網頁開發人員所熟悉，並提供快速且有效率的開發體驗所設計。

## 開發拓朴

可透過Universal Editor編輯的Edge Delivery Services網站專案之開發拓朴包含下列幾個方面：

- **GitHub存放庫**：
   - **用途**：主控網站的程式碼(CSS和JavaScript)。
   - **結構**： **主要分支**&#x200B;包含生產就緒的程式碼，而其他分支則包含工作程式碼。
   - **功能**：任何分支的程式碼都可以在&#x200B;**生產**&#x200B;或&#x200B;**預覽**&#x200B;環境中執行，而不會影響已上線的網站。

- **AEM作者服務**：
   - **用途**：做為編輯和管理網站內容的標準內容存放庫。
   - **功能**：內容是由&#x200B;**通用編輯器**&#x200B;讀取和寫入。 已編輯的內容已發佈到&#x200B;**生產**&#x200B;或&#x200B;**預覽**&#x200B;環境中的&#x200B;**Edge Delivery Services**。

- **通用編輯器**：
   - **用途**：提供用於編輯網站內容的WYSIWYG介面。
   - **功能**：讀取和寫入&#x200B;**AEM作者服務**。 可設定為使用&#x200B;**GitHub存放庫**&#x200B;中任何分支的程式碼來測試及驗證變更。

- **Edge Delivery Services**：
   - **生產環境**：
      - **用途**：將即時網站內容和程式碼提供給一般使用者。
      - **功能**：使用&#x200B;**GitHub存放庫**&#x200B;的&#x200B;**主要分支**&#x200B;的程式碼，提供從&#x200B;**AEM Author服務**&#x200B;發佈的內容。
   - **預覽環境**：
      - **目的**：提供測試環境，以便在部署之前測試和預覽內容與程式碼。
      - **功能**：使用&#x200B;**GitHub存放庫**&#x200B;之任何分支的程式碼，提供從&#x200B;**AEM Author服務**&#x200B;發佈的內容，以啟用徹底測試，而不會影響已上線的網站。

- **本機開發人員環境**：
   - **用途**：可讓開發人員在本機撰寫及測試程式碼(CSS和JavaScript)。
   - **結構**：
      - **GitHub存放庫**&#x200B;的本機復本，用於分支式開發。
      - 做為開發伺服器的&#x200B;**AEM CLI**&#x200B;會將本機程式碼變更套用至&#x200B;**預覽環境**，以進行熱過載體驗。
   - **工作流程**：開發人員在本機撰寫程式碼、認可對有效分支的變更、將分支推送到GitHub、在&#x200B;**通用編輯器** （使用指定的分支）中驗證，並在準備好進行生產部署時將其合併到&#x200B;**主要分支**。

## 先決條件

開始開發之前，請在您的電腦上安裝下列專案：

1. [Git](https://git-scm.com/)
1. [Node.js &amp; npm](https://nodejs.org)
1. [Microsoft Visual Studio Code](https://code.visualstudio.com/) （或類似的程式碼編輯器）

## 複製GitHub存放庫

將包含AEMEdge Delivery Services程式碼專案的[GitHub存放庫](./1-new-code-project.md)複製到您的本機開發環境。

![GitHub存放庫複製](./assets/3-local-development-environment/github-clone.png)

```bash
$ cd ~/Code
$ git clone git@github.com:<YOUR_ORG>/aem-wknd-eds-ue.git
```

已在`Code`目錄中建立新的`aem-wknd-eds-ue`資料夾，做為專案的根目錄。 雖然專案可以複製到電腦上的任何位置，但本教學課程使用`~/Code`作為根目錄。

## 安裝專案相依性

瀏覽至專案資料夾，並安裝`npm install`的必要相依性。 雖然Edge Delivery Services專案不使用傳統的Node.js建置系統（例如Webpack或Vite），但它們仍需要多個相依性才能進行本機開發。

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install
```

## 安裝AEM CLI

AEM CLI是Node.js命令列工具，專為簡化以Edge Delivery Services為基礎的AEM網站的開發而設計，提供本機開發伺服器，用於快速開發和測試您的網站。

若要安裝AEM CLI，請執行：

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install @adobe/aem-cli
```

也可以使用`npm install --global @adobe/aem-cli`全域安裝AEM CLI。

## 啟動本機AEM開發伺服器

`aem up`命令會啟動本機開發伺服器，並自動開啟瀏覽器視窗以顯示伺服器的URL。 此伺服器會成為Edge Delivery Services環境的反向Proxy，在使用本機程式碼庫進行開發時從那裡提供內容。

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

AEM CLI會在您的瀏覽器中`http://localhost:3000/`開啟網站。 專案中的變更會自動在網頁瀏覽器中熱重新載入，而內容變更[需要發佈到預覽環境](./6-author-block.md)並重新整理網頁瀏覽器。

## 建立JSON片段

使用[AEM Boilerplate XWalk範本](https://github.com/adobe-rnd/aem-boilerplate-xwalk)建立的Edge Delivery Services專案依賴在Universal Editor中啟用區塊製作的JSON設定。

- **JSON片段**：與其關聯的區塊一起儲存，並定義區塊模型、定義和篩選器。
   - **模型片段**：儲存在`/blocks/example/_example.json`。
   - **定義片段**：儲存在`/blocks/example/_example.json`。
   - **篩選片段**：儲存在`/blocks/example/_example.json`。

NPM指令碼會編譯這些JSON片段，並將其置於專案根目錄的適當位置。 若要建立JSON檔案，請使用提供的NPM指令碼。 例如，若要編譯所有片段，請執行：

```bash
# ~/Code/aem-wknd-eds-ue

npm run build:json
```

| NPM指令碼 | 說明 |
|--------------------|-----------------------------------------------------------------------------|
| `build:json` | 從片段建置所有JSON檔案，並將其新增至適當的`component-*.json`檔案。 |
| `build:json:models` | 建置區塊JSON片段並將它們編譯成`/component-models.json`。 |
| `build:json:definitions` | 建置頁面JSON片段並將它們編譯成`/component-definitions.json`。 |
| `build:json:filters` | 建置頁面JSON片段並將它們編譯成`/component-filters.json`。 |

>[!TIP]
>
> 在對片段檔案進行任何變更後執行`npm run build:json`以重新產生主要JSON檔案。

## 鑲邊

Linting可確保程式碼品質和一致性，這是將變更合併到`main`分支之前的Edge Delivery Services專案所必需的。

NPM指令碼可以透過`npm run`執行，例如：

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

| NPM指令碼 | 說明 |
|------------------|--------------------------------------------------|
| `lint:js` | 執行JavaScript Linting檢查。 |
| `lint:css` | 執行CSS篩選檢查。 |
| `lint` | 執行JavaScript和CSS篩選檢查。 |

### 自動修正Linting問題

您可以將下列`scripts`新增至專案的`package.json`，以自動解決Linting問題，並且可以透過`npm run`執行：

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint:fix
```

這些指令碼未預先設定AEM Boilerplate XWalk範本，但可新增至`package.json`檔案：

>[!BEGINTABS]

>[!TAB 其他指令碼]

| NPM指令碼 | 命令 | 說明 |
|------------------|------------------------------------------------|-------------------------------------------------------|
| `lint:js:fix` | `npm run lint:js --fix` | 自動修正JavaScript Linting問題。 |
| `lint:css:fix` | `stylelint blocks/**/*.css styles/*.css --fix` | 自動修正CSS篩選問題。 |
| `lint:fix` | `npm run lint:js:fix && npm run lint:css:fix` | 執行JS和CSS修正指令碼以快速清理。 |

>[!TAB package.json範例]

下列指令碼專案可新增至`package.json` `scripts`陣列。

```json
{
  ...
  "scripts": [
    ...,
    "lint:js:fix": "npm run lint:js --fix",
    "lint:css:fix": "npm run lint:css --fix",
    "lint:fix": "npm run lint:js:fix && npm run lint:css:fix",
    ...
  ]
  ...
}
```

>[!ENDTABS]

