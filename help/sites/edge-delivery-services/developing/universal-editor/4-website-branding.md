---
title: 新增網站品牌化
description: 定義 Edge Delivery Services 網站的全域 CSS、CSS 變數和網頁字型。
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: a5cd9906-7e7a-43dd-a6b2-e80f67d37992
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1313'
ht-degree: 100%

---

# 新增網站品牌化

首先藉由更新全域樣式、定義 CSS 變數以及新增網頁字型來設定整體品牌調性。這些基礎元素能確保網站保持一致且可維護，並且應該在整個網站上一致地套用。

## 建立 GitHub 問題

為使一切井然有序，請使用 GitHub 追蹤工作。首先，針對這項工作建立一個 GitHub 問題：

1. 前往 GitHub 存放庫 (如需詳細資訊，請參閱[建立程式碼專案](./1-new-code-project.md)章節)。
2. 按一下「**問題**」索引標籤，然後按一下「**新問題**」。
3. 針對要完成的工作填入&#x200B;**標題**&#x200B;和&#x200B;**說明**。
4. 按一下「**提交新問題**」。

稍後會在[建立提取要求](#merge-code-changes)時使用 GitHub 問題。

![GitHub 建立新問題](./assets/4-website-branding/github-issues.png)

## 建立工作分支

為維持組織架構及確保程式碼品質，請將每項工作各自建立一個新的分支。這種做法可以防止新程式碼對效能產生負面影響，並確保變更在完成之前不會生效。

在本章當中，因為重點在於網站的基本樣式，請建立一個名為 `wknd-styles` 的分支。

```bash
# ~/Code/aem-wknd-eds-ue

$ git checkout -b wknd-styles
```

## 全域 CSS

Edge Delivery Services 會使用位於 `styles/styles.css` 的全域 CSS 檔案來設定整個網站的通用樣式。`styles.css` 會控制如顏色、字型和間距等方面，確保整個網站的所有內容看起來具有一致性。

全域 CSS 不應涉及區塊等低層級的結構，而是專注於網站的整體外觀，以及共用的視覺處理。

請記得，需要時可以覆寫全域 CSS 樣式。

### CSS 變數

[CSS 變數](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties)是儲存顏色、字型和大小等設計設定的絕佳方式。藉由使用變數，您可以在單一位置變更這些元素，然後將其更新至整個網站。

若要開始自訂 CSS 變數，請依照以下步驟進行：

1. 在程式碼編輯器中開啟 `styles/styles.css` 檔案。
2. 找到 `:root` 宣告，那裡儲存全域 CSS 變數。
3. 修改顏色和字型變數，使其符合 WKND 品牌調性。

範例如下：


```css
/* styles/styles.css */

:root {
  /* colors */
  --primary-color: rgb(255, 234, 3); /* WKND primary color */
  --secondary-color: rgb(32, 32, 32); /* Secondary brand color */
  --background-color: white; /* Background color */
  --light-color: rgb(235, 235, 235); /* Light background color */
  --dark-color: var(--secondary-color); /* Dark text color */
  --text-color: var(--secondary-color); /* Default text color */
  --link-color: var(--text-color); /* Link color */
  --link-hover-color: black; /* Link hover color */

  /* fonts */
  --heading-font: 'Roboto', sans-serif; /* Heading font */
  --body-font: 'Open Sans', sans-serif; /* Body font */
  --base-font-size: 16px; /* Base font size */
}
```

探索 `:root` 區段中的其他變數，並檢閱預設設定。

當您在開發網站的過程中發現自己重複相同的 CSS 值時，請考量建立新變數，讓樣式更易於管理。其他可受益於 CSS 變數的 CSS 屬性範例包括：`border-radius`、`padding`、`margin` 和 `box-shadow`。

### 裸元素

裸元素會直接以其元素名稱來設定樣式，而非使用 CSS 類別。例如，並非對 `.page-heading` CSS 類別進行樣式設定，而是使用 `h1 { ... }` 將樣式套用至 `h1` 元素。

在 `styles/styles.css` 檔案中，一組基本樣式被套用至 HTML 裸元素。Edge Delivery Services 網站會優先使用裸元素，因為其與 Edge Delivery Service 的原生語義 HTML 一致。

若要與 WKND 品牌化保持一致，請在 `styles.css` 中設定一些裸元素的樣式：

```css
/* styles/styles.css */

...
h2 {
  font-size: var(--heading-font-size-xl); /* Set font size for h2 */
}

/* Add a partial yellow underline under H2 */
h2::after {
  border-bottom: 2px solid var(--primary-color); /* Yellow underline */
  content: "";
  display: block;
  padding-top: 8px;
  width: 84px;
}
...
```

這些樣式能確保 `h2` 元素 (除非被覆寫) 與 WKND 品牌化保持一致的樣式，有助於建立清晰的視覺層級。每個 `h2` 下方的部分黃色底線，為標題增添獨特的風格。

### 推論元素

在 Edge Delivery Services 中，專案的 `scripts.js` 和 `aem.js` 程式碼會根據其在 HTML 中的內容，自動增強特定的 HTML 裸元素。

例如，在本身的文字行上製作，而非內嵌於周圍文字中的錨點 (`<a>`) 元素，會根據此內容被推論為按鈕。這些錨點會自動被內含 CSS 類別 `button-container` 的容器 `div` 包裝，且錨點元素中已新增一個 `button` CSS 類別。

例如，當連結在其本身的文字行上進行製作時，Edge Delivery Services JavaScript 會將其 DOM 更新為以下內容：

```html
<p class="button-container">
  <a href="/authored/link" title="Click me" class="button">Click me</a>
</p>
```

這些按鈕可以自訂以符合 WKND 品牌調性，其中規定按鈕應顯示為含有黑色文字的黃色矩形。

以下是如何在 `styles.css` 中設定「推論按鈕」樣式的範例：

```css
/* styles/styles.css */

/* Buttons */
a.button:any-link,
button {
  box-sizing: border-box;
  display: inline-block;
  max-width: 100%;
  margin: 12px 0;
  border: 2px solid transparent;
  padding: 0.5em 1.2em;
  font-family: var(--body-font-family);
  font-style: normal;
  font-weight: 500;
  line-height: 1.25;
  text-align: center;
  text-decoration: none;
  cursor: pointer;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;

  /* WKND specific treatments */
  text-transform: uppercase;
  background-color: var(--primary-color);
  color: var(--dark-color);
  border-radius: 0;
}
```

此 CSS 定義基本的按鈕樣式，並加入 WKND 特定的處理，例如大寫文字、黃色背景和黑色文字。`background-color` 和 `color` 屬性使用 CSS 變數，讓按鈕樣式能夠與品牌顏色保持一致。這種方法可以確保整個網站上的按鈕樣式一致，同時保持彈性。

## 網頁字型

Edge Delivery Services 專案將網頁字型的使用最佳化，藉以保持高效能，並盡量減少對 Lighthouse 分數的影響。這種方法能確保快速轉譯，但不會對網站的視覺識別造成負面影響。以下是如何有效地實施網頁字型以獲得最佳效能的方法。

### Font face

使用 `styles/fonts.css` 檔案中的 CSS `@font-face` 宣告新增自訂網頁字型。於 `fonts.css` 中新增 `@font-faces`，能確保在最佳時間載入網頁字型，有助於維持 Lighthouse 分數。

1. 開啟 `styles/fonts.css`。
2. 新增以下 `@font-face` 宣告以加入 WKND 品牌字型：`Asar` 和 `Source Sans Pro`。

```css
/* styles/fonts.css */

@font-face {
  font-family: Asar;
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/asar/v22/sZlLdRyI6TBIbkEaDZtQS6A.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 300;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKwdSBYKcSV-LCoeQqfX1RYOo3qPZZMkids18S0xR41.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xK1dSBYKcSV-LCoeQqfX1RYOo3qPZ7nsDJB9cme.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 600;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKwdSBYKcSV-LCoeQqfX1RYOo3qPZY4lCds18S0xR41.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 300;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKydSBYKcSV-LCoeQqfX1RYOo3ik4zwlxdu3cOWxw.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xK3dSBYKcSV-LCoeQqfX1RYOo3qOK7lujVj9w.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 600;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKydSBYKcSV-LCoeQqfX1RYOo3i54rwlxdu3cOWxw.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}
```

本教學課程中所用的字型來自 Google Fonts，但您可以向任何字型提供者取得網頁字型，包括 [Adobe Fonts](https://fonts.adobe.com/)。

+++使用本機網頁字型檔案

或者，將網頁字型檔案複製到 `/fonts` 資料夾中的專案內，並在 `@font-face` 宣告中參照。

本教學課程使用遠端託管的網頁字型，因此更容易跟著進行。

```css
/* styles/fonts.css */

@font-face { 
    font-family: Asar;
    ...
    src: url("/fonts/asar.woff2") format('woff2'),
    ...
}
```

+++

最後，更新 `styles/styles.css` CSS 變數，開始使用新字型：

```css
/* styles/styles.css */

:root {
    ...
    /* fonts */
    --body-font-family: 'Source Sans Pro', roboto-fallback, sans-serif;
    --heading-font-family: 'Asar', roboto-condensed-fallback, sans-serif;
    ...
}
```

`roboto-fallback` 和 `roboto-condensed-fallback` 為備用字型，會於[備用字型](#fallback-fonts)區段更新以保持一致，支援自訂的 `Asar` 和 `Source Sans Pro` 網頁字型。

### 備用字型

網頁字型通常會因其大小而影響效能，可能會導致累積的版面移動 (CLS) 分數增加，而降低整體 Lighthouse 分數。為確保在載入網頁字型時能即時顯示文字，Edge Delivery Services 專案使用瀏覽器原生的備用字型。這種方法有助於在套用所需字型時保持流暢的使用者體驗。

若要選取最佳的備用字型，請使用 Adobe 的 [Helix 字型備用 Chrome 擴充功能](https://www.aem.live/developer/font-fallback)，此擴充功能會在自訂字型載入之前，決定一個適合瀏覽器使用的最接近的字型。所產生的備用字型宣告應新增至 `styles/styles.css` 檔案，以提高效能並確保使用者的順暢體驗。

![Helix 字型備用 Chrome 擴充功能](./assets/4-website-branding/font-fallback-chrome-plugin.png){align=center}

若要使用 [Helix 字型備用 Chrome 擴充功能](https://www.aem.live/developer/font-fallback)，請確保網頁所套用的網頁字型與 Edge Delivery Services 網站上所使用的字型相同。本教學課程示範在 [wknd.site](http://wknd.site/us/en.html) 上的擴充功能。開發網站時，應將擴充功能套用至正在開發的網站，而非 [wknd.site](http://wknd.site/us/en.html)。

```css
/* styles/styles.css */
...

/* fallback fonts */

/* Fallback font for Asar (normal - 400) */
@font-face {
    font-family: "asar-normal-400-fallback";
    size-adjust: 95.7%;
    src: local("Arial");
}

/* Fallback font for Source Sans Pro (normal - 400) */
@font-face {
    font-family: "source-sans-pro-normal-400-fallback";
    size-adjust: 92.9%;
    src: local("Arial");
}

...
```

將備用字型系列名稱新增至 `styles/styles.css` 的字型 CSS 變數，放在「real」字型系列名稱之後。

```css
/* styles/styles.css */

:root {
    ...
    /* fonts */
    --body-font-family: 'Source Sans Pro', source-sans-pro-normal-400-fallback, sans-serif;
    --heading-font-family: 'Asar', asar-normal-400-fallback, sans-serif;
    ...
}
```

## 開發預覽

當您新增 CSS 時，AEM CLI 的本機開發環境會自動重新載入變更，因此能快速輕鬆地查看 CSS 會如何影響區塊。

![WKND 品牌 CSS 開發預覽](./assets/4-website-branding/preview.png)


## 下載最終版的 CSS 檔案

您可以從下方連結下載更新的 CSS 檔案：

* [`styles.css`](https://raw.githubusercontent.com/davidjgonzalez/aem-wknd-eds-ue/refs/heads/main/styles/styles.css)
* [`fonts.css`](https://raw.githubusercontent.com/davidjgonzalez/aem-wknd-eds-ue/refs/heads/main/styles/fonts.css)

## 對 CSS 檔案執行 lint 檢查

務必針對您的程式碼變更[經常進行 lint 檢查](./3-local-development-environment.md#linting)，保持程式碼整潔且一致。定期 linting 有助於及早發現問題，進而減少整體開發時間。請記住，在所有 linting 問題都得到解決以前，您不能將您的開發工作合併到主分支內！

```bash
$ npm run lint:css
```

## 合併程式碼變更

將變更合併到 GitHub 上的 `main` 分支中，並以這些更新為基礎建置未來的工作。

```bash
$ git add .
$ git commit -m "Add global CSS, CSS variables, and web fonts"
$ git push origin wknd-styles
```

變更推送至 `wknd-styles` 分支後，在 GitHub 上建立提取要求，將其合併到 `main` 分支中。

1. 從[建立新專案](./1-new-code-project.md)章節導覽至 GitHub 存放庫。
2. 按一下「**提取要求**」索引標籤，然後選取「**新提取要求**」。
3. 將 `wknd-styles` 設定為來源分支，將 `main` 設定為目標分支。
4. 檢閱變更然後按一下「**建立提取要求**」。
5. 在提取要求詳細資料中，**新增以下內容**：

   ```
   Add basic global CSS, CSS variables, and web fonts (including fallback fonts) to support the WKND brand.
   
   Fix #1
   
   Test URLs:
   - Before: https://main--wknd-aem-eds-ue--davidjgonzalez.aem.live/
   - After: https://wknd-styles--wknd-aem-eds-ue--davidjgonzalez.aem.live/
   ```

   * `Fix #1` 會參照先前所建立的 GitHub 問題。
   * 測試 URL 會告訴 AEM Code Sync 要使用哪些分支進行驗證和比較。「After」URL 使用工作分支 `wknd-styles`，檢查程式碼變更會如何影響網站效能。

6. 按一下「**建立提取要求**」。
7. 等待 [AEM Code Sync GitHub 應用程式](./1-new-code-project.md)**完成品質檢查**。若未通過，請解決錯誤然後重新執行檢查。
8. 檢查通過後，將&#x200B;**提取要求合併**&#x200B;到 `main` 內。

隨著變更合併至 `main` 內，現在被視為已經部署至生產環境，並可以根據這些更新進行新的開發。
