---
title: 擴充元件 |開始使用AEM SPA編輯器和Angular
description: 了解如何擴充現有的核心元件以與AEM SPA編輯器搭配使用。 了解如何將屬性和內容新增至現有元件，是擴充AEM SPA Editor實作功能的強大技術。 了解如何使用委派模式來擴充Sling模型和Sling Resource Merger的功能。
sub-product: Sites
feature: SPA編輯器，核心元件
doc-type: tutorial
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 5871
thumbnail: 5871-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1989'
ht-degree: 2%

---


# 擴展核心元件{#extend-component}

了解如何擴充現有的核心元件以與AEM SPA編輯器搭配使用。 了解如何擴充現有元件是自訂和擴充AEM SPA Editor實作功能的強大技術。

## 目標

1. 使用其他屬性和內容擴充現有的核心元件。
2. 使用`sling:resourceSuperType`了解元件繼承的基本內容。
3. 了解如何針對Sling模型運用[委派模式](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models)來重複使用現有的邏輯和功能。

## 您將建置的

本章將建立新的`Card`元件。 `Card`元件將擴展[影像核心元件](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/components/image.translate.html)添加諸如「標題」和「呼叫操作」按鈕等其他內容欄位，以對SPA內的其他內容執行預告的角色。

![卡元件的最終製作](assets/extend-component/final-authoring-card.png)

>[!NOTE]
>
> 在實際實作中，更合適的做法是直接使用[預告元件](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/components/teaser.html)，然後擴展[影像核心元件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/image.html)，以根據項目需求製作`Card`元件。 建議您盡可能直接使用[核心元件](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/introduction.html)。

## 必備條件

查看設定[本地開發環境](overview.md#local-dev-environment)所需的工具和說明。

### 取得程式碼

1. 透過Git下載本教學課程的起始點：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/extend-component-start
   ```

2. 使用Maven將程式碼基底部署至本機AEM例項：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   如果使用[AEM 6.x](overview.md#compatibility)新增`classic`設定檔：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 安裝傳統[WKND參考站點](https://github.com/adobe/aem-guides-wknd/releases/latest)的已完成包。 [WKND參考站點](https://github.com/adobe/aem-guides-wknd/releases/latest)提供的影像將在WKND SPA上重新使用。 可使用[AEM套件管理器](http://localhost:4502/crx/packmgr/index.jsp)安裝套件。

   ![包管理器安裝wknd.all](./assets/map-components/package-manager-wknd-all.png)

您一律可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution)上檢視完成的程式碼，或切換至分支`Angular/extend-component-solution`在本機檢出程式碼。

## Inspect初始卡實作

章節啟動程式代碼已提供初始卡元件。 Inspect是卡片實作的起點。

1. 在所選IDE中，開啟`ui.apps`模組。
2. 導覽至`ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/card`並檢視`.content.xml`檔案。

   ![卡元件AEM定義開始](assets/extend-component/aem-card-cmp-start-definition.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Card"
       sling:resourceSuperType="wknd-spa-angular/components/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   屬性`sling:resourceSuperType`指向`wknd-spa-angular/components/image`，指示`Card`元件將繼承WKND SPA Image元件的所有功能。

3. Inspect檔案`ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Image"
       sling:resourceSuperType="core/wcm/components/image/v2/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   請注意，`sling:resourceSuperType`指向`core/wcm/components/image/v2/image`。 這表示WKND SPA Image元件繼承核心元件映像的所有功能。

   也稱為[Proxy模式](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#proxy-component-pattern) Sling資源繼承是功能強大的設計模式，可讓子元件視需要繼承功能和擴充/覆寫行為。 Sling繼承支援多個繼承層級，因此新`Card`元件最終會繼承核心元件影像的功能。

   許多開發團隊努力成為DRY（別重複）。 Sling繼承讓AEM成為可能。

4. 在`card`資料夾下，開啟檔案`_cq_dialog/.content.xml`。

   此檔案是`Card`元件的「元件對話框」定義。 如果使用Sling繼承，則可能使用[Sling Resource Merger](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/sling-resource-merger.html)的功能來覆寫或延伸對話方塊的部分。 在此範例中，對話方塊已新增一個索引標籤，以從作者擷取其他資料以填入「卡片元件」。

   `sling:orderBefore`等屬性可讓開發人員選擇插入新索引標籤或表單欄位的位置。 在這種情況下， `Text`標籤將插入在`asset`標籤之前。 要充分利用Sling Resource Merger，請務必了解[影像元件對話方塊](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_dialog/.content.xml)的原始對話節點結構。

5. 在`card`資料夾下，開啟檔案`_cq_editConfig.xml`。 此檔案會指定AEM製作UI中的拖放行為。 擴充影像元件時，資源類型必須符合元件本身。 查看`<parameters>`節點：

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-angular/components/card"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   大多數元件不需要`cq:editConfig`，影像元件的影像和子代是例外。

6. 在IDE交換機中，導航至`ui.frontend/src/app/components/card`模組：`ui.frontend`

   ![Angular元件開始](assets/extend-component/angular-card-component-start.png)

7. Inspect檔案`card.component.ts`。

   已使用標準`MapTo`函式將元件映射到AEM `Card`元件。

   ```js
   MapTo('wknd-spa-angular/components/card')(CardComponent, CardEditConfig);
   ```

   查看類中`src`、`alt`和`title`的三個`@Input`參數。 這些是AEM元件中應對應至Angular元件的JSON值。

8. 開啟檔案`card.component.html`:

   ```html
   <div class="card"  *ngIf="hasContent">
       <app-image class="card__image" [src]="src" [alt]="alt" [title]="title"></app-image>
   </div>
   ```

   在此範例中，我們選取僅從`card.component.ts`傳遞`@Input`參數，以重新使用現有的Angular影像元件`app-image`。 在稍後的教學課程中，將新增並顯示其他屬性。

## 更新模板策略

透過此初始`Card`實作，檢閱AEM SPA Editor中的功能。 要查看初始`Card`元件，需要更新模板策略。

1. 將入門程式碼部署至AEM的本機執行個體（如果尚未部署）:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. 導覽至SPA頁面範本，網址為[http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html)。
3. 更新「配置容器」的策略，以將新`Card`元件添加為允許的元件：

   ![更新佈局容器策略](assets/extend-component/card-component-allowed.png)

   保存對策略的更改，並將`Card`元件作為允許的元件進行觀察：

   ![卡元件作為允許的元件](assets/extend-component/card-component-allowed-layout-container.png)

## 製作初始卡片元件

接下來，使用AEM SPA編輯器編寫`Card`元件。

1. 導覽至[http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)。
2. 在`Edit`模式中，將`Card`元件添加到`Layout Container`:

   ![插入新元件](assets/extend-component/insert-custom-component.png)

3. 從資產尋找器將影像拖放至`Card`元件：

   ![新增影像](assets/extend-component/card-add-image.png)

4. 開啟`Card`元件對話方塊，並注意&#x200B;**Text**&#x200B;索引標籤的新增。
5. 在&#x200B;**Text**&#x200B;標籤上輸入以下值：

   ![「文本元件」頁簽](assets/extend-component/card-component-text.png)

   **卡片路徑**  — 在SPA首頁下方選擇頁面。

   **CTA文字**  - 「了解詳情」

   **卡片標題**  — 留空

   **從連結的頁面取得標題**  — 勾選核取方塊以指出true。

6. 更新&#x200B;**資產中繼資料**&#x200B;標籤以新增&#x200B;**替代文字**&#x200B;和&#x200B;**註解**&#x200B;的值。

   目前，在更新對話方塊後，不會顯示其他變更。 若要將新欄位公開給Angular元件，我們需要更新`Card`元件的Sling模型。

7. 開啟新索引標籤，並導覽至[CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd-spa-angular/us/en/home/jcr%3Acontent/root/responsivegrid/card)。 Inspect `/content/wknd-spa-angular/us/en/home/jcr:content/root/responsivegrid`下方的內容節點，以尋找`Card`元件內容。

   ![CRXDE-Lite元件屬性](assets/extend-component/crxde-lite-properties.png)

   請注意，對話方塊會保存屬性`cardPath`、`ctaText`、`titleFromPage`。

## 更新Card Sling模型

若要最終將元件對話方塊中的值公開給Angular元件，我們需要更新填入`Card`元件JSON的Sling模型。 我們還有機會實施兩個業務邏輯：

* 若`titleFromPage`變為&#x200B;**true**，則傳回`cardPath`所指定頁面的標題，否則傳回`cardTitle`文字欄位的值。
* 傳回`cardPath`所指定頁面的上次修改日期。

返回您選擇的IDE並開啟`core`模組。

1. 在`core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/Card.java`開啟檔案`Card.java`。

   請注意，`Card`介面當前擴展`com.adobe.cq.wcm.core.components.models.Image`，因此繼承`Image`介面的所有方法。 `Image`介面已延伸`ComponentExporter`介面，讓Sling模型可匯出為JSON，並由SPA編輯器對應。 因此，我們不需要像在[自訂元件章節](custom-component.md)中所做的那樣顯式擴展`ComponentExporter`介面。

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

3. 開啟 `CardImpl.java`. 這是`Card.java`介面的實現。 為加速本教學課程，本實作已做部分研究。  請注意使用`@Model`和`@Exporter`註解，以確保Sling模型能透過Sling模型匯出工具序列化為JSON。

   `CardImpl.java` 也會使用 [Sling模型的委](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) 派模式，以避免從影像核心元件重新寫入所有邏輯。

4. 請觀察下列行：

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   上述批注將根據`Card`元件的`sling:resourceSuperType`繼承來實例化名為`image`的影像對象。

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   然後，只需使用`image`對象來實現由`Image`介面定義的方法，而無需自行編寫邏輯。 此技術用於`getSrc()`、`getAlt()`和`getTitle()`。

5. 接下來，實作`initModel()`方法，以根據`cardPath`的值啟動專用變數`cardPage`

   ```java
   @PostConstruct
   public void initModel() {
       if(StringUtils.isNotBlank(cardPath) && pageManager != null) {
           cardPage = pageManager.getPage(this.cardPath);
       }
   }
   ```

   初始化Sling模型時，一律會呼叫`@PostConstruct initModel()`，因此，這是初始化模型中其他方法可能使用的物件的良機。 `pageManager`是透過`@ScriptVariable`註解可供Sling模型使用的數個[Java支援的全域物件](https://docs.adobe.com/content/help/en/experience-manager-htl/using/htl/global-objects.html#java-backed-objects)之一。 [getPage](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/PageManager.html#getPage-java.lang.String-)方法會進入路徑並傳回AEM [Page](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/Page.html)物件，若路徑未指向有效頁面，則傳回null。

   這會初始化`cardPage`變數，其他新方法將使用該變數來傳回基礎連結頁面的相關資料。

6. 檢閱已對應至已儲存製作對話方塊之JCR屬性的全域變數。 `@ValueMapValue`注釋用於自動執行映射。

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

   這些變數將用於實作`Card.java`介面的其他方法。

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
   > 您可以在此處](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/extend-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CardImpl.java)檢視[已完成的CardImpl.java。

8. 開啟終端機視窗，使用`core`目錄中的Maven `autoInstallBundle`設定檔，只部署`core`模組的更新。

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

   如果使用[AEM 6.x](overview.md#compatibility) ，請新增`classic`設定檔。

9. 請在下列網址檢視JSON模型回應：[http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)並搜尋`wknd-spa-angular/components/card`:

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

   請注意，在`CardImpl` Sling模型中更新方法後，JSON模型已更新為其他索引鍵/值組。

## 更新Angular元件

現在，JSON模型已填入`ctaLinkURL`、`ctaText`、`cardTitle`和`cardLastModified`的新屬性，我們可以更新Angular元件以顯示這些屬性。

1. 返回IDE並開啟`ui.frontend`模組。 （可選）從新的終端窗口啟動Webpack開發伺服器，以即時查看更改：

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

2. 在`ui.frontend/src/app/components/card/card.component.ts`開啟`card.component.ts`。 添加附加的`@Input`注釋以捕獲新模型：

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

3. 新增方法，以檢查動作呼叫是否已就緒，並根據`cardLastModified`輸入傳回日期/時間字串：

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

4. 開啟`card.component.html`並新增下列標籤以顯示標題、動作呼叫和上次修改日期：

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

   已在`card.component.scss`新增Sass規則，以設定標題、動作呼叫和上次修改日期的樣式。

   >[!NOTE]
   >
   > 您可以在此處](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution/ui.frontend/src/app/components/card)檢視已完成的[Angular卡元件代碼。

5. 使用Maven從專案的根目錄部署完整變更至AEM:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

6. 導覽至[http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)以查看更新的元件：

   ![更新AEM中的卡片元件](assets/extend-component/updated-card-in-aem.png)

7. 您應該可以重新編寫現有內容，以建立類似下列的頁面：

   ![卡元件的最終製作](assets/extend-component/final-authoring-card.png)

## 恭喜！ {#congratulations}

恭喜您，您已學會如何使用擴充AEM元件，以及Sling模型和對話方塊如何與JSON模型搭配運作。

您一律可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution)上檢視完成的程式碼，或切換至分支`Angular/extend-component-solution`在本機檢出程式碼。
