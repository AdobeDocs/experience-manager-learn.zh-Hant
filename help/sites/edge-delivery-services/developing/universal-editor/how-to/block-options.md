---
title: 區塊選項
description: 了解如何建置具有多個顯示選項的區塊。
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-17296
duration: 700
exl-id: f41dff22-bd47-4ea0-98cc-f5ca30b22c4b
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1961'
ht-degree: 100%

---

# 開發具有選項的區塊

本教學課程以 Edge Delivery Services 和通用編輯器教學課程為基礎進行建置，引導您完成在區塊中新增區塊選項的流程。透過定義區塊選項，您可以自訂區塊的外觀和功能，讓不同的變化版本可以滿足各種內容需求。網站的設計系統因此具備更高的彈性和再使用性。

![並排區塊選項](./assets/block-options/main.png){align="center"}

在本教學課程中，您將在 Teaser 區塊中新增區塊選項，為作者提供&#x200B;**預設**&#x200B;和&#x200B;**並排**&#x200B;兩種顯示選項。**預設**&#x200B;選項會在文字的上方和背面顯示影像，而&#x200B;**並排**&#x200B;選項則會並排顯示影像和文字。

## 常見使用案例

在開發 **Edge Delivery Services** 和&#x200B;**通用編輯器**&#x200B;時使用&#x200B;**區塊選項**&#x200B;的常見使用案例包括但不限於：

1. **版面變化版本：**&#x200B;輕鬆切換至不同版面。例如，切換成水平或垂直，或切換成網格或清單。
2. **樣式變化版本：**&#x200B;輕鬆切換成不同的主題或視覺處理方式。例如，切換成明亮或陰暗模式，或切換成大型字體或小型字體。
3. **內容顯示控制：**&#x200B;切換元素的可見度，或切換成不同的內容樣式 (精簡或詳細)。

這些選項提升建置動態和適應性區塊的彈性和效率。

本教學課程示範版面變化版本的使用案例，其中 Teaser 區塊可以用&#x200B;**預設**&#x200B;和&#x200B;**並排**&#x200B;兩種不同的版面顯示。

## 區塊模型

若要在 Teaser 區塊中新增區塊選項，請於 `/block/teaser/_teaser.json` 開啟其 JSON 片段，並在模型定義中新增一個欄位。此欄位的 `name` 屬性設定為 `classes`，這是一個受保護的欄位，AEM 會在這個欄位中儲存套要用至區塊的 Edge Delivery Services HTML 的區塊選項。

### 欄位設定

下方的索引標籤會說明在區塊模型中設定區塊選項的各種方法，包括使用單一 CSS 類別的單一選取、使用多個 CSS 類別的單一選取，以及使用多個 CSS 類別的多重選取。本教學課程會[實作&#x200B;**使用單一 CSS 類別的選取**&#x200B;中所使用的較簡單方法](#field-configuration-for-this-tutorial)。

>[!BEGINTABS]

>[!TAB 使用單一 CSS 類別的選取]

本教學課程示範如何使用 `select` (下拉式) 輸入類型，讓作者能夠選擇單一區塊選項，而後套用為單一相對應的 CSS 類別。

![使用單一 CSS 類別的選取](./assets/block-options/tab-1.png){align="center"}

#### 區塊模型

「**預設**」選項以空字串 (`""`) 表示，而「**並排**」選項使用 `"side-by-side"`。此選項的&#x200B;**名稱**&#x200B;和&#x200B;**值**&#x200B;不一定要相同，但套用於區塊 HTML 的 CSS 類別會由&#x200B;**值**&#x200B;來決定。例如，「**並排**」選項的值可能是 `layout-10` 而非 `side-by-side`。但是，CSS 類別最好使用語義上有意義的名稱，以確保選項的值明確且維持一致性。

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="下方程式碼範例的檔案名稱。"}

```json{highlight="4,8,9-18"}
...
"fields": [
    {
        "component": "select",
        "name": "classes",
        "value": "",
        "label": "Teaser options",
        "valueType": "string",
        "options": [
            {
                "name": "Default",
                "value": ""
            },
            {
                "name": "Side-by-side",
                "value": "side-by-side"
            }
        ]
    }
]
...
```

#### 區塊 HTML

作者選取一個選項後，區塊 HTML 中即會新增相對應的值作為 CSS 類別：

- 若選取「**預設**」：

  ```html
  <div class="block teaser">
      <!-- Block content here -->
  </div>
  ```

- 若選取「**並排**」：

  ```html
  <div class="block teaser side-by-side">
      <!-- Block content here -->
  </div>
  ```

因此可以根據所選的選項套用不同的樣式和條件式 JavaScript。


>[!TAB 使用多個 CSS 類別的選取]

**本教學課程中並未使用此方法，但會舉例說明替代方法和進階區塊選項。**

`select` 輸入類型讓作者能夠選擇單一區塊選項，並可以選擇對應到多個 CSS 類別。若要達到此目的，請將 CSS 類別以空格分隔的值列出。

![使用多個 CSS 類別的選取](./assets/block-options/tab-2.png){align="center"}

#### 區塊模型

例如，「**並排**」選項可以支援讓影像出現在左側 (`side-by-side left`) 或右側 (`side-by-side right`) 的變化版本。

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="下方程式碼範例的檔案名稱。"}

```json{highlight="4,8,9-21"}
...
"fields": [
    {
        "component": "select",
        "name": "classes",
        "value": "",
        "label": "Teaser options",
        "valueType": "string",
        "options": [
            {
                "name": "Default",
                "value": ""
            },
            {
                "name": "Side-by-side with Image on left",
                "value": "side-by-side left"
            },
            {
                "name": "Side-by-side with Image on right",
                "value": "side-by-side right"
            }
        ]
    }
]
...
```

#### 區塊 HTML

作者選取一個選項後，區塊 HTML 便會套用對應值作為以空格分隔的一組 CSS 類別：

- 若選取「**預設**」：

  ```html
  <div class="block teaser">
      <!-- Block content here -->
  </div>
  ```

- 若選取「**並排且影像在左側**」：

  ```html
  <div class="block teaser side-by-side left">
      <!-- Block content here -->
  </div>
  ```

- 若選取「**並排且影像在右側**」：

  ```html
  <div class="block teaser side-by-side right">
      <!-- Block content here -->
  </div>
  ```

因此可以根據所選的選項套用不同的樣式和條件式 JavaScript。


>[!TAB 使用多個 CSS 類別的多重選取]

**本教學課程中並未使用此方法，但會舉例說明替代方法和進階區塊選項。**

輸入類型 `"component": "multiselect"` 讓作者可以同時選取多個選項。這樣一來，只要結合多種設計選擇，區塊外觀便可以進行複雜的排列組合。

![使用多個 CSS 類別的多重選取](./assets/block-options/tab-3.png){align="center"}

### 區塊模型

例如，「**並排**」、「**影像在左側**」和「**影像在右側**」可以支援將影像放置在左側 (`side-by-side left`) 或右側 (`side-by-side right`) 的變化版本。

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="下方程式碼範例的檔案名稱。"}

```json{highlight="4,6,8,10-21"}
...
"fields": [
    {
        "component": "multiselect",
        "name": "classes",
        "value": [],
        "label": "Teaser options",
        "valueType": "array",
        "options": [
            {
                "name": "Side-by-side",
                "value": "side-by-side"
            },
            {
                "name": "Image on left",
                "value": "left"
            },
            {
                "name": "Image on right",
                "value": "right"
            }
        ]
    }
]
...
```

#### 區塊 HTML

作者選取多個選項後，區塊 HTML 便會套用相對應的值作為以空格分隔的 CSS 類別：

- 若選取「**並排**」和「**影像在左側**」：

  ```html{highlight="1"}
  <div class="block teaser side-by-side left">
      <!-- Block content here -->
  </div>
  ```

- 若選取「**並排**」和「**影像在右側**」：

  ```html{highlight="1"}
  <div class="block teaser side-by-side right">
      <!-- Block content here -->
  </div>
  ```

雖然多重選取能提供彈性，但是管理設計的排列組合也是複雜的工作。若無限制，相互衝突的選取內容可能會導致不連續或不符合品牌形象的體驗。

例如：

- 若選擇「**影像在左側**」或「**影像在右側**」但未選取「**並排**」，則會在默示情況下套用「**預設**」，而由於預設選項一律將影像設定為背景，因此左側和右側的對齊方式不會產生作用。
- 同時選取「**影像在左側**」和「**影像在右側**」是相互矛盾的。
- 選取「**並排**」卻未選取「**影像在左側**」或「**影像在右側**」，可能被認為是設定不明確，因為沒有指定影像的位置。

為了避免出現問題，也不要讓作者在使用多重選取時感到困惑，請確保選項經過精心規劃，並且所有排列組合都經過測試。多重選取最適合簡單且不會衝突的增強選項，例如「大」或「醒目提示」等，而比較不適合版面變化的選項。


>[!TAB 預設選項]

**本教學課程中並未使用此方法，但會舉例說明替代方法和進階區塊選項。**

在通用編輯器中，要在頁面中新增區塊實例時，可以將區塊選項設定為預設值。方法是在[區塊定義](../5-new-block.md#block-definition)中設定 `classes` 屬性的預設值。

#### 區塊定義

在下方範例中，把 `classes` 欄位的 `value` 屬性指派為 `side-by-side`，即可把預設選項設定為「**並排**」。您可以選擇在區塊模型中輸入相對應的區塊選項。

您也可以為同一個區塊定義多個項目，每個項目具有不同的名稱和類別。這樣一來，通用編輯器可以顯示不同的區塊項目，每個項目都預先設定好特定的區塊選項。雖然這些在編輯器中顯示為個別區塊，但程式碼基底包含一個會根據所選選項而動態轉譯的單一區塊。

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="下方程式碼範例的檔案名稱。"}

```json{highlight="12"}
{
    "definitions": [{
      "title": "Teaser",
      "id": "teaser",
      "plugins": {
        "xwalk": {
          "page": {
            "resourceType": "core/franklin/components/block/v1/block",
            "template": {
              "name": "Teaser",
              "model": "teaser",
              "classes": "side-by-side",
              "textContent_text": "<h2>Enter a title</h2><p>...and body text here!</p>",
              "textContent_cta": "/",
              "textContent_ctaText": "Click me!"
            }
          }
        }
      }
    }],
    "models": [... from previous section ...],
    "filters": []
}
```

>[!ENDTABS]


### 本教學課程的欄位設定


在本教學課程中，我們會採取在上述第一個索引標籤內說明的使用單一 CSS 類別的選取的方法，因此可以使用兩個獨立的區塊選項：「**預設**」和「**並排**」。

在區塊 JSON 片段內的模型定義中，在區塊選項中新增一個單一選取欄位。作者可以透過這個欄位選擇預設版面或並排版面。

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="下方程式碼範例的檔案名稱。"}

```json{highlight="7-24"}
{
    "definitions": [...],
    "models": [
        {
            "id": "teaser", 
            "fields": [
                {
                    "component": "select",
                    "name": "classes",
                    "value": "",
                    "label": "Teaser options",
                    "description": "",
                    "valueType": "string",
                    "options": [
                        {
                            "name": "Default",
                            "value": ""
                        },
                        {
                            "name": "Side-by-side",
                            "value": "side-by-side"
                        }
                    ]
                },
                {
                    "component": "reference",
                    "valueType": "string",
                    "name": "image",
                    "label": "Image",
                    "multi": false
                },
                {
                    "component": "text",
                    "valueType": "string",
                    "name": "imageAlt",
                    "label": "Image alt text",
                    "required": true
                },
                {
                    "component": "richtext",
                    "name": "textContent_text",
                    "label": "Text",
                    "valueType": "string",
                    "required": true
                },
                {
                    "component": "aem-content",
                    "name": "textContent_cta",
                    "label": "CTA",
                    "valueType": "string"
                },
                {
                    "component": "text",
                    "name": "textContent_ctaText",
                    "label": "CTA label",
                    "valueType": "string"
                }
            ]
        }
    ],
    "filters": []
}
```

## 在通用編輯器中更新區塊

若要在通用編輯器中使用更新的區塊選項輸入，請將 JSON 程式碼變更部署至 GitHub，建立一個新頁面，使用「**並排**」選項新增和製作 Teaser 區塊，然後發佈該頁面進行預覽。發佈完畢後，在本機開發環境中載入頁面進行程式碼編寫。

### 將變更推送至 GitHub

若要在通用編輯器中使用更新後的區塊選項輸入，以利設定區塊選項並針對所產生的 HTML 進行開發，必須對專案進行 lint 檢查，並將變更推送至 GitHub 分支 (在此案例中，是指 `block-options` 分支)。

```bash
# ~/Code/aem-wknd-eds-ue

# Lint the changes to catch any syntax errors
$ npm run lint 

$ git add .
$ git commit -m "Add Teaser block option to JSON file so it is available in Universal Editor"
$ git push origin teaser
```

### 建立測試頁面

在 AEM Author 服務中，建立一個新頁面以新增供開發使用的 Teaser 區塊。遵循 [Edge Delivery Services 和通用編輯器開發人員教學課程](../0-overview.md)的[製作區塊](../6-author-block.md)章節中的慣例，在 `branches` 頁面下建立一個測試頁面，並以您正在處理的 Git 分支命名 (在此案例中為 `block-options`)。

### 製作區塊

於通用編輯器中編輯新的&#x200B;**區塊選項**&#x200B;頁面，並新增 **Teaser** 區塊。務必在 URL 中新增查詢參數 `?ref=block-options`，以便使用來自 `block-options` GitHub 分支的程式碼載入頁面。

區塊對話框現在包含一個「**Teaser 選項**」下拉式選單，其中有「**預設**」和「**並排**」選項。選擇「**並排**」，然後完成其餘的內容製作。

![Teaser 和選項區塊對話框](./assets/block-options/block-dialog.png){align="center"}

或者，新增兩個 **Teaser** 區塊，一個設定為「**預設**」，另一個設定為「**並排**」。這樣您就可以在開發過程中並排預覽這兩個選項，確保實施「**並排**」不會影響到「**預設**」選項。

### 發佈至預覽

在頁面中新增 Teaser 區塊後，使用「**發佈**」按鈕[將頁面發佈至預覽](../6-author-block.md)，然後在通用編輯器中選擇發佈至「**預覽**」。

## 區塊 HTML

若要開始進行區塊開發，首先請檢閱 Edge Delivery Services 預覽版所公開的 DOM 結構。DOM 透過 JavaScript 增強，並使用 CSS 設定樣式，為建置和自訂區塊打下基礎。

>[!BEGINTABS]

>[!TAB 要進行修飾的 DOM]

以下是 Teaser 區塊的 DOM，已選取「**並排**」區塊選項，此為要使用 JavaScript 和 CSS 進行修飾的目標。

```html{highlight="7"}
...
<body>
    <header/>
    <main>
        <div>
            <!-- Start block HTML -->
            <div class="teaser block side-by-side" data-block-name="teaser" data-block-status="loaded">
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
                    <p>Terms and conditions: By signing up, you agree to the rules for participation and booking.</p>
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

若要找到要修飾的 DOM，請在本機開發環境中開啟含有該區塊的頁面，使用網頁瀏覽器的開發人員工具選取該區塊，然後檢查 DOM。如此，您便能夠找出要進行修飾的相關元素。

![檢查區塊 DOM](./assets/block-options/dom.png){align="center"}

>[!ENDTABS]

## 區塊 CSS

編輯 `blocks/teaser/teaser.css`，為「**並排**」選項加入特定 CSS 樣式。此檔案包含該區塊的預設 CSS。

若要修改「**並排**」選項的樣式，請在 `teaser.css` 檔案中加入一個限定範圍的新 CSS 規則，以使用 `side-by-side` 類別進行設定的 Teaser 區塊為目標。

```css
.block.teaser.side-by-side { ... }
```

或者，您可以使用 CSS 巢狀內嵌來設定更簡潔的版本：

```css
.block.teaser {
    ... Default teaser block styles ...

    &.side-by-side {
        ... Side-by-side teaser block styles ...
    }
}
```

在 `&.side-by-side` 規則內，新增必要的 CSS 屬性，以便在套用 `side-by-side` 類別時設定該區塊的樣式。

常見的方法來重設預設樣式，只要將 `all: initial` 套用至共用選擇器，然後為 `side-by-side` 變體新增所需的樣式即可。若大多數的樣式皆由多個選項共用，則覆寫特定屬性可能會更為容易。不過，如果有多個選擇器需要變更，則重設所有樣式並僅重新套用必要的樣式，可以讓程式碼更清晰且更易於維護。
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
        }

        p.terms-and-conditions {
            font-size: var(--body-font-size-xs);
            color: var(--secondary-color);
            padding: .5rem 1rem;
            font-style: italic;
            border: solid var(--light-color);
            border-width: 0 0 0 10px;
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

    /**
    *  Add styling for the side-by-side variant 
    **/

    /* This evaluates to .block.teaser.side-by-side */
    &.side-by-side {    
        /* Since this default teaser option doesn't have a style (such as `.default`), we use `all: initial` to reset styles rather than overriding individual styles. */
        all: initial;
        display: flex;
        margin: auto;
        max-width: 900px;

        .image-wrapper {
            all: initial;
            flex: 2;
            overflow: hidden;                 
            
            * {
                height: 100%;
            }        

            .image {
                object-fit: cover;
                object-position: center;
                width: 100%;
                height: 100%;
                transform: scale(1); 
                transition: transform 0.6s ease-in-out;                

                &.zoom {
                    /* This option has a different zoom level than the default */
                    transform: scale(1.5);
                }
            }
        }

        .content {
            all: initial;
            flex: 1;
            background-color: var(--light-color);
            padding: 3.5em 2em 2em;
            font-size: var(--body-font-size-s);
            font-family: var(--body-font-family);
            text-align: justify;
            text-justify: newspaper;
            hyphens: auto;

            p.terms-and-conditions {
                border: solid var(--text-color);
                border-width: 0;
                padding-left: 0;
                text-align: left;
            }
        }

        /* Media query for mobile devices */
        @media (width <= 900px) {
            flex-direction: column; /* Stack elements vertically on mobile */
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


## 區塊 JavaScript

只要查看套用於區塊元素的類別，即可直接確認區塊的作用中選項。在這個例子中，我們需要根據作用中選項調整 `.image-wrapper` 樣式的套用位置。

`getOptions` 函數會傳回套用於該區塊的類別陣列，但不包括 `block` 和 `teaser` (因為所有區塊都有 `block` 類別，而且所有 Teaser 區塊都有 `teaser` 類別)。陣列中其餘的任何類別便是使用中選項。如果陣列為空，則是套用預設選項。

```javascript
function getOptions(block) {
  // Get the block's classes, excluding 'block' and 'teaser'; anything remaining is a block option.
  return [...block.classList].filter((c) => !['block', 'teaser'].includes(c));
}
```

您可以使用這份選項清單，在區塊的 JavaScript 中依照條件執行自訂邏輯：

```javascript
if (getOptions(block).includes('side-by-side')) {
  /* For side-by-side teaser, add the image-wrapper to a higher-level div to support CSS */
  block.querySelector(':scope > div:first-child').classList.add('image-wrapper');
} else if (!getOptions(block)) {
  /* For the default option, add the image-wrapper to the picture element to support CSS */
  block.querySelector('picture').classList.add('image-wrapper');
}
```

包含「預設」和「並排」兩個選項的 Teaser 區塊完整更新的 JavaScript 檔案如下：

[!BADGE /blocks/teaser/teaser.js]{type=Neutral tooltip="下方程式碼範例的檔案名稱。"}

```javascript
/* /blocks/teaser/teaser.js */

/**
 * Block options are applied as classes to the block's DOM element
 * alongside the `block` and `<block-name>` classes.
 *
 * @param {HTMLElement} block represents the block's DOM element/tree
 */
function getOptions(block) {
  // Get the block's classes, excluding 'block' and 'teaser'.
  return [...block.classList].filter((c) => !['block', 'teaser'].includes(c));
}

/**
 * Adds a zoom effect to the image using event listeners.
 *
 * When the CTA button is hovered over, the image zooms in.
 *
 * @param {HTMLElement} block represents the block's DOM tree
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
 * Entry point to the block's JavaScript.
 * Must be exported as default and accept a block's DOM element.
 * This function is called by the project's style.js, passing the block's element.
 *
 * @param {HTMLElement} block represents the block's DOM element/tree
 */
export default function decorate(block) {
  /* Common treatments for all options */
  block.querySelector(':scope > div:last-child').classList.add('content');
  block.querySelector('h1,h2,h3,h4,h5,h6').classList.add('title');
  block.querySelector('img').classList.add('image');

  // Process each paragraph and mark it as text or terms-and-conditions
  block.querySelectorAll('p').forEach((p) => {
    const innerHTML = p.innerHTML?.trim();
    if (innerHTML?.startsWith('Terms and conditions:')) {
      p.classList.add('terms-and-conditions');
    }
  });

  /* Conditional treatments for specific options */
  if (getOptions(block).includes('side-by-side')) {
    /* For side-by-side teaser, add the image-wrapper to a higher-level div to support CSS */
    block.querySelector(':scope > div:first-child').classList.add('image-wrapper');
  } else if (!getOptions(block)) {
    /* For the default option, add the image-wrapper to the picture element to support CSS */
    block.querySelector('picture').classList.add('image-wrapper');
  }

  addEventListeners(block);
}
```

## 開發預覽

在加入 CSS 和 JavaScript 之後，AEM CLI 的本機開發環境會即時重新載入相關變更，因而可以快速又輕鬆地將程式碼對區塊的影響以視覺化圖形呈現。將游標停留在 CTA 上，檢查並確認 Teaser 的影像會放大和縮小顯示。

![使用 CSS 和 JS 的 Teaser 的本機開發預覽](./assets/block-options//local-development-preview.png)

## 對程式碼進行 lint 檢查

務必針對您的程式碼變更[經常進行 lint 檢查](../3-local-development-environment.md#linting)，保持程式碼整潔且一致。定期進行 lint 檢查有助於及早發現問題，進而減少整體開發時間。請記住，在解決所有 linting 問題以前，您不能將開發工作合併到 `main` 分支內！

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

## 在通用編輯器中預覽

若要在 AEM 通用編輯器中檢視變更，請將其新增、提交並推送至通用編輯器所使用的 Git 存放庫分支。這樣做可以確保區塊實施不會破壞製作體驗。

```bash
# ~/Code/aem-wknd-eds-ue

$ git add .
$ git commit -m "CSS and JavaScript implementation for Teaser block option Side-by-side"
# JSON files are compiled automatically and added to the commit via a Husky pre-commit hook
$ git push origin block-options
```

現在，使用 `?ref=block-options` 查詢參數時可以在通用編輯器中看到變更。

![通用編輯器中的 Teaser](./assets/block-options/universal-editor-preview.png){align="center"}


## 恭喜！

您現在已經探索過 Edge Delivery Services 和通用編輯器中的區塊選項，讓您擁有多項工具，能夠更靈活地自訂和簡化內容編輯的工作。開始在您的專案中套用這些選項，藉此提高效率並保持一致性。

若要了解更多最佳實務和進階技術，請查看[通用編輯器文件](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/create-block#block-options)。
