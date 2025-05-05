---
title: 延伸元件 | AEM SPA Editor and Angular快速入門
description: 瞭解如何擴充要與AEM SPA Editor搭配使用的現有核心元件。 瞭解如何將屬性和內容新增至現有元件，是擴充AEM SPA Editor實作功能的強大技術。 瞭解如何使用委派模式來延伸Sling模型和Sling Resource Merger的功能。
feature: SPA Editor, Core Components
version: Experience Manager as a Cloud Service
jira: KT-5871
thumbnail: 5871-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 0265d3df-3de8-4a25-9611-ddf73d725f6e
duration: 435
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1713'
ht-degree: 0%

---

# 擴充核心元件 {#extend-component}

瞭解如何擴充要與AEM SPA Editor搭配使用的現有核心元件。 瞭解如何擴充現有元件是一項強大的技術，可自訂和擴充AEM SPA Editor實作的功能。

## 目標

1. 使用其他屬性和內容擴充現有的核心元件。
2. 瞭解使用`sling:resourceSuperType`的元件繼承基本知識。
3. 瞭解如何為Sling模型使用[委派模式](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models)以重複使用現有邏輯和功能。

## 您將建置的內容

在本章中，已建立新的`Card`元件。 `Card`元件擴充[影像核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=zh-Hant)，新增其他內容欄位，例如「標題」和「行動號召」按鈕，以針對SPA內的其他內容執行Teaser的角色。

![卡片元件的最終製作](assets/extend-component/final-authoring-card.png)

>[!NOTE]
>
> 在真實世界的實作中，視專案需求而定，可能更適合使用[Teaser元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/teaser.html?lang=zh-Hant)，而不是擴充[影像核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=zh-Hant)以產生`Card`元件。 建議您儘可能直接使用[核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-hant)。

## 先決條件

檢閱設定[本機開發環境](overview.md#local-dev-environment)所需的工具和指示。

### 取得程式碼

1. 透過Git下載本教學課程的起點：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/extend-component-start
   ```

2. 使用Maven將程式碼庫部署到本機AEM執行個體：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   如果使用[AEM 6.x](overview.md#compatibility)，請新增`classic`設定檔：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 安裝傳統[WKND參考站台](https://github.com/adobe/aem-guides-wknd/releases/tag/aem-guides-wknd-2.1.0)的完成套件。 由[WKND參考網站](https://github.com/adobe/aem-guides-wknd/releases/latest)提供的影像會在WKND SPA上重複使用。 可以使用[AEM的封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)來安裝封裝。

   ![封裝管理員安裝wknd.all](./assets/map-components/package-manager-wknd-all.png)

您一律可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution)上檢視完成的程式碼，或切換至分支`Angular/extend-component-solution`在本機簽出程式碼。

## 檢查初始卡片實施

章節起始程式碼已提供初始卡片元件。 檢查卡片實作的起點。

1. 在您選擇的IDE中，開啟`ui.apps`模組。
2. 瀏覽至`ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/card`並檢視`.content.xml`檔案。

   ![卡片元件AEM定義開始](assets/extend-component/aem-card-cmp-start-definition.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Card"
       sling:resourceSuperType="wknd-spa-angular/components/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   屬性`sling:resourceSuperType`指向`wknd-spa-angular/components/image`，表示`Card`元件繼承了WKND SPA影像元件的功能。

3. 檢查檔案`ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image/.content.xml`：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Image"
       sling:resourceSuperType="core/wcm/components/image/v2/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   請注意，`sling:resourceSuperType`指向`core/wcm/components/image/v2/image`。 這表示WKND SPA影像元件繼承了核心元件影像的功能。

   也稱為[Proxy模式](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html?lang=zh-Hant#proxy-component-pattern) Sling資源繼承是一種強大的設計模式，可讓子元件繼承功能並在需要時擴充/覆寫行為。 Sling繼承支援多個層級的繼承，所以新`Card`元件最終會繼承核心元件影像的功能。

   許多開發團隊都會努力做到自我（請勿重複這點）。 Sling繼承可讓AEM實現此目標。

4. 在`card`資料夾下，開啟檔案`_cq_dialog/.content.xml`。

   此檔案是`Card`元件的元件對話方塊定義。 如果使用Sling繼承，則可以使用[Sling資源合併器](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/sling-resource-merger.html?lang=zh-Hant)的功能來覆寫或擴充對話方塊的部分。 在此範例中，對話方塊中已新增索引標籤，以從作者擷取其他資料並填入卡片元件。

   `sling:orderBefore`之類的屬性可讓開發人員選擇插入新標籤或表單欄位的位置。 在此情況下，`Text`索引標籤會插入`asset`索引標籤之前。 若要充分利用Sling Resource Merger，請務必瞭解[影像元件對話方塊](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_dialog/.content.xml)的原始對話方塊節點結構。

5. 在`card`資料夾下，開啟檔案`_cq_editConfig.xml`。 此檔案會指定AEM編寫UI中的拖放行為。 擴充影像元件時，資源型別必須符合元件本身。 檢閱`<parameters>`節點：

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-angular/components/card"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   大部分元件不需要`cq:editConfig`，影像和影像元件的子系下階是例外。

6. 在IDE切換至`ui.frontend`模組，瀏覽至`ui.frontend/src/app/components/card`：

   ![Angular元件開始](assets/extend-component/angular-card-component-start.png)

7. 檢查檔案`card.component.ts`。

   元件已經使用標準`MapTo`函式截斷，以對應至AEM `Card`元件。

   ```js
   MapTo('wknd-spa-angular/components/card')(CardComponent, CardEditConfig);
   ```

   檢閱類別中`src`、`alt`和`title`的三個`@Input`引數。 這些是AEM元件中對應至Angular元件的預期JSON值。

8. 開啟檔案`card.component.html`：

   ```html
   <div class="card"  *ngIf="hasContent">
       <app-image class="card__image" [src]="src" [alt]="alt" [title]="title"></app-image>
   </div>
   ```

   在此範例中，我們選擇從`card.component.ts`傳遞`@Input`引數，以重複使用現有的Angular影像元件`app-image`。 在稍後的教學課程中，會新增並顯示其他屬性。

## 更新範本原則

使用此初始`Card`實作，檢閱AEM SPA編輯器中的功能。 若要檢視初始`Card`元件，需要更新範本原則。

1. 將入門程式碼部署到AEM的本機執行個體（如果尚未部署）：

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. 導覽至[http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html)的SPA頁面範本。
3. 更新配置容器的原則以將新的`Card`元件新增為允許的元件：

   ![更新配置容器原則](assets/extend-component/card-component-allowed.png)

   儲存原則的變更，並將`Card`元件視為允許的元件：

   ![卡片元件為允許的元件](assets/extend-component/card-component-allowed-layout-container.png)

## 作者初始卡片元件

接下來，使用AEM SPA編輯器編寫`Card`元件。

1. 導覽至[http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)。
2. 在`Edit`模式中，將`Card`元件新增至`Layout Container`：

   ![插入新元件](assets/extend-component/insert-custom-component.png)

3. 將影像從資產尋找器拖放至`Card`元件上：

   ![新增影像](assets/extend-component/card-add-image.png)

4. 開啟`Card`元件對話方塊並注意已新增&#x200B;**文字**&#x200B;索引標籤。
5. 在&#x200B;**文字**&#x200B;索引標籤上輸入下列值：

   ![文字元件索引標籤](assets/extend-component/card-component-text.png)

   **卡片路徑** — 在SPA首頁下方選擇頁面。

   **CTA文字** - 「瞭解詳情」

   **卡片標題** — 留空

   **從連結的頁面取得標題** — 勾選核取方塊以表示True。

6. 更新&#x200B;**資產中繼資料**&#x200B;索引標籤以新增&#x200B;**替代文字**&#x200B;和&#x200B;**標題**&#x200B;的值。

   更新對話方塊後，目前沒有其他變更顯示。 若要將新欄位公開給Angular元件，我們需要更新`Card`元件的Sling模型。

7. 開啟新索引標籤並導覽至[CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd-spa-angular/us/en/home/jcr%3Acontent/root/responsivegrid/card)。 檢查`/content/wknd-spa-angular/us/en/home/jcr:content/root/responsivegrid`下的內容節點以尋找`Card`元件內容。

   ![CRXDE-Lite元件屬性](assets/extend-component/crxde-lite-properties.png)

   觀察對話方塊是否持續儲存屬性`cardPath`、`ctaText`、`titleFromPage`。

## 更新卡片Sling模型

若要最終將元件對話方塊中的值公開給Angular元件，我們需要更新為`Card`元件填入JSON的Sling模型。 我們也有機會實作兩種商業邏輯：

* 如果`titleFromPage`為&#x200B;**true**，則傳回`cardPath`所指定頁面的標題，否則傳回`cardTitle`文字欄位的值。
* 傳回`cardPath`所指定頁面的上次修改日期。

返回您選擇的IDE並開啟`core`模組。

1. 在`core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/Card.java`開啟檔案`Card.java`。

   請注意，`Card`介面目前延伸`com.adobe.cq.wcm.core.components.models.Image`，因此繼承`Image`介面的方法。 `Image`介面已擴充`ComponentExporter`介面，可讓Sling模型匯出為JSON並由SPA編輯器對應。 因此，我們不需要像在[自訂元件章節](custom-component.md)中那樣明確擴充`ComponentExporter`介面。

2. 將下列方法新增至介面：

   ```java
   @ProviderType
   public interface Card extends Image {
   
       /***
       * The URL to populate the CTA button as part of the card.
       * The link should be based on the cardPath property that points to a page.
       * @return String URL
       */
       public String getCtaLinkURL();
   
       /***
       * The text to display on the CTA button of the card.
       * @return String CTA text
       */
       public String getCtaText();
   
   
   
       /***
       * The date to be displayed as part of the card.
       * This is based on the last modified date of the page specified by the cardPath
       * @return
       */
       public Calendar getCardLastModified();
   
   
       /**
       * Return the title of the page specified by cardPath if `titleFromPage` is set to true.
       * Otherwise return the value of `cardTitle`
       * @return
       */
       public String getCardTitle();
   }
   ```

   這些方法會透過JSON模型API公開，並傳遞至Angular元件。

3. 開啟`CardImpl.java`。 這是`Card.java`介面的實作。 為了加速教學課程，已部分解決此實作。  請注意是否使用`@Model`和`@Exporter`註解，以確保Sling模型能夠透過Sling模型匯出工具序列化為JSON。

   `CardImpl.java`也對Sling模型使用[委派模式](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models)以避免從影像核心元件重寫邏輯。

4. 請注意下列各行：

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   上述註解會根據`Card`元件的`sling:resourceSuperType`繼承，將名為`image`的影像物件具現化。

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   然後就可以直接使用`image`物件來實作`Image`介面定義的方法，而不需要自行撰寫邏輯。 此技巧用於`getSrc()`、`getAlt()`和`getTitle()`。

5. 接下來，實作`initModel()`方法，以根據`cardPath`的值起始私有變數`cardPage`

   ```java
   @PostConstruct
   public void initModel() {
       if(StringUtils.isNotBlank(cardPath) && pageManager != null) {
           cardPage = pageManager.getPage(this.cardPath);
       }
   }
   ```

   初始化Sling模型時會呼叫`@PostConstruct initModel()`，因此您可以藉此機會初始化模型中其他方法可能使用的物件。 `pageManager`是透過`@ScriptVariable`註解提供給Sling模型使用的數個[Java™支援的全域物件](https://experienceleague.adobe.com/docs/experience-manager-htl/content/global-objects.html?lang=zh-Hant)之一。 [getPage](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html)方法接受路徑並傳回AEM [Page](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html)物件，如果路徑未指向有效頁面，則傳回null。

   這會初始化`cardPage`變數，其他新方法會使用此變數來傳回基礎連結頁面的相關資料。

6. 檢閱已對映至作者對話方塊所儲存JCR屬性的全域變數。 `@ValueMapValue`註解是用來自動執行對應。

   ```java
   @ValueMapValue
   private String cardPath;
   
   @ValueMapValue
   private String ctaText;
   
   @ValueMapValue
   private boolean titleFromPage;
   
   @ValueMapValue
   private String cardTitle;
   ```

   這些變數可用來實作`Card.java`介面的其他方法。

7. 實作`Card.java`介面中定義的其他方法：

   ```java
   @Override
   public String getCtaLinkURL() {
       if(cardPage != null) {
           return cardPage.getPath() + ".html";
       }
       return null;
   }
   
   @Override
   public String getCtaText() {
       return ctaText;
   }
   
   @Override
   public Calendar getCardLastModified() {
      if(cardPage != null) {
          return cardPage.getLastModified();
      }
      return null;
   }
   
   @Override
   public String getCardTitle() {
       if(titleFromPage) {
           return cardPage != null ? cardPage.getTitle() : null;
       }
       return cardTitle;
   }
   ```

   >[!NOTE]
   >
   > 您可以在[&#128279;](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/extend-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CardImpl.java)檢視完成的CardImpl.java。

8. 開啟終端機視窗，並使用`core`目錄中的Maven `autoInstallBundle`設定檔僅部署`core`模組的更新。

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

   如果使用[AEM 6.x](overview.md#compatibility)，請新增`classic`設定檔。

9. 檢視JSON模型回應： [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)並搜尋`wknd-spa-angular/components/card`：

   ```json
   "card": {
       "ctaText": "Read More",
       "cardTitle": "Page 1",
       "title": "Woman chillaxing with river views in Australian bushland",
       "src": "/content/wknd-spa-angular/us/en/home/_jcr_content/root/responsivegrid/card.coreimg.jpeg/1595190732886/adobestock-216674449.jpeg",
       "alt": "Female sitting on a large rock relaxing in afternoon dappled light the Australian bushland with views over the river",
       "cardLastModified": 1591360492414,
       "ctaLinkURL": "/content/wknd-spa-angular/us/en/home/page-1.html",
       ":type": "wknd-spa-angular/components/card"
   }
   ```

   請注意，更新`CardImpl` Sling模型中的方法後，JSON模型已更新為其他索引鍵/值組。

## 更新Angular元件

現在JSON模型已填入`ctaLinkURL`、`ctaText`、`cardTitle`和`cardLastModified`的新屬性，我們可以更新Angular元件以顯示這些屬性。

1. 返回IDE並開啟`ui.frontend`模組。 您可以選擇從新的終端機視窗啟動webpack開發伺服器，即時檢視變更：

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

2. 在`ui.frontend/src/app/components/card/card.component.ts`開啟`card.component.ts`。 新增其他`@Input`註解以擷取新模型：

   ```diff
   export class CardComponent implements OnInit {
   
        @Input() src: string;
        @Input() alt: string;
        @Input() title: string;
   +    @Input() cardTitle: string;
   +    @Input() cardLastModified: number;
   +    @Input() ctaLinkURL: string;
   +    @Input() ctaText: string;
   ```

3. 新增方法以檢查呼叫動作是否準備就緒，並根據`cardLastModified`輸入傳回日期/時間字串：

   ```js
   export class CardComponent implements OnInit {
       ...
       get hasCTA(): boolean {
           return this.ctaLinkURL && this.ctaLinkURL.trim().length > 0 && this.ctaText && this.ctaText.trim().length > 0;
       }
   
       get lastModifiedDate(): string {
           const lastModifiedDate = this.cardLastModified ? new Date(this.cardLastModified) : null;
   
           if (lastModifiedDate) {
           return lastModifiedDate.toLocaleDateString();
           }
           return null;
       }
       ...
   }
   ```

4. 開啟`card.component.html`並新增下列標籤以顯示標題、行動號召和上次修改日期：

   ```html
   <div class="card"  *ngIf="hasContent">
       <app-image class="card__image" [src]="src" [alt]="alt" [title]="title"></app-image>
       <div class="card__content">
           <h2 class="card__title">
               {{cardTitle}}
               <span class="card__lastmod" *ngIf="lastModifiedDate">{{lastModifiedDate}}</span>
           </h2>
           <div class="card__action-container" *ngIf="hasCTA">
               <a [routerLink]="ctaLinkURL" class="card__action-link" [title]="ctaText">
                   {{ctaText}}
               </a>
           </div>
       </div>
   </div>
   ```

   已在`card.component.scss`新增Sass規則，以設定標題、行動號召和上次修改日期的樣式。

   >[!NOTE]
   >
   > 您可以在此檢視已完成的[Angular卡元件程式碼](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution/ui.frontend/src/app/components/card)。

5. 使用Maven從專案的根將完整變更部署到AEM：

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

6. 導覽至[http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)檢視更新的元件：

   ![已在AEM中更新卡片元件](assets/extend-component/updated-card-in-aem.png)

7. 您應該能夠重新編寫現有內容以建立類似下列的頁面：

   ![卡片元件的最終製作](assets/extend-component/final-authoring-card.png)

## 恭喜！ {#congratulations}

恭喜，您已瞭解如何擴充AEM元件，以及Sling模型和對話方塊如何搭配JSON模型使用。

您一律可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution)上檢視完成的程式碼，或切換至分支`Angular/extend-component-solution`在本機簽出程式碼。
