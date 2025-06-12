---
title: 使用 CSS 和 JS 開發區塊
description: 使用 CSS 和 JavaScript 開發 Edge Delivery Services 區塊，並可以使用通用編輯器進行編輯。
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: 41c4cfcf-0813-46b7-bca0-7c13de31a20e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '772'
ht-degree: 100%

---

# 使用 CSS 和 JavaScript 開發區塊

在[先前章節](./7b-block-js-css.md)中，我們介紹僅使用 CSS 設定區塊樣式的內容。現在，將焦點轉移到使用 JavaScript 和 CSS 開發區塊。

此範例會顯示如何透過以下三種方式增強區塊：

1. 新增自訂 CSS 類別。
1. 使用事件監聽程式新增動作。
1. 處理可以選擇性納入 Teaser 文字中的條款與條件。

## 常見使用案例

這種方法在以下情境中特別有用：

- **外部 CSS 管理：**&#x200B;當您在 Edge Delivery Services 之外管理區塊的 CSS，而且與其 HTML 結構不一致時。
- **附加屬性：**&#x200B;當需要額外的屬性時，例如用於無障礙協助的 [ARIA](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA) 或 [Microdata](https://developer.mozilla.org/en-US/docs/Web/HTML/Microdata)。
- **JavaScript 增強功能：**&#x200B;當需要互動功能 (如事件監聽程式) 時。

此方法依賴瀏覽器原生的 JavaScript DOM 操作，但在修改 DOM 時需要小心，尤其是在移動元素時。這些變更可能會破壞通用編輯器的製作體驗。理想情況下，區塊的[內容模型](./5-new-block.md#block-model)應經過周全的設計，以盡量減少大量 DOM 變更的需要。

## 區塊 HTML

若要進行區塊開發，首先要檢閱 Edge Delivery Services 所公開的 DOM。該結構透過 JavaScript 得到增強，並透過 CSS 進行樣式設定。

>[!BEGINTABS]

>[!TAB 要進行修飾的 DOM]

以下是使用 JavaScript 和 CSS 進行修飾的目標 Teaser 區塊 DOM。

```html
...
<body>
    <header/>
    <main>
        <div>
            <!-- Start block HTML -->
            <div class="teaser block" data-block-name="teaser" data-block-status="loaded">
                <div>
                    <div>
                    <picture>
                        <source type="image/webp" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=2000&amp;format=webply&amp;optimize=medium" media="(min-width: 600px)">
                        <source type="image/webp" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=750&amp;format=webply&amp;optimize=medium">
                        <source type="image/jpeg" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=2000&amp;format=jpeg&amp;optimize=medium" media="(min-width: 600px)">
                        <img loading="eager" alt="Woman looking out over a river." src="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=750&amp;format=jpeg&amp;optimize=medium" width="1600" height="900">
                    </picture>
                    </div>
                </div>
                <div>
                    <div>
                    <h2 id="wknd-adventures">WKND Adventures</h2>
                    <p>Join us on one of our next adventures. Browse our list of curated experiences and sign up for one when you're ready to explore with us.</p>
                    <p class="button-container"><a href="/" title="View trips" class="button">View trips</a></p>
                    </div>
                </div>
            </div>     
            <!-- End block HTML -->
        </div>
    </main>
    <footer/>
</body>
...
```

>[!TAB 如何找到 DOM]

若要找到要修飾的 DOM，請在本機開發環境中開啟未修飾區塊的頁面，選取該區塊，然後檢查 DOM。

![檢查區塊 DOM](./assets/7a-block-css/inspect-block-dom.png)

>[!ENDTABS]


## 區塊 JavaScript

若要在區塊中新增 JavaScript 功能，請在區塊的目錄中建立一個與區塊同名的 JavaScript 檔案，例如 `/blocks/teaser/teaser.js`。

JavaScript 檔案應該會匯出一個預設函數：

```javascript
export default function decorate(block) { ... }
```

預設函數會採用代表 Edge Delivery Services HTML 中區塊的 DOM 元素/樹，並包含在轉譯區塊時執行的自訂 JavaScript。

此 JavaScript 範例執行三個主要動作：

1. 在 CTA 按鈕中新增事件監聽程式，在游標停留時放大影像。
1. 在區塊元素中新增語義 CSS 類別，於整合現有 CSS 設計系統時很有用。
1. 在以 `Terms and conditions:` 開頭的段落中加入特殊的 CSS 類別。

[!BADGE /blocks/teaser/teaser.js]{type=Neutral tooltip="下方程式碼範例的檔案名稱。"}

```javascript
/* /blocks/teaser/teaser.js */

/**
 * Adds a zoom effect to image using event listeners.
 *
 * When the CTA button is hovered over, the image zooms in.
 *
 * @param {HTMLElement} block represents the block's' DOM tree
 */
function addEventListeners(block) {
  block.querySelector('.button').addEventListener('mouseover', () => {
    block.querySelector('.image').classList.add('zoom');
  });

  block.querySelector('.button').addEventListener('mouseout', () => {
    block.querySelector('.image').classList.remove('zoom');
  });
}

/**
   * Entry point to block's JavaScript.
   * Must be exported as default and accept a block's DOM element.
   * This function is called by the project's style.js, and passed the block's element.
   *
   * @param {HTMLElement} block represents the block's' DOM element/tree
   */
export default function decorate(block) {
  /* This JavaScript makes minor adjustments to the block's DOM */

  // Dress the DOM elements with semantic CSS classes so it's obvious what they are.
  // If needed we could also add ARIA roles and attributes, or add/remove/move DOM elements.

  // Add a class to the first picture element to target it with CSS
  block.querySelector('picture').classList.add('image-wrapper');

  // Use previously applied classes to target new elements
  block.querySelector('.image-wrapper img').classList.add('image');

  // Mark the second/last div as the content area (white, bottom aligned box w/ text and cta)
  block.querySelector(':scope > div:last-child').classList.add('content');

  // Mark the first H1-H6 as a title
  block.querySelector('h1,h2,h3,h4,h5,h6').classList.add('title');

  // Process each paragraph and mark it as text or terms-and-conditions
  block.querySelectorAll('p').forEach((p) => {
    const innerHTML = p.innerHTML?.trim();

    // If the paragraph starts with Terms and conditions: then style it as such
    if (innerHTML?.startsWith("Terms and conditions:")) {
      /* If a paragraph starts with '*', add a special CSS class. */
      p.classList.add('terms-and-conditions');
    }
  });

  // Add event listeners to the block
  addEventListeners(block);
}
```

## 區塊 CSS

如果您在[先前章節](./7a-block-css.md)中已建立 `teaser.css`，請將其刪除，或將其重新命名為 `teaser.css.bak`，因為本章節會實施不同的 Teaser 區塊 CSS。

在區塊資料夾中建立一個 `teaser.css` 檔案。此檔案會包含能設定區塊樣式的 CSS 程式碼。此 CSS 程式碼目標為區塊元素，以及 `teaser.js` 中 JavaScript 所新增的特定語義 CSS 類別。

仍可直接設定裸元素樣式，或使用自訂套用的 CSS 類別。對於更複雜的區塊，套用語義 CSS 類別有助於讓 CSS 更易於理解和維護，尤其是在與更大的團隊長期合作時。

[與先前相同](./7a-block-css.md#develop-a-block-with-css)，使用 [CSS 巢狀](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_nesting)將 CSS 範圍限定為 `.block.teaser` 以避免與其他區塊衝突。

[!BADGE /blocks/teaser/teaser.css]{type=Neutral tooltip="下方程式碼範例的檔案名稱。"}

```css
/* /blocks/teaser/teaser.css */

/* Scope each selector in the block with `.block.teaser` to avoid accidental conflicts outside the block */
.block.teaser {
    animation: teaser-fade-in 1s;
    position: relative;
    width: 1600px;
    max-width: 100vw;
    left: 50%; 
    transform: translateX(-50%);
    height: 500px;
    overflow: hidden; 

    /* The teaser image */
    .image-wrapper {
        position: absolute;
        z-index: -1;
        inset: 0;
        box-sizing: border-box;
        overflow: hidden; 

        .image {
            object-fit: cover;
            object-position: center;
            width: 100%;
            height: 100%;
            transform: scale(1); 
            transition: transform 0.6s ease-in-out;

            .zoom {
                transform: scale(1.1);
            }            
        }
    }

    /* The teaser text content */
    .content {
        position: absolute;
        bottom: 0;
        left: 50%;
        transform: translateX(-50%);
        background: var(--background-color);
        padding: 1.5rem 1.5rem 1rem;
        width: 80vw;
        max-width: 1200px;
  
        .title {
            font-size: var(--heading-font-size-xl);
            margin: 0;
        }

        .title::after {
            border-bottom: 0;
        }

        p {
            font-size: var(--body-font-size-s);
            margin-bottom: 1rem;
            animation: teaser-fade-in .6s;
        
            &.terms-and-conditions {
                font-size: var(--body-font-size-xs);
                color: var(--secondary-color);
                padding: .5rem 1rem;
                font-style: italic;
                border: solid var(--light-color);
                border-width: 0 0 0 10px;
            }
        }

        /* Add underlines to links in the text */
        a:hover {
            text-decoration: underline;
        }

        /* Add specific spacing to buttons. These button CSS classes are automatically added by Edge Delivery Services. */
        .button-container {
            margin: 0;
            padding: 0;
        
            .button {   
                background-color: var(--primary-color);
                border-radius: 0;
                color: var(--dark-color);
                font-size: var(--body-font-size-xs);
                font-weight: bold;
                padding: 1em 2.5em;
                margin: 0;
                text-transform: uppercase;
            }
        }
    }
}

/** Animations 
    Scope the @keyframes to the block (teaser) to avoid accidental conflicts outside the block

    Global @keyframes can defines in styles/styles.css and used in this file.
**/
@keyframes teaser-fade-in {
    from {
        opacity: 0;
    }

    to {
        opacity: 1;
    }
}
```

## 新增條款與條件

上述實施增加對於以文字 `Terms and conditions:` 開頭之特殊樣式段落的支援。若要驗證此功能，請在通用編輯器中更新 Teaser 區塊的文字內容以包含條款與條件。

依照[製作區塊](./6-author-block.md)中的步驟進行，並編輯文字以在末尾包含&#x200B;**條款與條件**&#x200B;段落：

```
WKND Adventures

Join us on one of our next adventures. Browse our list of curated experiences and sign up for one when you're ready to explore with us.

Terms and conditions: By signing up, you agree to the rules for participation and booking.
```

驗證該段落是否有在本機開發環境中以條款與條件樣式呈現。請記得，這些程式碼變更不會反映在通用編輯器中，直到其[被推送到 GitHub 上的分支](#preview-in-universal-editor)且已設定可供通用編輯器使用為止。

## 開發預覽

在加入 CSS 和 JavaScript 之後，AEM CLI 的本機開發環境會即時重新載入相關變更，因而可以快速又輕鬆地將程式碼對區塊的影響以視覺化圖形呈現。將游標停留在 CTA 上，檢查並確認 Teaser 的影像會放大和縮小顯示。

![使用 CSS 和 JS 進行 Teaser 的本機開發預覽](./assets/7b-block-js-css/local-development-preview.png)

## 對程式碼進行 lint 檢查

務必針對您的程式碼變更[經常進行 lint 檢查](./3-local-development-environment.md#linting)，保持程式碼整潔且一致。定期進行 lint 檢查有助於及早發現問題，進而減少整體開發時間。請記住，在解決所有 linting 問題以前，您不能將開發工作合併到 `main` 分支內！

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

## 在通用編輯器中預覽

若要在 AEM 通用編輯器中檢視變更，請將其新增、提交並推送至通用編輯器所使用的 Git 存放庫分支。這樣做可以確保區塊實施不會破壞製作體驗。

```bash
# ~/Code/aem-wknd-eds-ue

$ git add .
$ git commit -m "CSS and JavaScript implementation for teaser block"
$ git push origin teaser
```

現在，當您新增 `?ref=teaser` 查詢參數時，您可以在通用編輯器中預覽變更。

![通用編輯器中的 Teaser](./assets/7b-block-js-css/universal-editor-preview.png)
