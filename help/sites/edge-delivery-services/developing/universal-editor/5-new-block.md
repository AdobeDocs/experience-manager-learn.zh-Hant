---
title: 建立區塊
description: 在 Edge Delivery Services 網站建置一個可使用通用編輯器進行編輯的區塊。
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: 9698c17a-0ac8-426d-bccb-729b048cabd1
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1767'
ht-degree: 100%

---

# 建立新區塊

本章節介紹使用通用編輯器為 Edge Delivery Services 網站建立新的可編輯 Teaser 區塊的流程。

![新的 Teaser 區塊](./assets//5-new-block/teaser-block.png)

該區塊名為 `teaser`，會展示以下元素：

- **影像**：吸引目光的影像。
- **文字內容**：
   - **標題**：引人注目的標題。
   - **正文**：提供背景或細節的描述性內容，包括選用的條款與條件。
   - **行動號召 (CTA) 按鈕**：專門提示使用者互動，並引導其進一步參與的連結。

`teaser` 區塊的內容可以在通用編輯器中編輯，確保整個網站的易用性和再使用性。

請注意 `teaser` 區塊和樣板專案的 `hero` 區塊類似；因此 `teaser` 區塊只是一個用來說明開發概念的簡單範例。

## 建立新的 Git 分支

為保持乾淨、有條理的工作流程，請為每個特定的開發工作建立一個新的分支。這有助於避免將不完整或未經測試的程式碼部署至生產中而導致的問題。

1. **從主分支開始**：從最新的生產程式碼開始進行，確保堅實的基礎。
2. **取得遠端變更**：從 GitHub 取得最新更新，確保在開始開發之前取得最新的程式碼。
   - 範例：將來自 `wknd-styles` 分支的變更合併到 `main` 內之後，取得最新的更新。
3. **建立新的分支**：

```bash
# ~/Code/aem-wknd-eds-ue

$ git fetch origin  
$ git checkout -b teaser origin/main  
```

一旦 `teaser` 分支建立完成之後，您便可以開始開發 Teaser 區塊。

## 區塊資料夾

在專案的 `blocks` 目錄中，建立名為 `teaser` 的新資料夾。此資料夾包含區塊的 JSON、CSS 和 JavaScript 檔案，將區塊的檔案整理存放在同一個位置：

```
# ~/Code/aem-wknd-eds-ue

/blocks/teaser
```

以區塊資料夾名稱作為區塊 ID，並用於在整個開發過程中參照該區塊。

## 區塊 JSON

區塊 JSON 定義了區塊的三個關鍵面向：

- **定義**：將區塊註冊為通用編輯器中的可編輯元件，並將其連結至區塊模型，也可以選擇連結至篩選器。
- **模型**：指定區塊的製作欄位，以及這些欄位如何轉譯為語義 Edge Delivery Services HTML。
- **篩選器**：設定篩選器規則，限制可以透過通用編輯器將區塊新增至哪些容器中。大多數的區塊並非容器，反而是其 ID 被加入到其他容器區塊的篩選器中。

在 `/blocks/teaser/_teaser.json` 建立新檔案，初始結構如下，順序完全相同。若金鑰順序錯誤，則可能無法正確建置。

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="下方程式碼範例的檔案名稱。"}

```json
{
    "definitions": [],
    "models": [],
    "filters": []
}
```

### 區塊模型

區塊模型是區塊設定的關鍵部分，因為其負責定義：

1. 製作體驗，方法是定義可供編輯的欄位。

   ![通用編輯器欄位](./assets/5-new-block/fields-in-universal-editor.png)

2. 欄位的值如何轉譯到 Edge Delivery Services HTML 中。

模型被指派一個與[區塊定義](#block-definition)相對應的 `id`，並包含一個指定可編輯欄位的 `fields` 陣列。

`fields` 陣列中的每個欄位都有一個 JSON 物件，其中包含以下必要屬性：

| JSON 屬性 | 說明 |
|---------------|-----------------------------------------------------------------------------------------------------------------------|
| `component` | [欄位類型](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/field-types#component-types)，例如 `text`、`reference` 或 `aem-content`。 |
| `name` | 欄位的名稱，對應至 AEM 中所儲存之值的 JCR 屬性。 |
| `label` | 通用編輯器中向作者顯示的標籤。 |

關於屬性的完整清單 (包括選用屬性)，請檢閱[通用編輯器欄位文件](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/field-types#fields)。

#### 區塊設計

![Teaser 區塊](./assets/5-new-block/block-design.png)

Teaser 區塊包含以下可編輯元素：

1. **影像**：能代表 Teaser 的視覺內容。
2. **文字內容**：包含標題、正文和行動號召按鈕，位於白色矩形內。
   - **標題**&#x200B;和&#x200B;**正文**&#x200B;可以透過同一個 RTF 文字編輯器製作。
   - 可以使用&#x200B;**標籤**&#x200B;的 `text` 欄位以及&#x200B;**連結**&#x200B;的 `aem-content` 欄位來製作 **CTA**。

Teaser 區塊的設計分為這兩個邏輯元件 (影像和文字內容)，確保為使用者提供結構化和直覺易用的製作體驗。

### 區塊欄位

定義區塊所需的欄位：影像、影像替代文字、文字、CTA 標籤和 CTA 連結。

>[!BEGINTABS]

>[!TAB 正確方法]

**此索引標籤說明建立 Teaser 區塊模型的正確方法。**

Teaser 由兩個邏輯區域組成：影像和文字。為簡化將 Edge Delivery Services HTML 顯示為所需網頁體驗的程式碼，區塊模型也應反映這種結構。

- 使用[欄位收合](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse)將&#x200B;**影像**&#x200B;和&#x200B;**影像替代文字**&#x200B;組合在一起。
- 使用[元素分組](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#element-grouping)以及 [CTA 的欄位收合](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse)，將文字內容欄位組合在一起。

如果您不熟悉[欄位收合](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse)、[元素分組](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#element-grouping)或[類型推斷](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#type-inference)，請在繼續之前檢閱連結的文件，因為這些項目對於建立結構良好的區塊模型來說非常重要。

在下方的範例中：

- [類型推斷](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#type-inference)用於從 `image` 欄位自動建立 `<img>` HTML 元素。欄位收合與 `image` 和 `imageAlt` 欄位一起使用，以建立 `<img>` HTML 元素。`src` 屬性設定為 `image` 欄位的值，而 `alt` 屬性設定為 `imageAlt` 欄位的值。
- `textContent` 是用於對欄位進行分類的群組名稱。其應為語義性質，但可以是這個區塊獨有的任何內容。這會通知通用編輯器在最終版 HTML 輸出中的相同 `<div>` 元素內，轉譯帶有此前置詞的所有欄位。
- 在 `textContent` 群組中，行動號召亦具備欄位收合功能。CTA 是透過[類型推斷](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#type-inference)建立，並採用 `<a>` 的形式。`cta` 欄位用於設定 `<a>` 元素的 `href` 屬性，而 `ctaText` 欄位針對 `<a ...>` 標記中的連結提供文字內容。

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="下方程式碼範例的檔案名稱。"}

```json
{
    "definitions": [],
    "models": [
        {
            "id": "teaser", 
            "fields": [
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

此模型定義通用編輯器中區塊的製作輸入。

此區塊產生的 Edge Delivery Services HTML 會將影像放在第一個 div 中，並將元素群組 `textContent` 欄位放在第二個 div 中。

```html
<div>
    <div>
        <!-- This div contains the field-collapsed image fields  -->
        <picture>
            ...
            <source .../>            
            <img src="..." alt="The authored alt text"/>
        </picture>
    </div>
    <div>
        <!-- This div, via element grouping contains the textContent fields -->
        <h2>The authored title</h2>
        <p>The authored body text</p>
        <a href="/authored/cta/link">The authored CTA label</a>
    </div>
</div>        
```

如[下一章節](./7a-block-css.md)所示範，這種 HTML 結構簡化該區塊作為緊密相連的單元之樣式設計。

若要了解不使用欄位收合和元素分組的影響，請參閱上面的&#x200B;**錯誤的方式**&#x200B;索引標籤。

>[!TAB 錯誤的方式]

**此索引標籤的內容說明建立 Teaser 區塊模型時比較不理想的方法，而且只是與正確方法的對比。**

將每個欄位定義為區塊模型中的獨立欄位，而沒有使用[欄位收合](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse)和[元素分組](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#element-grouping)，可能看起來很吸引人。然而，這種疏忽會使將區塊作為緊密相連的單元之樣式設計變得更複雜。

例如，在&#x200B;**沒有**&#x200B;欄位收合或元素分組的情況下，可將 Teaser 模型定義如下：

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="下方程式碼範例的檔案名稱。"}

```json
{
    "definitions": [],
    "models": [
        {
            "id": "teaser", 
            "fields": [
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
                    "name": "alt",
                    "label": "Image alt text",
                    "required": true
                },
                {
                    "component": "richtext",
                    "name": "text",
                    "label": "Text",
                    "valueType": "string",
                    "required": true
                },
                {
                    "component": "aem-content",
                    "name": "link",
                    "label": "CTA",
                    "valueType": "string"
                },
                {
                    "component": "text",
                    "name": "label",
                    "label": "CTA label",
                    "valueType": "string"
                }
            ]
        }
    ],
    "filters": []
}
```

區塊的 Edge Delivery Services HTML 會在個別的 `div` 中轉譯每個欄位的值，使達到所需設計的內容理解、樣式應用和 HTML 結構調整變得複雜。

```html
<div>
    <div>
        <!-- This div contains the field-collapsed image  -->
        <picture>
            ...
            <source .../>            
            <img src="/authored/image/reference"/>
        </picture>
    </div>
    <div>
        <p>The authored alt text</p>
    </div>
    <div>
        <h2>The authored title</h2>
        <p>The authored body text</p>
    </div>
    <div>
        <a href="/authored/cta/link">/authored/cta/link</a>
    </div>
    <div>
        The authored CTA label
    </div>
</div>        
```

每個欄位獨自存在於自己的 `div` 內，因此很難將影像和文字內容視為緊密相連的單元進行樣式設計。透過努力和創意來達成所需的設計是可能的，但使用[元素分組](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#element-grouping)將文字內容欄位分組，以及使用[欄位收合](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse)來新增自訂值作為元素屬性，則更為簡單、輕鬆，而且也保持正確的語義。

請參閱上面的&#x200B;**正確的方式**&#x200B;索引標籤，了解如何建立更好的 Teaser 區塊模型。

>[!ENDTABS]


### 區塊定義

區塊定義會在通用編輯器中註冊該區塊。以下是區塊定義中使用之 JSON 屬性的劃分：

| JSON 屬性 | 說明 |
|---------------|-------------|
| `definition.title` | 區塊的標題，如同其在通用編輯器的&#x200B;**新增**&#x200B;區塊中所顯示者。 |
| `definition.id` | 區塊的唯一 ID，用於控制其在 `filters` 的使用情形。 |
| `definition.plugins.xwalk.page.resourceType` | 定義在通用編輯器中轉譯元件所使用的 Sling 資源類型。請務必使用 `core/franklin/components/block/v#/block` 資源類型。 |
| `definition.plugins.xwalk.page.template.name` | 區塊的名稱。其應該是小寫並含有連字符，以符合區塊的資料夾名稱。該值也可以用於為通用編輯器中的區塊實例加上標籤。 |
| `definition.plugins.xwalk.page.template.model` | 將此定義連結到其 `model` 定義，該定義會控制在通用編輯器顯示的區塊製作欄位。此處的值必須符合 `model.id` 值。 |
| `definition.plugins.xwalk.page.template.classes` | 選用屬性，其值會新增到區塊 HTML 元素的 `class` 屬性。這樣便可以產生同一個區塊的變體。在區塊[模型](#block-model)中[新增類別欄位](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/create-block#block-options)，可以讓 `classes` 值變成可編輯。 |


以下是區塊定義的 JSON 範例：

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="下方程式碼範例的檔案名稱。"}

```json
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

在此範例中：

- 區塊名稱為「Teaser」並使用 `teaser` 模型，該模型會決定可在通用編輯器中編輯哪些欄位。
- 該區塊包含 `textContent_text` 欄位的預設內容，此為標題和正文的 RTF 文字區域，以及 `textContent_cta` 和 `textContent_ctaText`，用於 CTA (行動號召) 連結和標籤。包含初始內容的範本欄位名稱，與[內容模型之欄位陣列](#block-model)所定義的欄位名稱相符；

此結構能確保在通用編輯器中設定的區塊具有適當的欄位、內容模型和資源類型以進行轉譯。

### 區塊篩選器

區塊的 `filters` 陣列會針對[容器區塊](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#container)定義哪些其他區塊可以加入到容器中。篩選器定義出一個可以加入容器中的區塊 ID 清單 (`model.id`)。

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="下方程式碼範例的檔案名稱。"}

```json
{
  "definitions": [... populated from previous section ...],
  "models": [... populated from previous section ...],
  "filters": []
}
```

Teaser 元件不是[容器區塊](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#container)，也就是說您不能在其中加入其他區塊。因此，其 `filters` 陣列是空白的。相反地，將 Teaser 的 ID 加入到區段區塊的篩選器清單中，便能將 Teaser 加入到區段中。

![區塊篩選器](./assets/5-new-block/filters.png)

Adobe 提供的區塊 (例如區段區塊) 會將篩選器儲存在專案的 `models` 資料夾中。若要進行調整，請找到 Adobe 提供之區塊的 JSON 檔案 (例如 `/models/_section.json`)，並在篩選器清單中新增 Teaser 的 ID (`teaser`)。此設定會向通用編輯器發出訊號，表示可以將 Teaser 元件新增至區段容器區塊中。

[!BADGE /models/_section.json]{type=Neutral tooltip="下方程式碼範例的檔案名稱。"}

```json
{
  "definitions": [],
  "models": [],
  "filters": [
    {
      "id": "section",
      "components": [
        "text",
        "image",
        "button",
        "title",
        "hero",
        "cards",
        "columns",
        "fragment",
        "teaser"
      ]
    }
  ]
}
```

`teaser` 的 Teaser 區塊定義 ID 已新增至 `components` 陣列中。

## 針對您的 JSON 檔案進行 lint 檢查

務必針對您的程式碼變更[經常進行 lint 檢查](./3-local-development-environment.md#linting)，保持程式碼整潔且一致。頻繁 linting 有助於及早發現問題，進而減少整體開發時間。`npm run lint:js` 命令也會對 JSON 檔案進行 lint 檢查，並抓出任何語法錯誤。

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint:js
```

## 建置專案 JSON

設定區塊 JSON 檔案 (例如 `blocks/teaser/_teaser.json`、`models/_section.json`) 之後，那些檔案會自動編譯術專案的 `component-models.json`、`component-definitions.json`，和 `component-filters.json` 檔案。此編譯會由包含在 [AEM 樣板專案 XWalk 專案範本](https://github.com/adobe-rnd/aem-boilerplate-xwalk)中的 [Husky](https://typicode.github.io/husky/) 預先提交 Hook 自動處理。

也可以使用專案的[建置 JSON](./3-local-development-environment.md#build-json-fragments) NPM 指令碼，以手動或程式設計的方式觸發組建。

## 部署區塊 JSON

若要在通用編輯器中使用該區塊，則必須提交專案並推送至 GitHub 存放庫的分支，在本案例中為 `teaser` 分支。

每位使用者皆可透過通用編輯器的 URL，調整通用編輯器所使用之確切分支的名稱。

```bash
# ~/Code/aem-wknd-eds-ue

$ git add .
$ git commit -m "Add teaser block JSON files so it is available in Universal Editor"
# JSON files are compiled automatically and added to the commit via a husky precommit hook
$ git push origin teaser
```

使用查詢參數 `?ref=teaser` 開啟通用編輯器時，新的 `teaser` 區塊會出現在區塊面板中。請注意，該區塊沒有樣式設定；該區塊會將區塊的欄位轉譯為語義 HTML，僅能透過[全域 CSS](./4-website-branding.md#global-css) 設定樣式。
