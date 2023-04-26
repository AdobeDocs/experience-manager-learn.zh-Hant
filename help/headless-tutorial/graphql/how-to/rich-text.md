---
title: 搭配AEM Headless使用RTF
description: 了解如何使用多行RTF編輯器搭配Adobe Experience Manager內容片段來編寫內容及內嵌參考內容，以及AEM GraphQL API如何以JSON形式傳送RTF，供無頭應用程式使用。
version: Cloud Service
doc-type: article
kt: 9985
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
exl-id: 790a33a9-b4f4-4568-8dfe-7e473a5b68b6
source-git-commit: 117b67bd185ce5af9c83bd0c343010fab6cd0982
workflow-type: tm+mt
source-wordcount: '1465'
ht-degree: 0%

---

# RTF文字(含AEM Headless)

多行文字欄位是內容片段的資料類型，可讓作者建立RTF內容。 其他內容的參考，例如影像或其他內容片段，可在文字流程內以動態方式串聯插入。 單行文字欄位是內容片段的其他資料類型，應用於簡單文字元素。

AEM GraphQL API提供強大的功能，可傳回RTFHTML、純文字或純JSON格式。 JSON表示法功能強大，因為它可讓用戶端應用程式完全控制如何呈現內容。

## 多行編輯器

>[!VIDEO](https://video.tv.adobe.com/v/342104?quality=12&learn=on)

在內容片段編輯器中，多行文字欄位的功能表列可為作者提供標準RTF格式功能，例如 **粗體**, *斜體*，和底線。 以全螢幕模式開啟多行欄位，可啟用 [其他格式工具，如段落類型、尋找和取代、拼字檢查等](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-variations.html).

>[!NOTE]
>
> 無法自訂多行編輯器中的RTF外掛程式。

## 多行文本資料類型 {#multi-line-data-type}

使用 **多行文本** 定義內容片段模型時的資料類型，以啟用RTF編寫。

![多行RTF資料類型](assets/rich-text/multi-line-rich-text.png)

可配置多行欄位的多個屬性。

此 **呈現為** 屬性可設為：

* 文字區域 — 轉譯單一多行欄位
* 多欄位 — 轉譯多個多行欄位


此 **預設類型** 可設為：

* RTF
* Markdown
* 純文字

此 **預設類型** 選項會直接影響編輯體驗，並決定是否存在rtf工具。

您也可以 [啟用行內引用](#insert-fragment-references) 至其他內容片段，方法是檢查 **允許片段參考** 和配置 **允許的內容片段模型**.

檢查 **可翻譯** 框中，以便本地化內容。 只有RTF和純文字才能本地化。 請參閱 [使用本地化內容以取得詳細資訊](./localized-content.md).

## 使用GraphQL API的RTF回應

建立GraphQL查詢時，開發人員可從 `html`, `plaintext`, `markdown`，和 `json` 從多行欄位。

開發人員可使用 [JSON預覽](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-json-preview.html) 在「內容片段」編輯器中，顯示目前「內容片段」的所有值，這些值可使用GraphQL API傳回。

## GraphQL持續查詢

選取 `json` 處理RTF內容時，多行欄位的回應格式提供最大的彈性。 RTF內容會以JSON節點類型的陣列傳送，可根據用戶端平台加以唯一處理。

以下是多行欄位的JSON回應類型，名稱為 `main` 包含段落：&quot;*這是段落，包括&#x200B;**重要**內容。*&quot;其中&quot;important&quot;標籤為 **粗體**.

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
  {
    item {
      _path
      main {
        json
      }
    }
  }
}
```

此 `$path` 變數 `_path` 篩選器需要內容片段的完整路徑(例如 `/content/dam/wknd/en/magazine/sample-article`)。

**GraphQL回應：**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
          "json": [
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "This is a paragraph that includes "
                },
                {
                  "nodeType": "text",
                  "value": "important",
                  "format": {
                    "variants": [
                      "bold"
                    ]
                  }
                },
                {
                  "nodeType": "text",
                  "value": " content. "
                }
              ]
            }
          ]
        }
      }
    }
  }
}
```

### 其他範例

以下是多行欄位的幾個回應類型範例，名為 `main` 包含段落：「這段話包括 **重要** 內容。」 其中「重要」標示為 **粗體**.

+++HTML範例

**GraphQL持續查詢：**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
  {
    item {
      _path
      main {
        html
      }
    }
  }
}
```

**GraphQL回應：**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "html": "<p>This is a paragraph that includes <b>important</b> content.&nbsp;</p>\n"
        }
      }
    }
  }
}
```

+++

+++Markdown範例

**GraphQL持續查詢：**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
  {
    item {
      _path
      main {
        markdown
      }
    }
  }
}
```

**GraphQL回應：**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "markdown": "This is a paragraph that includes **important** content. \n\n ",
        }
      }
    }
  }
}
```

+++

+++純文字範例

**GraphQL持續查詢：**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
  {
    item {
      _path
      main {
        plaintext
      }
    }
  }
}
```

**GraphQL回應：**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "plaintext": "This is a paragraph that includes important content. ",
        }
      }
    }
  }
}
```

此 `plaintext` 呈現選項會移除任何格式。

+++


## 轉譯RTF JSON回應 {#render-multiline-json-richtext}

多行欄位的RTF JSON回應會結構化為階層樹狀結構。 每個物件或節點代表RTF的不同HTML區塊。

以下是多行文字欄位的範例JSON回應。 請注意，每個物件或節點都包含 `nodeType` 它代表來自RTF的HTML區塊，如 `paragraph`, `link`，和 `text`. 每個節點可選地包含 `content` 是包含當前節點的任何子項的子陣列。

```json
"json": [// root "content" or child nodes
            {
                "nodeType": "paragraph", // node for a paragraph
                "content": [ // children of current node
                {
                    "nodeType": "text", // node for a text
                    "value": "This is the first paragraph. "
                },
                {
                    "nodeType": "link",
                    "data": {
                        "href": "http://www.adobe.com"
                    },
                    "value": "An external link"
                }
                ],
            },
            {
                "nodeType": "paragraph",
                "content": [
                {
                    "nodeType": "text",
                    "value": "This is the second paragraph."
                },
                ],
            },
]
```

呈現多行的最簡單方法 `json` 回應是在回應中處理每個物件或節點，然後處理目前節點的任何子項。 遞歸函式可用於遍歷JSON樹。

以下是說明遞歸遍歷方法的示例代碼。 這些範例以JavaScript為基礎，並使用React的 [JSX](https://reactjs.org/docs/introducing-jsx.html)，但程式設計概念可套用至任何語言。

```javascript
// renderNodeList - renders a list of nodes
function renderNodeList(childNodes) {
    
    if(!childNodes) {
        // null check
        return null;
    }

    return childNodes.map(node, index) => {
        return renderNode(node);
    }
}
```

`renderNodeList` 是遞歸函式，取用 `childNodes`. 然後，陣列中的每個節點都會傳遞至函式 `renderNode`，接著呼叫 `renderNodeList` 如果節點有子項。

```javascript
// renderNode - renders an individual node
function renderNode(node) {

    // if the current node has children, recursively process them
    const children = node.content ? renderNodeList(node.content) : null;

    // use a map to render the current node based on its nodeType
    return nodeMap[node.nodeType]?.(node, children);
}
```

此 `renderNode` 函式需要單個名為 `node`. 節點可以具有子項，這些子項使用 `renderNodeList` 函式。 最後， `nodeMap` 用於根據節點的內容來呈現節點的內容 `nodeType`.

```javascript
// nodeMap - object literal that maps a JSX response based on a given key (nodeType)
const nodeMap = {
    'paragraph': (node, children) => <p>{children}</p>,
    'link': node => <a href={node.data.href} target={node.data.target}>{node.value}</a>,
    'text': node => node.value,
    'unordered-list': (node, children) => <ul>{children}</ul>,
    'ordered-list': (node, children) => <ol>{children}</ol>,
    'list-item': (node, children) => <li>{children}</li>,
    ...
}
```

此 `nodeMap` 是作為映射使用的JavaScript物件常值。 每個「金鑰」代表不同的 `nodeType`. 的參數 `node` 和 `children` 可傳遞至呈現節點的結果函式。 此範例中使用的傳回類型為JSX，但方法可適應來建立代表HTML內容的字串常值。

### 完整程式碼範例

可在 [WKND GraphQL React範例](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

* [renderRichText.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/utils/renderRichText.js)  — 可重複使用的公用程式，可公開函式 `mapJsonRichText`. 若要將RTF JSON回應轉譯為React JSX，元件可使用此公用程式。
* [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js)  — 提出包含RTF文字之GraphQL請求的元件範例。 元件使用 `mapJsonRichText` 用於呈現RTF和任何引用的實用程式。


## 將內嵌參考新增至RTF {#insert-fragment-references}

「多重連結」欄位可讓作者在RTF流程中插入來自AEM Assets的影像或其他數位資產。

![插入影像](assets/rich-text/insert-image.png)

上方的螢幕擷圖描繪插入多行欄位中的影像，使用 **插入資產** 按鈕。

其他內容片段的參考也可以連結或插入多行欄位，使用 **插入內容片段** 按鈕。

![插入內容片段參考](assets/rich-text/insert-contentfragment.png)

上面的螢幕截圖描述了插入多行欄位的另一個內容片段 — LA Skate Parks的Ultimate指南。 可插入欄位的內容片段類型由 **允許的內容片段模型** 設定 [多行資料類型](#multi-line-data-type) （在內容片段模型中）。

## 使用GraphQL查詢行內參考

GraphQL API可讓開發人員建立查詢，其中包含與插入多行欄位中之任何參照相關的其他屬性。 JSON回應包含個別 `_references` 列出這些額外屬性的物件。 JSON回應可讓開發人員完全掌控如何轉譯參考或連結，而不必處理確信的HTML。

例如，您可能想要：

* 包括自訂路由邏輯，用於在實作單頁應用程式時（例如使用React Router或Next.js）管理連結至其他內容片段
* 使用AEM發佈環境的絕對路徑，以 `src` 值。
* 決定如何使用其他自訂屬性呈現內嵌參考至其他內容片段。

使用 `json` 傳回類型並包含 `_references` 物件，當您建構GraphQL查詢時：

**GraphQL持續查詢：**

```graphql
query ($path: String!) {
  articleByPath(_path: $path, _assetTransform: { format: JPG, preferWebp: true })
  {
    item {
      _path
      main {
        json
      }
    }
    _references {
      ...on ImageRef {
        _dynamicUrl
        __typename
      }
      ...on ArticleModel {
        _path
        author
        __typename
      }  
    }
  }
}
```

在上述查詢中， `main` 欄位會以JSON傳回。 此 `_references` 物件包含用於處理任何類型參考的片段 `ImageRef` 或類型 `ArticleModel`.

**JSON回應：**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
          "json": [
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "This is a paragraph that includes "
                },
                {
                  "nodeType": "text",
                  "value": "important",
                  "format": {
                    "variants": [
                      "bold"
                    ]
                  }
                },
                {
                  "nodeType": "text",
                  "value": " content. "
                }
              ]
            },
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "reference",
                  "data": {
                    "path": "/content/dam/wknd/en/activities/climbing/sport-climbing.jpg",
                    "mimetype": "image/jpeg"
                  }
                }
              ]
            },
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "Reference another Content Fragment: "
                },
                {
                  "nodeType": "reference",
                  "data": {
                    "href": "/content/dam/wknd/en/magazine/la-skateparks/ultimate-guide-to-la-skateparks",
                    "type": "fragment"
                  },
                  "value": "Ultimate Guide to LA Skateparks"
                }
              ]
            }
          ]
        }
      },
      "_references": [
        {
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--dd42d814-88ec-4c4d-b5ef-e3dc4bc0cb42/sport-climbing.jpg?preferwebp=true",
          "__typename": "ImageRef"
        },
        {
          "_path": "/content/dam/wknd/en/magazine/la-skateparks/ultimate-guide-to-la-skateparks",
          "author": "Stacey Roswells",
          "__typename": "ArticleModel"
        }
      ]
    }
  }
}
```

JSON回應會包含參考插入RTF中的位置，其中包含 `"nodeType": "reference"`. 此 `_references` 然後，物件會包含每個參考。

## 以RTF格式呈現內嵌參考

要渲染線內參照，遞歸方法在 [轉譯多行JSON回應](#render-multiline-json-richtext) 可以展開。

其中 `nodeMap` 是轉譯JSON節點的地圖。

```javascript
const nodeMap = {
        'reference': (node, children) => {

            // variable for reference in _references object
            let reference;
            
            // asset reference
            if (node.data.path) {
                // find reference based on path
                reference = references.find( ref => ref._path === node.data.path);
            }
            // Fragment Reference
            if (node.data.href) {
                // find in-line reference within _references array based on href and _path properties
                reference = references.find( ref => ref._path === node.data.href);
            }

            // if reference found, merge properties of reference and current node, then return render method of it using __typename property
            return reference ? renderReference[reference.__typename]({...reference, ...node}) : null;
        }
    }
```

高級方法是在 `nodeType` 等於 `reference` 在多行JSON回應中。 接著，可以呼叫自訂轉譯函式，其中包含 `_references` 在GraphQL回應中傳回的物件。

然後，可將線上參考路徑與 `_references` 物件和其他自訂地圖 `renderReference` 可呼叫。

```javascript
const renderReference = {
    // node contains merged properties of the in-line reference and _references object
    'ImageRef': (node) => {
        // when __typename === ImageRef
        return <img src={node._dynamicUrl} alt={'in-line reference'} /> 
    },
    'ArticleModel': (node) => {
        // when __typename === ArticleModel
        return <Link to={`/article:${node._path}`}>{`${node.value}`}</Link>;
    }
    ...
}
```

此 `__typename` 的 `_references` 物件可用來將不同的參考類型對應至不同的轉譯函式。

### 完整程式碼範例

在中可以找到撰寫自訂參照轉譯器的完整範例，如 [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js) 作為 [WKND GraphQL React範例](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

## 端對端範例

>[!VIDEO](https://video.tv.adobe.com/v/342105?quality=12&learn=on)

>[!NOTE]
>
> 上述影片使用 `_publishUrl` 來呈現影像參考。 相反， `_dynamicUrl` 如 [網頁最佳化影像作法](./images.md);


前面的影片示範端對端範例：

1. 更新內容片段模型的多行文字欄位以允許片段參考
2. 使用內容片段編輯器在多行文字欄位中包含影像和參考另一個片段。
3. 建立GraphQL查詢，其中包含多行文字回應作為JSON和任何 `_references` 已使用。
4. 撰寫可轉譯RTF回應之內嵌參考的React SPA。
