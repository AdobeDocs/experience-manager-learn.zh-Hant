---
title: 將富文本與無AEM頭
description: 學習使用帶有Adobe Experience Manager內容片段的多行富文本編輯器創作內容並嵌入引用內容，以及AEMGraphQLAPI將富文本作為JSON傳遞給無頭應用程式的方式。
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

# 帶無頭的富AEM文本

多行文本欄位是內容片段的資料類型，使作者能夠建立富格文本內容。 對其他內容的引用，例如影像或其他內容片段可以動態地在文本流內以行形式插入。 「單行」文本欄位是應用於簡單文本元素的「內容片段」的另一種資料類型。

AEMGraphQLAPI提供強大功能，可將富格文本作為HTML、純文字檔案或純JSON返回。 JSON表示法功能強大，因為它賦予客戶端應用程式對如何呈現內容的完全控制權。

## 多行編輯器

>[!VIDEO](https://video.tv.adobe.com/v/342104?quality=12&learn=on)

在內容片段編輯器中，多行文本欄位的菜單欄為作者提供了標準的富格文本格式功能，如 **粗**。 *斜體*&#x200B;和下划線。 以全屏模式開啟多行欄位啟用 [其他格式工具，如段落類型、查找和替換、拼寫檢查等](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-variations.html)。

>[!NOTE]
>
> 無法自定義多行編輯器中的富文本插件。

## 多行文本資料類型 {#multi-line-data-type}

使用 **多行文本** 定義內容片段模型以啟用富格文本創作時的資料類型。

![多行富文本資料類型](assets/rich-text/multi-line-rich-text.png)

可以配置多行欄位的多個屬性。

的 **呈現為** 屬性可設定為：

* 文本區域 — 呈現單個多行欄位
* 多個欄位 — 呈現多個多行欄位


的 **預設類型** 可設定為：

* RTF
* Markdown
* 純文字

的 **預設類型** 選項會直接影響編輯體驗，並確定是否存在富格文本工具。

您也可以 [啟用行內引用](#insert-fragment-references) 通過檢查 **允許片段引用** 和配置 **允許的內容片段模型**。

檢查 **可翻譯** 框中，選擇「預設值」。 只能本地化「富格文本」和「純文字檔案」。 請參閱 [使用本地化內容，瞭解詳細資訊](./localized-content.md)。

## 使用GraphQLAPI的富文本響應

建立GraphQL查詢時，開發人員可以從 `html`。 `plaintext`。 `markdown`, `json` 從多行欄位中。

開發人員可以 [JSON預覽](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-json-preview.html) 在「內容片段」編輯器中，顯示可使用GraphQLAPI返回的當前內容片段的所有值。

## GraphQL永續查詢

選擇 `json` 多行欄位的響應格式在處理富文本內容時提供了最大的靈活性。 富文本內容作為JSON節點類型的陣列傳遞，這些JSON節點類型可以基於客戶端平台進行唯一處理。

下面是名為的多行欄位的JSON響應類型 `main` 包含段落：&quot;*這是一段&#x200B;**重要**內容。*&quot;其中&quot;重要&quot;標籤為 **粗**。

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

的 `$path` 在 `_path` 篩選器需要內容片段的完整路徑(例如 `/content/dam/wknd/en/magazine/sample-article`)。

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

### 其他示例

下面是名為的多行欄位的幾種響應類型示例 `main` 包含段落：「這是一段話，其中包括 **重要** 內容。」 其中，&quot;importent&quot;標籤為 **粗**。

+++HTML示例

**GraphQL持久查詢：**

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

+++Markdown示例

**GraphQL持久查詢：**

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

+++明文示例

**GraphQL持久查詢：**

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

的 `plaintext` render選項會刪除任何格式。

+++


## 呈現JSON富文本響應 {#render-multiline-json-richtext}

多行欄位的富文本JSON響應被構造為分層樹。 每個對象或節點表示富格文本的不同HTML塊。

下面是多行文本欄位的示例JSON響應。 觀察每個對象或節點都包括 `nodeType` 它表示來自富格文本的HTML塊，如 `paragraph`。 `link`, `text`。 每個節點（可選）包含 `content` 是包含當前節點的任何子級的子陣列。

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

渲染多行的最簡單方法 `json` 響應是處理響應中的每個對象或節點，然後處理當前節點的任何子項。 遞歸函式可用於遍歷JSON樹。

下面是示例代碼，說明了遞歸遍歷方法。 示例基於JavaScript，並使用React [JSX](https://reactjs.org/docs/introducing-jsx.html)但是，寫程式概念可以應用於任何語言。

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

`renderNodeList` 是一個遞歸函式，它 `childNodes`。 然後，將陣列中的每個節點傳遞給函式 `renderNode`，然後調用 `renderNodeList` 的下界。

```javascript
// renderNode - renders an individual node
function renderNode(node) {

    // if the current node has children, recursively process them
    const children = node.content ? renderNodeList(node.content) : null;

    // use a map to render the current node based on its nodeType
    return nodeMap[node.nodeType]?.(node, children);
}
```

的 `renderNode` 函式需要名為 `node`。 節點可以具有子級，子級使用 `renderNodeList` 函式。 最後， `nodeMap` 用於根據節點的內容來呈現其內容 `nodeType`。

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

的 `nodeMap` 是用作映射的JavaScript對象文本。 每個&quot;鍵&quot;代表不同的 `nodeType`。 參數 `node` 和 `children` 可以傳遞到呈現節點的結果函式。 本示例中使用的返回類型是JSX，但該方法可以適用於生成表示HTML內容的字串文本。

### 完整代碼示例

在 [WKNDGraphQL反應示例](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)。

* [renderRichText.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/utils/renderRichText.js)  — 可重用的實用程式，可公開函式 `mapJsonRichText`。 此實用程式可由要將RT JSON響應呈現為React JSX的元件使用。
* [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js)  — 發出包含富格文本的GraphQL請求的示例元件。 元件使用 `mapJsonRichText` 用於呈現富格文本和任何引用的實用程式。


## 將行內引用添加到富格文本 {#insert-fragment-references}

Mutline欄位允許作者在富文本流中插入來自AEM Assets的影像或其他數字資產。

![插入影像](assets/rich-text/insert-image.png)

上面的螢幕快照使用 **插入資產** 按鈕

也可以使用 **插入內容片段** 按鈕

![插入內容片段引用](assets/rich-text/insert-contentfragment.png)

上面的螢幕快照描述了插入到多行欄位的另一個內容片段「LA Skate Parks終極指南」。 可插入欄位的內容片段的類型由 **允許的內容片段模型** 配置 [多行資料類型](#multi-line-data-type) 的子菜單。

## 查詢帶GraphQL的行內引用

GraphQLAPI使開發人員能夠建立查詢，該查詢包括關於插入多行欄位中的任何引用的附加屬性。 JSON響應包含單獨 `_references` 列出這些額外屬性的對象。 JSON響應使開發人員能夠完全控制如何呈現引用或連結，而不必處理有主見的HTML。

例如，您可能希望：

* 包括自定義路由邏輯，用於在實施單頁應用程式時管理到其他內容片段的連結，如使用React Router或Next.js
* 使用AEM發佈環境的絕對路徑渲染聯機影像 `src` 值。
* 確定如何使用其他自定義屬性呈現對另一個內容片段的嵌入引用。

使用 `json` 返回類型並包括 `_references` 構造GraphQL查詢時的對象：

**GraphQL持久查詢：**

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

在上述查詢中， `main` 欄位返回為JSON。 的 `_references` 對象包括用於處理任何類型的引用的片段 `ImageRef` 或類型 `ArticleModel`。

**JSON響應：**

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

JSON響應包括引用插入到富格文本中的位置，其中 `"nodeType": "reference"`。 的 `_references` 然後，對象包括每個引用。

## 在富格文本中呈現行內引用

要渲染線內參照，請使用中介紹的遞歸方法 [呈現多行JSON響應](#render-multiline-json-richtext) 可以展開。

位置 `nodeMap` 是呈現JSON節點的映射。

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

高級別方法是， `nodeType` 等於 `reference` 在多行JSON響應中。 然後，可以調用包含 `_references` 在GraphQL響應中返回的對象。

然後，可以將串聯參考路徑與中相應條目進行比較 `_references` 對象和另一個自定義映射 `renderReference` 可以叫。

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

的 `__typename` 的 `_references` 對象可用於將不同的參考型別映射到不同的呈現函式。

### 完整代碼示例

有關編寫自定義引用呈現器的完整示例，請參見 [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js) 作為 [WKNDGraphQL反應示例](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)。

## 端到端示例

>[!VIDEO](https://video.tv.adobe.com/v/342105?quality=12&learn=on)

>[!NOTE]
>
> 以上視頻使用 `_publishUrl` 顯示影像引用。 相反，更喜歡 `_dynamicUrl` 如 [Web優化的影像如何](./images.md);


上面的視頻顯示了一個端到端示例：

1. 更新內容片段模型的多行文本欄位以允許片段引用
2. 使用內容片段編輯器在多行文本欄位中包括影像和對另一個片段的引用。
3. 建立包含多行文本響應的GraphQL查詢(JSON和任何 `_references` 。
4. 編寫呈SPA現RTF響應的行內引用的React。
