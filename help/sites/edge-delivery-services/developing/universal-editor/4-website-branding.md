---
title: 新增網站品牌
description: 為Edge Delivery Services網站定義全域CSS、CSS變數和網頁字型。
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: a5cd9906-7e7a-43dd-a6b2-e80f67d37992
source-git-commit: ecd3ce33204fa6f3f2c27ebf36e20ec26e429981
workflow-type: tm+mt
source-wordcount: '1315'
ht-degree: 0%

---

# 新增網站品牌

從設定整體品牌開始，方法是更新全域樣式、定義CSS變數以及新增網頁字型。 這些基礎元素可確保網站維持一致性和可維護性，並應在整個網站中以一致的方式套用。

## 建立GitHub問題

若要讓一切井然有序，請使用GitHub追蹤工作。 首先，請針對以下工作內容建立GitHub問題：

1. 前往GitHub存放庫（如需詳細資訊，請參閱[建立程式碼專案](./1-new-code-project.md)章節）。
2. 按一下&#x200B;**問題**&#x200B;標籤，然後按&#x200B;**新問題**。
3. 為要完成的工作撰寫&#x200B;**標題**&#x200B;和&#x200B;**描述**。
4. 按一下&#x200B;**提交新問題**。

稍後當[建立提取請求](#merge-code-changes)時會使用GitHub問題。

![GitHub建立新問題](./assets/4-website-branding/github-issues.png)

## 建立工作分支

若要維護組織並確保程式碼品質，請為每個工作內容建立新的分支。 此做法可防止新程式碼對效能造成負面影響，並確保變更在完成之前不會生效。

本章著重於網站的基本樣式，請建立名為`wknd-styles`的分支。

```bash
# ~/Code/aem-wknd-eds-ue

$ git checkout -b wknd-styles
```

## 全域CSS

Edge Delivery Services使用位於`styles/styles.css`的全域CSS檔案，為整個網站設定通用樣式。 `styles.css`可控制顏色、字型和間距等方面，確保整個網站的所有內容看起來都一致。

全域CSS應與較低層級的建構（例如區塊）無關，而是應專注於網站的整體外觀和風格，以及共用的視覺處理。

請記住，必要時可覆寫全域CSS樣式。

### css變數

[CSS變數](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties)是儲存設計設定（如顏色、字型和大小）的絕佳方式。 透過使用變數，您可以在一個位置變更這些元素，並使其在整個網站中更新。

若要開始自訂CSS變數，請遵循下列步驟：

1. 在程式碼編輯器中開啟`styles/styles.css`檔案。
2. 尋找儲存全域CSS變數的`:root`宣告。
3. 修改顏色和字型變數以符合WKND品牌。

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

探索`:root`區段中的其他變數並檢閱預設設定。

當您開發網站時，如果發現重複相同的CSS值，請考慮建立新變數讓樣式更易於管理。 可以從CSS變數受益的其他CSS屬性範例，包括： `border-radius`、`padding`、`margin`和`box-shadow`。

### 裸元素

裸元素會直接透過其元素名稱來設定樣式，而不使用CSS類別。 例如，使用`h1 { ... }`將樣式套用至`h1`元素，而不使用`.page-heading` CSS類別的樣式。

在`styles/styles.css`檔案中，一組基本樣式套用至純HTML元素。 Edge Delivery Services網站會使用裸元素來排定優先順序，因為它們會符合Edge Delivery服務的原生語意HTML。

為了符合WKND品牌，讓我們在`styles.css`中設定一些裸元素的樣式：

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

這些樣式可確保`h2`元素（除非被覆寫）的樣式與WKND品牌化一致，有助於建立清晰的視覺階層。 每個`h2`下方的部分黃色底線會為標題增加獨特觸控。

### 推斷的元素

在Edge Delivery Services中，專案的`scripts.js`和`aem.js`程式碼會根據HTML中的內容，自動增強特定的純HTML元素。

例如，根據此內容，在錨點(`<a>`)元素自己的行上撰寫（而不是與周圍文字內嵌）被推斷為按鈕。 這些錨點會以CSS類別`button-container`的容器`div`自動包裝，而且錨點元素已新增`button` CSS類別。

例如，當連結是在自己的行上編寫時，Edge Delivery Services JavaScript會將其DOM更新為以下內容：

```html
<p class="button-container">
  <a href="/authored/link" title="Click me" class="button">Click me</a>
</p>
```

這些按鈕可以自訂以符合WKND品牌 — 這指定按鈕以帶有黑色文字的黃色矩形顯示。

以下是如何設定`styles.css`中「推斷的按鈕」樣式的範例：

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

此CSS會定義基本按鈕樣式，並包含WKND專屬的處理方式，例如大寫文字、黃色背景和黑色文字。 `background-color`和`color`屬性使用CSS變數，允許按鈕樣式與品牌顏色保持一致。 此方法可確保整個網站的按鈕樣式一致，同時保持彈性。

## 網頁字型

Edge Delivery Services專案最佳化網頁字型的使用，以維持高效能並儘量降低對Lighthouse分數的影響。 此方法可確保快速演算，而不會損害網站的視覺身分。 以下說明如何有效實作Web字型以獲得最佳效能。

### 字型

在`styles/fonts.css`檔案中使用CSS `@font-face`宣告來新增自訂網頁字型。 將`@font-faces`新增至`fonts.css`可確保網頁字型在最佳時間載入，有助於維持Lighthouse分數。

1. 開啟`styles/fonts.css`。
2. 新增下列`@font-face`宣告以包含WKND品牌字型： `Asar`和`Source Sans Pro`。

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

本教學課程中使用的字型來源為Google Fonts，但網頁字型可以從任何字型提供者取得，包括[Adobe Fonts](https://fonts.adobe.com/)。

+++使用本機Web字型檔案

或者，也可以將Web字型檔案複製到`/fonts`資料夾中的專案中，並在`@font-face`宣告中參照。

本教學課程使用遠端、託管的Web字型，因此更容易遵循。

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

最後，更新`styles/styles.css` CSS變數以使用新字型：

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

`roboto-fallback`和`roboto-condensed-fallback`是在[備援字型](#fallback-fonts)區段中更新的備援字型，以對齊支援自訂`Asar`和`Source Sans Pro`網頁字型。

### 遞補字型

網頁字型因其大小而經常影響效能，可能會增加累積版面位移(CLS)分數，並降低整體Lighthouse分數。 為了確保在Web字型載入時立即顯示文字，Edge Delivery Services專案使用瀏覽器原生備援字型。 在套用所需字型時，此方法有助於維持流暢的使用者體驗。

若要選取最佳備援字型，請使用Adobe的[Helix字型備援Chrome擴充功能](https://www.aem.live/developer/font-fallback)，該擴充功能會在自訂字型載入前，決定要供瀏覽器使用的緊密相符字型。 應將產生的遞補字型宣告新增至`styles/styles.css`檔案，以改善效能並確保使用者獲得順暢的體驗。

![Helix字型後援Chrome擴充功能](./assets/4-website-branding/font-fallback-chrome-plugin.png){align=center}

若要使用[Helix Font Fallback Chrome擴充功能](https://www.aem.live/developer/font-fallback)，請確定網頁已套用在Edge Delivery Services網站上使用的相同變數中的網頁字型。 此教學課程示範[wknd.site](http://wknd.site/us/en.html)上的擴充功能。 開發網站時，請將擴充功能套用至正在處理的網站，而非[wknd.site](http://wknd.site/us/en.html)。

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

在`styles/styles.css`中的字型CSS變數中，在「真實」字型系列名稱之後新增遞補字型系列名稱。

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

新增CSS時，AEM CLI的本機開發環境會自動重新載入變更，因此可快速輕鬆地檢視CSS對區塊的影響。

![WKND品牌CSS的開發預覽](./assets/4-website-branding/preview.png)


## 下載最終CSS檔案

您可以從下列連結下載更新的CSS檔案：

* [`styles.css`](https://raw.githubusercontent.com/davidjgonzalez/aem-wknd-eds-ue/refs/heads/main/styles/styles.css)
* [`fonts.css`](https://raw.githubusercontent.com/davidjgonzalez/aem-wknd-eds-ue/refs/heads/main/styles/fonts.css)

## 將CSS檔案加入連結

請務必[經常lint](./3-local-development-environment.md#linting)您的程式碼變更，以確保它們乾淨一致。 定期篩選有助於及早發現問題，並縮短整體開發時間。 請記住，您必須先解決所有Linting問題，才能將您的工作合併到主要分支！

```bash
$ npm run lint:css
```

## 合併程式碼變更

將變更合併至GitHub上的`main`分支，以便在這些更新上建置未來的工作。

```bash
$ git add .
$ git commit -m "Add global CSS, CSS variables, and web fonts"
$ git push origin wknd-styles
```

一旦變更推送至`wknd-styles`分支後，請在GitHub上建立提取請求，以便合併至`main`分支。

1. 從[建立新專案](./1-new-code-project.md)章節導覽至GitHub存放庫。
2. 按一下&#x200B;**提取請求**&#x200B;索引標籤，然後選取&#x200B;**新提取請求**。
3. 將`wknd-styles`設為來源分支，並將`main`設為目標分支。
4. 檢閱變更並按一下&#x200B;**建立提取要求**。
5. 在提取要求詳細資料中，**新增下列**：

   ```
   Add basic global CSS, CSS variables, and web fonts (including fallback fonts) to support the WKND brand.
   
   Fix #1
   
   Test URLs:
   - Before: https://main--wknd-aem-eds-ue--davidjgonzalez.aem.live/
   - After: https://wknd-styles--wknd-aem-eds-ue--davidjgonzalez.aem.live/
   ```

   * `Fix #1`參考了先前建立的GitHub問題。
   * 測試URL會告訴AEM程式碼同步處理要使用哪些分支來進行驗證和比較。 「之後」URL使用工作分支`wknd-styles`，以檢查程式碼變更如何影響網站效能。

6. 按一下&#x200B;**建立提取請求**。
7. 等待[AEM程式碼同步GitHub應用程式](./1-new-code-project.md)到&#x200B;**完成品質檢查**。 如果失敗，請解決錯誤並重新執行檢查。
8. 檢查通過後，**將提取要求**&#x200B;合併至`main`。

變更合併至`main`後，系統不會將它們視為部署至生產環境，新的開發工作可以根據這些更新繼續進行。
