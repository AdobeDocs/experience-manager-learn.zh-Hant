---
title: 頁首與頁尾
description: 瞭解如何在Edge Delivery Services和Universal Editor中開發頁首和頁尾。
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
jira: KT-17470
duration: 300
exl-id: 70ed4362-d4f1-4223-8528-314b2bf06c7c
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1207'
ht-degree: 0%

---

# 開發頁首和頁尾

![頁首與頁尾](./assets/header-and-footer/hero.png){align="center"}

頁首和頁尾在Edge Delivery Services (EDS)中起著唯一的作用，因為它們直接繫結到HTML `<header>`和`<footer>`元素。 不同於一般頁面內容，這些檔案會個別管理，且不需整個頁面快取即可獨立更新。 雖然作者的實作在`blocks/header`和`blocks/footer`下的程式碼專案中是區塊，但作者可以透過包含任何區塊組合的專用AEM頁面編輯其內容。

## 標題區塊

![標頭區塊](./assets/header-and-footer/header-local-development-preview.png){align="center"}

標頭是繫結至Edge Delivery Services HTML `<header>`元素的特殊區塊。
`<header>`元素會傳送為空白，並透過XHR (AJAX)填入至個別的AEM頁面。
如此一來，標題就能與頁面內容分開管理，且無需完全清除所有頁面快取即可更新。

標頭區塊負責請求包含標頭內容的AEM頁面片段，並在`<header>`元素中呈現該片段。

[!BADGE /blocks/header/header.js]{type=Neutral tooltip="以下程式碼範例的檔案名稱。"}

```javascript
import { getMetadata } from '../../scripts/aem.js';
import { loadFragment } from '../fragment/fragment.js';

...

export default async function decorate(block) {
  // load nav as fragment

  // Get the path to the AEM page fragment that defines the header content from the <meta name="nav"> tag. This is set via the site's Metadata file.
  const navMeta = getMetadata('nav');

  // If the navMeta is not defined, use the default path `/nav`.
  const navPath = navMeta ? new URL(navMeta, window.location).pathname : '/nav';

  // Make an XHR (AJAX) call to request the AEM page fragment and serialize it to a HTML DOM tree.
  const fragment = await loadFragment(navPath);
  
  // Add the content from the fragment HTML to the block and decorate it as needed
  ...
}
```

`loadFragment()`函式向`${navPath}.plain.html`發出XHR (AJAX)要求，而要求會傳回存在於頁面`<main>`標籤中AEM頁面HTML的EDS HTML轉譯、使用可能包含的任何區塊處理其內容，以及傳回更新的DOM樹狀結構。

## 編寫頁首頁面

在開發標題區塊之前，請先在通用編輯器中編寫其內容，以便針對其進行開發。

標題內容位於名為`nav`的專用AEM頁面中。

![預設頁首頁](./assets/header-and-footer/header-page.png){align="center"}

若要編寫標題：

1. 在通用編輯器中開啟`nav`頁面
1. 以包含WKND標誌的&#x200B;**影像區塊**&#x200B;取代預設按鈕
1. 請以下列方式更新&#x200B;**文字區塊**&#x200B;中的導覽功能表：
   - 新增您想要的導覽連結
   - 視需要建立子導覽專案
   - 現在正在設定首頁(`/`)的所有連結

通用編輯器中的![作者標題區塊](./assets/header-and-footer/header-author.png){align="center"}

### 發佈以預覽

更新頁首頁面後，[發佈頁面以預覽](../6-author-block.md)。

由於頁首內容存在於其本身的頁面（`nav`頁面）上，因此您必須發佈該頁面，頁首變更才會生效。 發佈使用標頭的其他頁面將不會更新Edge Delivery Services上的標頭內容。

## 封鎖HTML

若要開始區塊開發，請先檢閱Edge Delivery Services預覽所公開的DOM結構。 DOM已透過JavaScript增強，並採用CSS樣式，為建置和自訂區塊提供基礎。

由於標頭已載入為片段，因此我們需要在透過`loadFragment()`插入DOM並裝飾後，檢查XHR要求傳回的HTML。 這可透過檢查瀏覽器開發人員工具中的DOM來完成。


>[!BEGINTABS]

>要裝飾的[!TAB DOM]

以下是頁首頁面使用提供的`header.js`載入並插入DOM中的HTML：

```html
<header class="header-wrapper">
  <div class="header block" data-block-name="header" data-block-status="loaded">
    <div class="nav-wrapper">
      <nav id="nav" aria-expanded="true">
        <div class="nav-hamburger">
          <button type="button" aria-controls="nav" aria-label="Close navigation">
            <span class="nav-hamburger-icon"></span>
          </button>
        </div>
        <div class="section nav-brand" data-section-status="loaded" style="">
          <div class="default-content-wrapper">
            <p class="">
              <a href="#" title="Button" class="">Button</a>
            </p>
          </div>
        </div>
        <div class="section nav-sections" data-section-status="loaded" style="">
          <div class="default-content-wrapper">
            <ul>
              <li aria-expanded="false">Examples</li>
              <li aria-expanded="false">Getting Started</li>
              <li aria-expanded="false">Documentation</li>
            </ul>
          </div>
        </div>
        <div class="section nav-tools" data-section-status="loaded" style="">
          <div class="default-content-wrapper">
            <p>
              <span class="icon icon-search">
                <img data-icon-name="search" src="/icons/search.svg" alt="" loading="lazy">
              </span>
            </p>
          </div>
        </div>
      </nav>
    </div>
  </div>
</header>
```

>[!TAB 如何尋找DOM]

若要在網頁瀏覽器的開發人員工具中尋找及檢查頁面的`<header>`元素。

![標頭DOM](./assets/header-and-footer/header-dom.png){align="center"}

>[!ENDTABS]


## 封鎖JavaScript

來自[AEM Boilerplate XWalk專案範本](https://github.com/adobe-rnd/aem-boilerplate-xwalk)的`/blocks/header/header.js`檔案提供JavaScript以進行導覽，包括下拉式功能表和回應式行動檢視。

雖然`header.js`指令碼經常大量自訂以符合網站設計，但必須保留`decorate()`中的第一行，以擷取及處理標頭頁面片段。

[!BADGE /blocks/header/header.js]{type=Neutral tooltip="以下程式碼範例的檔案名稱。"}

```javascript
export default async function decorate(block) {
  // load nav as fragment
  const navMeta = getMetadata('nav');
  const navPath = navMeta ? new URL(navMeta, window.location).pathname : '/nav';
  const fragment = await loadFragment(navPath);
  ...
```

剩餘的程式碼可以修改以符合您的專案需求。

根據頁首需求，可以調整或移除樣板程式碼。 在本教學課程中，我們將使用提供的程式碼，並透過在第一個編寫的影像周圍新增超連結，將其連結至網站的首頁來增強程式碼。

範本的程式碼會處理頁首頁面片段，假設它依下列順序包含三個區段：

1. **品牌區段** — 包含標誌且樣式為`.nav-brand`類別。
2. **區段** — 定義網站的主要功能表並以`.nav-sections`設定樣式。
3. **工具區段** — 包含搜尋、登入/登出和設定檔等專案，樣式為`.nav-tools`。

若要將標誌影像超連結至首頁，我們會更新區塊JavaScript，如下所示：

>[!BEGINTABS]

>[!TAB 已更新JavaScript]

更新後的標誌影像包裝程式碼會包含網站首頁(`/`)的連結，如下所示：

[!BADGE /blocks/header/header.js]{type=Neutral tooltip="以下程式碼範例的檔案名稱。"}

```javascript
export default async function decorate(block) {

  ...
  const navBrand = nav.querySelector('.nav-brand');
  
  // WKND: Turn the picture (image) into a linked site logo
  const logo = navBrand.querySelector('picture');
  
  if (logo) {
    // Replace the first section's contents with the authored image wrapped with a link to '/' 
    navBrand.innerHTML = `<a href="/" aria-label="Home" title="Home" class="home">${logo.outerHTML}</a>`;
    // Make sure the logo is not lazy loaded as it's above the fold and can affect page load speed
    navBrand.querySelector('img').settAttribute('loading', 'eager');
  }

  const navSections = nav.querySelector('.nav-sections');
  if (navSections) {
    // WKND: Remove Edge Delivery Services button containers and buttons from the nav sections links
    navSections.querySelectorAll('.button-container, .button').forEach((button) => {
      button.classList = '';
    });

    ...
  }
  ...
}
```

>[!TAB 原始JavaScript]

以下是從範本產生的原始`header.js`：

[!BADGE /blocks/header/header.js]{type=Neutral tooltip="以下程式碼範例的檔案名稱。"}

```javascript
export default async function decorate(block) {
  ...
  const navBrand = nav.querySelector('.nav-brand');
  const brandLink = navBrand.querySelector('.button');
  if (brandLink) {
    brandLink.className = '';
    brandLink.closest('.button-container').className = '';
  }

  const navSections = nav.querySelector('.nav-sections');
  if (navSections) {
    navSections.querySelectorAll(':scope .default-content-wrapper > ul > li').forEach((navSection) => {
      if (navSection.querySelector('ul')) navSection.classList.add('nav-drop');
      navSection.addEventListener('click', () => {
        if (isDesktop.matches) {
          const expanded = navSection.getAttribute('aria-expanded') === 'true';
          toggleAllNavSections(navSections);
          navSection.setAttribute('aria-expanded', expanded ? 'false' : 'true');
        }
      });
    });
  }
  ...
}
```

>[!ENDTABS]


## 封鎖CSS

更新`/blocks/header/header.css`以根據WKND的品牌設定其樣式。

我們將新增自訂CSS至`header.css`底部，讓教學課程變更更容易檢視和理解。 雖然這些樣式可直接整合至範本的CSS規則，但將其保持獨立有助於說明修改內容。

由於我們是在原始規則集之後新增規則，因此我們將使用`header .header.block nav` CSS選取器來包住規則，以確保規則優先於範本規則。

[!BADGE /blocks/header/header.css]{type=Neutral tooltip="以下程式碼範例的檔案名稱。"}

```css
/* /blocks/header/header.css */

... Existing CSS generated by the template ...

/* Add the following CSS to the end of the header.css */

/** 
* WKND customizations to the header 
* 
* These overrides can be incorporated into the provided CSS,
* however they are included discretely in thus tutorial for clarity and ease of addition.
* 
* Because these are added discretely
* - They are added to the bottom to override previous styles.
* - They are wrapped in a header .header.block nav selector to ensure they have more specificity than the provided CSS.
* 
**/

header .header.block nav {
  /* Set the height of the logo image.
     Chrome natively sets the width based on the images aspect ratio */
  .nav-brand img {
    height: calc(var(--nav-height) * .75);
    width: auto;
    margin-top: 5px;
  }
  
  .nav-sections {
    /* Update menu items display properties */
    a {
      text-transform: uppercase;
      background-color: transparent;
      color: var(--text-color);
      font-weight: 500;
      font-size: var(--body-font-size-s);
    
      &:hover {
        background-color: auto;
      }
    }

    /* Adjust some spacing and positioning of the dropdown nav */
    .nav-drop {
      &::after {
        transform: translateY(-50%) rotate(135deg);
      }
      
      &[aria-expanded='true']::after {
        transform: translateY(50%) rotate(-45deg);
      }

      & > ul {
        top: 2rem;
        left: -1rem;      
       }
    }
  }
```

## 開發預覽

CSS和JavaScript開發後，AEM CLI的本機開發環境會重新載入變更，讓您快速輕鬆地視覺化程式碼如何影響區塊。 將游標暫留在CTA上，並驗證Teaser的影像是否會放大和縮小。

![使用CSS和JS的標頭本機開發預覽](./assets/header-and-footer/header-local-development-preview.png){align="center"}

## 將程式碼插入

請務必[經常lint](../3-local-development-environment.md#linting)您的程式碼變更，以保持其整齊一致。 定期篩選有助於及早發現問題，減少整體開發時間。 請記住，您必須先解決所有Linting問題，才能將您的開發工作合併至`main`分支！

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

## 在通用編輯器中預覽

若要在AEM的通用編輯器中檢視變更，請新增、提交變更，並將其推送到通用編輯器使用的Git存放庫分支。 這麼做可確保區塊實作不會中斷編寫體驗。

```bash
# ~/Code/aem-wknd-eds-ue

$ git add .
$ git commit -m "CSS and JavaScript implementation for Header block"
# JSON files are compiled automatically and added to the commit via a Husky pre-commit hook
$ git push origin header-and-footer
```

現在，使用`?ref=header-and-footer`查詢引數時，可以在通用編輯器中看到變更。

通用編輯器中的![標題](./assets/header-and-footer/header-universal-editor-preview.png){align="center"}

## 頁尾

如同頁首，頁尾內容是在專用的AEM頁面上撰寫 — 在此案例中是頁尾頁面(`footer`)。 頁尾遵循與載入為片段相同的模式，並以CSS和JavaScript裝飾。

>[!BEGINTABS]

>[!TAB 頁尾]

頁尾應以包含下列專案的三欄版面配置實施：

- 包含促銷活動（影像和文字）的左欄
- 帶有導覽連結的中間欄
- 包含社群媒體連結的右側欄
- 底端橫跨所有三欄的資料列，享有著作權

![頁尾預覽](./assets/header-and-footer/footer-preview.png){align="center"}

>[!TAB 頁尾內容]

在「頁尾」頁面中使用資料欄區塊來建立三欄效果。

| 欄1 | 欄2 | 欄目3 |
| ---------|----------------|---------------|
| 影像 | 標題 3 | 標題 3 |
| 文字 | 連結清單 | 連結清單 |

![標頭DOM](./assets/header-and-footer/footer-author.png){align="center"}

>[!TAB 頁尾代碼]

下方的CSS以三欄版面配置、一致間距和印刷樣式來設定頁尾區塊的樣式。 頁尾實施僅使用範本提供的JavaScript。

[!BADGE /blocks/footer/footer.css]{type=Neutral tooltip="以下程式碼範例的檔案名稱。"}

```css
/* /blocks/footer/footer.css */

footer {
  background-color: var(--light-color);

  .block.footer {
    border-top: solid 1px var(--dark-color);
    font-size: var(--body-font-size-s);

    a { 
      all: unset;
      
      &:hover {
        text-decoration: underline;
        cursor: pointer;
      }
    }

    img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      border: solid 1px white;
    }

    p {
      margin: 0;
    }

    ul {
      list-style: none;
      padding: 0;
      margin: 0;

      li {
        padding-left: .5rem;
      }
    }

    & > div {
      margin: auto;
      max-width: 1200px;
    }

    .columns > div {
      gap: 5rem;
      align-items: flex-start;

      & > div:first-child {
        flex: 2;
      }
    }

    .default-content-wrapper {
      padding-top: 2rem;
      margin-top: 2rem;
      font-style: italic;
      text-align: right;
    }
  }
}

@media (width >= 900px) {
  footer .block.footer > div {
    padding: 40px 32px 24px;
  }
}
```


>[!ENDTABS]

## 恭喜！

您現在已探索如何在Edge Delivery Services和Universal Editor中管理和開發頁首和頁尾。 您已瞭解其運作方式：

- 在獨立於主要內容的專用AEM頁面上撰寫
- 以片段非同步載入，以啟用獨立更新
- 以JavaScript和CSS裝飾，建立回應式導覽體驗
- 與通用編輯器緊密整合，方便的內容管理

此模式提供靈活且可維護的方法，用於實作全網站的導覽元件。

如需更多最佳實務和進階技術，請參閱[通用編輯器檔案](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/create-block#block-options)。
