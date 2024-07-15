---
title: 搭配AEM Headless使用RTF文字
description: 瞭解如何使用具有Adobe Experience Manager內容片段的多行RTF編輯器編寫內容並嵌入參考內容，以及AEM的GraphQL API如何以JSON形式傳送RTF以供無頭應用程式使用。
version: Cloud Service
doc-type: article
jira: KT-9985
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
level: Intermediate
role: Developer
exl-id: 790a33a9-b4f4-4568-8dfe-7e473a5b68b6
duration: 785
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1409'
ht-degree: 0%

---

# RTF文字與AEM Headless

多行文字欄位是「內容片段」的資料型別，可讓作者建立RTF文字內容。 對其他內容（例如影像或其他內容片段）的參考可動態地在文字流程中內嵌插入。 單行文字欄位是另一個應用於簡單文字元素的內容片段資料型別。

AEM的GraphQL API提供強大的功能，可將RTF傳回HTML、純文字或純JSON。 JSON表示法功能強大，因為可讓使用者端應用程式完全掌控內容呈現方式。

## 多行編輯器

>[!VIDEO](https://video.tv.adobe.com/v/342104?quality=12&learn=on)

在內容片段編輯器中，多行文字欄位的功能表列為作者提供標準的RTF格式功能，例如&#x200B;**粗體**、*斜體*&#x200B;和底線。 以全熒幕模式開啟多行欄位，可啟用[其他格式化工具，例如段落文字、尋找和取代、拼字檢查等等](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-variations.html)。

>[!NOTE]
>
> 無法自訂多行編輯器中的RTF外掛程式。

## 多行文字資料型別 {#multi-line-data-type}

定義您的內容片段模型時，請使用&#x200B;**多行文字**&#x200B;資料型別來啟用RTF撰寫。

![多行RTF資料型別](assets/rich-text/multi-line-rich-text.png)

可以設定多行欄位的幾個屬性。

**Render as**&#x200B;屬性可以設為：

* 文字區域 — 呈現單一多行欄位
* 多個欄位 — 呈現多個多行欄位


**預設型別**&#x200B;可以設定為：

* RTF
* Markdown
* 純文字

**預設型別**&#x200B;選項會直接影響編輯體驗，並決定RTF工具是否存在。

您也可以檢查&#x200B;**允許片段參考**&#x200B;並設定&#x200B;**允許的內容片段模型**，以[啟用其他內容片段的內嵌參考](#insert-fragment-references)。

如果要將內容當地語系化，請核取&#x200B;**可翻譯**&#x200B;方塊。 只有RTF和純文字可以當地語系化。 如需詳細資訊，請參閱[使用當地語系化內容](./localized-content.md)。

## 使用GraphQL API提供RTF回應

建立GraphQL查詢時，開發人員可以從多行欄位中從`html`、`plaintext`、`markdown`和`json`選擇不同的回應型別。

開發人員可以在內容片段編輯器中使用[JSON預覽](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-json-preview.html?lang=zh-Hant)，以顯示可使用GraphQL API傳回之目前內容片段的所有值。

## GraphQL持續查詢

選取多行欄位的`json`回應格式，可在使用RTF文字內容時提供最大的彈性。 RTF內容會以JSON節點型別陣列的形式傳送，且可根據使用者端平台唯一處理。

以下是名為`main`且包含段落的多行欄位的JSON回應型別： &quot;*此段落包含&#x200B;**重要**內容。*」中，「重要」標示為&#x200B;**bold**。

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

`_path`篩選器中使用的`$path`變數需要內容片段的完整路徑（例如`/content/dam/wknd/en/magazine/sample-article`）。

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

以下是名為`main`且包含段落之多行欄位的幾個回應型別範例：「這是包含&#x200B;**重要**&#x200B;內容的段落。」 其中「重要」標示為&#x200B;**bold**。

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

`plaintext`演算選項會移除任何格式。

+++


## 轉譯RTF文字JSON回應 {#render-multiline-json-richtext}

多行欄位的RTF文字JSON回應會結構化為階層式樹狀結構。 每個物件或節點代表RTF文字的不同HTML區塊。

以下是多行文字欄位的JSON回應範例。 請注意，每個物件或節點都包含一個`nodeType`，代表RTF中的HTML區塊，例如`paragraph`、`link`和`text`。 每個節點選擇性地包含`content`，這是包含目前節點之任何子系的子陣列。

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

呈現多行`json`回應的最簡單方法是，處理回應中的每個物件或節點，然後處理目前節點的任何子系。 遞回函式可用於周遊JSON樹狀結構。

以下是說明遞回周遊方法的範常式式碼。 範例是以JavaScript為基礎，使用React的[JSX](https://reactjs.org/docs/introducing-jsx.html)，不過程式設計概念可套用至任何語言。

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

`renderNodeList`是接受`childNodes`陣列的遞回函式。 然後陣列中的每個節點會傳遞至函式`renderNode`，如果節點有子系，則函式會呼叫`renderNodeList`。

```javascript
// renderNode - renders an individual node
function renderNode(node) {

    // if the current node has children, recursively process them
    const children = node.content ? renderNodeList(node.content) : null;

    // use a map to render the current node based on its nodeType
    return nodeMap[node.nodeType]?.(node, children);
}
```

`renderNode`函式需要名為`node`的單一物件。 節點可以有子系，這些子系會使用上述`renderNodeList`函式遞回處理。 最後，使用`nodeMap`根據節點的`nodeType`來轉譯節點的內容。

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

`nodeMap`是用作地圖的JavaScript物件常值。 每個「金鑰」代表不同的`nodeType`。 `node`和`children`的引數可以傳入產生節點的函式。 此範例中使用的傳回型別是JSX，但方法可調整為建置表示HTML內容的字串常值。

### 完整程式碼範例

可在[WKND GraphQL React範例](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)中找到可重複使用的RTF轉譯公用程式。

* [renderRichText.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/utils/renderRichText.js) — 公開函式`mapJsonRichText`的可重複使用公用程式。 想要將RTF文字JSON回應轉譯為React JSX的元件可使用此公用程式。
* [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js) — 發出包含RTF文字的GraphQL請求的元件範例。 元件使用`mapJsonRichText`公用程式來轉譯RTF文字和任何參考。


## 新增內嵌參考至RTF {#insert-fragment-references}

「多行」欄位可讓作者在RTF文字流中插入AEM Assets的影像或其他數位資產。

![插入影像](assets/rich-text/insert-image.png)

上述熒幕擷圖描繪使用&#x200B;**插入資產**&#x200B;按鈕在多行欄位中插入的影像。

對其他內容片段的參考也可以使用&#x200B;**插入內容片段**&#x200B;按鈕連結或插入多行欄位中。

![插入內容片段參考](assets/rich-text/insert-contentfragment.png)

上面的熒幕擷圖描繪了插入多行欄位中的另一個內容片段，洛杉磯滑冰公園終極指南。 可插入欄位中的內容片段型別是由內容片段模式中[多行資料型別](#multi-line-data-type)中的&#x200B;**允許的內容片段模式**&#x200B;設定所控制。

## 使用GraphQL查詢內嵌參考

GraphQL API可讓開發人員建立查詢，查詢中包含有關插入多行欄位中的任何參考的其他屬性。 JSON回應包含一個單獨的`_references`物件，其中列出這些額外的屬性。 JSON回應可讓開發人員完全掌控如何呈現參考資料或連結，而不必處理教條式HTML。

例如，您可能想要：

* 包含自訂路由邏輯，用於在實作單頁應用程式（例如使用React路由器或Next.js）時管理其他內容片段的連結
* 使用指向AEM Publish環境的絕對路徑作為`src`值來轉譯內嵌影像。
* 決定如何使用其他自訂屬性呈現對其他內容片段的嵌入參考。

建構GraphQL查詢時使用`json`傳回型別並包含`_references`物件：

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

在上述查詢中，`main`欄位會傳回為JSON。 `_references`物件包含片段，用於處理任何型別為`ImageRef`或型別為`ArticleModel`的參考。

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

JSON回應包含參考以`"nodeType": "reference"`插入RTF中的位置。 然後`_references`物件會包含每個參考。

## 以RTF文字呈現內嵌參考

若要呈現內嵌參考，可展開[呈現多行JSON回應](#render-multiline-json-richtext)中說明的遞回方法。

其中`nodeMap`是轉譯JSON節點的對應。

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

高階方法是在Multli Line JSON回應中`nodeType`等於`reference`時檢查。 接著可以呼叫包含GraphQL回應中傳回的`_references`物件的自訂轉譯函式。

然後可以將內嵌參考路徑與`_references`物件中的對應專案比較，並且可以呼叫另一個自訂對應`renderReference`。

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

`_references`物件的`__typename`可用來將不同的參考型別對應到不同的轉譯器函式。

### 完整程式碼範例

您可以在[AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js)中找到寫入自訂參考轉譯器的完整範例，作為[WKND GraphQL React範例](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)的一部分。

## 端對端範例

>[!VIDEO](https://video.tv.adobe.com/v/342105?quality=12&learn=on)

>[!NOTE]
>
> 上述視訊使用`_publishUrl`來轉譯影像參考。 相反地，偏好使用`_dynamicUrl`，如[網頁最佳化影像how-to](./images.md)中所述；


前文影片顯示端對端範例：

1. 更新內容片段模型的多行文字欄位以允許片段參考
2. 使用內容片段編輯器在多行文字欄位中包含影像和對另一個片段的引用。
3. 建立GraphQL查詢，其中包含以JSON格式的多行文字回應及使用的任何`_references`。
4. 撰寫會呈現RTF回應內嵌參考的React SPA。
