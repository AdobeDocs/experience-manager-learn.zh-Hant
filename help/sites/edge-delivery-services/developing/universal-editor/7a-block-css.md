---
title: 使用CSS開發區塊
description: 使用CSS為Edge Delivery Services開發區塊，可使用通用編輯器進行編輯。
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
source-git-commit: e8ce91b0be577ec6cf8f3ab07ba9ff09c7e7a6ab
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---


# 使用CSS開發區塊

Edge Delivery Services中的區塊會使用CSS來設定樣式。 區塊的CSS檔案儲存在區塊的目錄中，且與區塊同名。 例如，名為`teaser`的區塊的CSS檔案位於`blocks/teaser/teaser.css`。

理想情況下，區塊在樣式設定上只需使用CSS，而不需仰賴JavaScript修改DOM或新增CSS類別。 對JavaScript的需求取決於區塊的[內容模式](./5-new-block.md#block-model)及其複雜性。 如有需要，可以新增[封鎖JavaScript](./7b-block-js-css.md)。

使用僅限CSS的方法時，區塊的（通常）純語意HTML元素會定位並設定樣式。

## 封鎖HTML

若要瞭解如何設定區塊的樣式，請先檢閱Edge Delivery Services公開的DOM，因為它適用於設定樣式。 檢查AEM CLI的本機開發環境所提供的區塊即可找到DOM。 請避免使用通用編輯器的DOM，因為它略有不同。

>[!BEGINTABS]

>[!TAB 樣式的DOM]

以下是樣式設定目標的Teaser區塊的DOM。

請注意[自動增強的`<p class="button-container">...`，作為Edge Delivery Services JavaScript推斷的元素。](./4-website-branding.md#inferred-elements)

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

>[!TAB 如何尋找DOM]

若要尋找要設定樣式的DOM，請在您的本機開發環境中開啟包含未設定樣式區塊的頁面、選取區塊並檢查DOM。

![Inspect區塊DOM](./assets/7a-block-css/inspect-block-dom.png)

>[!ENDTABS]

## 封鎖CSS

在區塊的資料夾中建立新的CSS檔案，使用區塊的名稱做為檔案名稱。 例如，對於&#x200B;**Teaser**&#x200B;區塊，檔案位於`/blocks/teaser/teaser.css`。

當Edge Delivery Services的JavaScript在頁面上偵測到代表Teaser區塊的DOM元素時，會自動載入此CSS檔案。

[!BADGE /blocks/teaser/teaser.css]{type=Neutral tooltip="以下程式碼範例的檔案名稱。"}

```css
/* /blocks/teaser/teaser.css */

/* Scope each selector in the block with `.block.teaser` to avoid accidental conflicts outside the block */
.block.teaser {
    animation: teaser-fade-in .6s;
    position: relative;
    width: 1600px;
    max-width: 100vw;
    left: 50%; 
    transform: translateX(-50%);
    height: 500px;
}

/* The image is rendered to the first div in the block */
.block.teaser picture {
    position: absolute;
    z-index: -1;
    inset: 0;
    box-sizing: border-box;
}

.block.teaser picture img {
    object-fit: cover;
    object-position: center;
    width: 100%;
    height: 100%;
}

/** 
The teaser's text is rendered to the second (also the last) div in the block.

These styles are scoped to the second (also the last) div in the block (.block.teaser > div:last-child).

This div order can be used to target different styles to the same semantic elements in the block. 
For example, if the block has two images, we could target the first image with `.block.teaser > div:first-child img`, 
and the second image with `.block.teaser > div:nth-child(2) img`.
**/
.block.teaser > div:last-child {
    position: absolute;
    bottom: 0;
    left: 50%;
    transform: translateX(-50%);
    background: var(--background-color);
    padding: 1.5rem 1.5rem 1rem;
    width: 80vw;
    max-width: 1200px;
}

/** 
The following elements reside within `.block.teaser > div:last-child` and could be scoped as such, for example:

 .block.teaser > div:last-child p { .. }

However since these element can only appear in the second/last div per our block's model, it's unnecessary to add this additional scope.
**/

/* Regardless of the authored heading level, we only want one style the heading */
.block.teaser h1,
.block.teaser h2,
.block.teaser h3,
.block.teaser h4,
.block.teaser h5,
.block.teaser h6 {
    font-size: var(--heading-font-size-xl);
    margin: 0;
}

.block.teaser h1::after,
.block.teaser h2::after,
.block.teaser h3::after,
.block.teaser h4::after,
.block.teaser h5::after,
.block.teaser h6::after {
    border-bottom: 0;
}

.block.teaser p {
    font-size: var(--body-font-size-s);
    margin-bottom: 1rem;
}

/* Add underlines to links in the text */
.block.teaser a:hover {
    text-decoration: underline;
}

/* Add specific spacing to buttons. These button CSS classes are automatically added by Edge Delivery Services. */
.block.teaser .button-container {
    margin: 0;
    padding: 0;
}

.block.teaser .button {
    background-color: var(--primary-color);
    border-radius: 0;
    color: var(--dark-color);
    font-size: var(--body-font-size-xs);
    font-weight: bold;
    padding: 1em 2.5em;
    margin: 0;
    text-transform: uppercase;
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

## 開發預覽

由於CSS是寫入程式碼專案中，AEM CLI的熱過載會有變更，因此可讓您快速輕鬆地瞭解CSS如何影響區塊。

![僅CSS預覽](./assets/7a-block-css/local-development-preview.png)

## 將程式碼插入

請確定您[經常lint](./3-local-development-environment.md#linting)您的程式碼變更，以確保其整潔一致。 Linting有助於及早發現問題，並縮短整體開發時間。 請記住，您必須先解決所有Linting問題，才能將您的開發工作合併到`main`！

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint:css
```

## 在通用編輯器中預覽

若要在AEM Universal Editor中檢視變更，請新增、提交變更，並將其推送至Universal Editor使用的Git存放庫分支。 此步驟有助於確保區塊實施不會中斷編寫體驗。

```bash
# ~/Code/aem-wknd-eds-ue

$ git add .
$ git commit -m "Add CSS-only implementation for teaser block"
$ git push origin teaser
```

現在，當您新增`?ref=teaser`查詢引數時，可以在通用編輯器中預覽變更。

通用編輯器中的![Teaser](./assets/7a-block-css/universal-editor-preview.png)

