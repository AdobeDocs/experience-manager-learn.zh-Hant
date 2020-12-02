---
title: 第7章——從行動應用程式使用AEM Content Services - Content Services
description: 教學課程的第7章會執行Android Mobile應用程式，以使用AEM Content Services中的創作內容。
translation-type: tm+mt
source-git-commit: 5012433a5f1c7169b1a3996453bfdbd5d78e5b1c
workflow-type: tm+mt
source-wordcount: '1408'
ht-degree: 0%

---


# 第7章——從行動應用程式使用AEM Content Services

教學課程的第7章使用原生Android Mobile應用程式來使用AEM Content Services的內容。

## Android行動應用程式

本教學課程使用&#x200B;**簡單的原生Android Mobile App**&#x200B;來使用和顯示AEM Content Services公開的「事件」內容。

使用[Android](https://developer.android.com/)基本上不重要，而且消費性的行動應用程式可以在任何行動平台（例如iOS）的架構中編寫。

Android是教學課程的使用者，因為它可在Windows、macOs和Linux上執行Android模擬器，而且其受歡迎程度也很高，而且它可以編寫為Java,AEM開發人員十分瞭解Java。

*教學課程的Android Mobile應用程式並非&#x200B;****用來指示如何建立Android Mobile應用程式或傳達Android開發最佳實務，而是用來說明如何從Mobile應用程式使用AEM Content Services。*

### AEM Content Services如何推動行動應用程式體驗

![行動應用程式與內容服務的對應](assets/chapter-7/content-services-mapping.png)

1. **logo**&#x200B;由[!DNL Events API]頁面的&#x200B;**影像元件**&#x200B;定義。
1. **標籤行**&#x200B;定義於[!DNL Events API]頁面的&#x200B;**文字元件**。
1. 該&#x200B;**事件清單**&#x200B;是從事件內容片段的序列化衍生而來，該序列化通過配置的&#x200B;**內容片段清單元件**&#x200B;公開。

## 行動應用程式展示

>[!VIDEO](https://video.tv.adobe.com/v/28345/?quality=12&learn=on)

### 設定非localhost使用的行動應用程式

如果AEM Publish未在&#x200B;**http://localhost:4503**&#x200B;上執行，則可在行動應用程式的[!DNL Settings]中更新主機和連接埠，以指向屬性AEM Publish主機／連接埠。

>[!VIDEO](https://video.tv.adobe.com/v/28344/?quality=12&learn=on)

## 在本機執行行動應用程式

1. 下載並安裝[Android Studio](https://developer.android.com/studio/install)以安裝Android模擬器。
1. **下** 載Android [!DNL APK] 檔 [案GitHub >資產> wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. 開啟&#x200B;**Android Studio**
   * 在Android Studio初次啟動時，將會出現安裝[!DNL Android SDK]的提示。 接受預設值並完成安裝。
1. 開啟Android Studio並選取「**設定檔」或「除錯APK」**
1. 選擇步驟2中下載的APK檔案(**wknd-mobile.x.x.x.apk**)，然後按一下&#x200B;**確定**
   * 如果提示&#x200B;**建立新資料夾**&#x200B;或&#x200B;**使用現有**，請選擇&#x200B;**使用現有**。
1. 在Android Studio初次啟動時，在「專案」清單中按一下滑鼠右鍵，然後選取「開啟模組設定」****。****
   * 在「**模組> wknd-mobile.x.x.x >相依性標籤**」下方，選取「**Android API 29 Platform**」。 點選「確定」以關閉並儲存變更。
   * 如果您未執行此動作，在嘗試啟動模擬器時會出現「請選取Android SDK」錯誤。
1. 通過選擇&#x200B;**工具> AVD管理器**&#x200B;或點選頂欄中的&#x200B;**AVD管理器**&#x200B;表徵圖，開啟&#x200B;**AVD管理器**。
1. 在&#x200B;**AVD管理器**&#x200B;窗口中，按一下&#x200B;**+建立虛擬設備……**（如果您尚未註冊裝置）。
   1. 在左側，選擇&#x200B;**Phone**&#x200B;類別。
   1. 選擇&#x200B;**像素2**。
   1. 按一下&#x200B;**Next**&#x200B;按鈕。
   1. 選擇&#x200B;**Q**，其中&#x200B;**API Level 29**。
      * 在AVD Manager初次啟動時，系統會要求您下載版本化API。 按一下「Q」版本旁的「下載」連結，然後完成下載和安裝。
   1. 按一下&#x200B;**Next**&#x200B;按鈕。
   1. 按一下&#x200B;**完成**&#x200B;按鈕。
1. 關閉&#x200B;**AVD管理器**&#x200B;窗口。
1. 在頂部菜單欄中，從&#x200B;**運行／編輯配置**&#x200B;下拉菜單中選擇&#x200B;**wknd-mobile.x.x.x**。
1. 點選所選&#x200B;**運行／編輯配置**&#x200B;旁的&#x200B;**運行**&#x200B;按鈕
1. 在彈出式選單中，選取新建立的&#x200B;**[!DNL Pixel 2 API 29]**&#x200B;虛擬裝置，然後點選&#x200B;**確定**
1. 如果[!DNL WKND Mobile]應用程式未立即載入，請在模擬器的Android首頁畫面中尋找並點選&#x200B;**[!DNL WKND]**&#x200B;圖示。
   * 如果模擬器啟動，但模擬器螢幕保持黑色，請在模擬器窗口旁邊的模擬器工具窗口中按一下&#x200B;**power**&#x200B;按鈕。
   * 若要在虛擬裝置內捲動，請按住並拖曳。
   * 若要從AEM重新整理內容，請從上方向下拉至「重新整理」圖示
顯示和釋放。

>[!VIDEO](https://video.tv.adobe.com/v/28341/?quality=12&learn=on)

## 行動應用程式程式碼

本節會反白顯示最能互動且依賴AEM Content Services及其JSON輸出的Android Mobile應用程式程式碼。

在載入時，行動應用程式會將`HTTP GET`設為`/content/wknd-mobile/en/api/events.model.json`，此為AEM Content Services端點，設定為提供內容以驅動行動應用程式。

由於事件API(`/content/wknd-mobile/en/api/events.model.json`)的可編輯範本已鎖定，因此行動應用程式可以編碼，以在JSON回應的特定位置尋找特定資訊。

### 高階程式碼流

1. 開啟[!DNL WKND Mobile]應用程式會叫用`HTTP GET`請求至`/content/wknd-mobile/en/api/events.model.json`的AEM Publish，以收集要填入行動應用程式UI的內容。
2. 從AEM接收內容後，行動應用程式的三個檢視元素、**標誌、標籤行和事件清單**&#x200B;中的每個元素都會以AEM的內容初始化。
   * 若要將AEM內容系結至Mobile App的檢視元素，表示每個AEM元件的JSON會對應至Java POJO的物件，而Java POJO則會系結至Android檢視元素。
      * 影像元件JSON →標誌POJO →標誌ImageView
      * 文字元件JSON → TagLine POJO → Text ImageView
      * 內容片段清單JSON → Events POJO→Events ReculerView
   * *行動應用程式程式碼可將JSON對應至POJO，因為JSON回應較多時，已知的位置。請記住，「image」、「text」和「contentfragmentlist」的JSON索引鍵是由AEM元件的後援節點名稱所指定。 如果這些節點名稱變更，行動應用程式將會中斷，因為它不知道如何從JSON資料中搜尋必要的內容。*

#### 叫用AEM Content Services端點

以下是行動應用程式`MainActivity`中負責叫用AEM Content Services以收集驅動行動應用程式體驗的內容的程式碼摘要。

```
protected void onCreate(Bundle savedInstanceState) {
    ...
    // Create the custom objects that will map the JSON -> POJO -> View elements
    final List<ViewBinder> viewBinders = new ArrayList<>();

    viewBinders.add(new LogoViewBinder(this, getAemHost(), (ImageView) findViewById(R.id.logo)));
    viewBinders.add(new TagLineViewBinder(this, (TextView) findViewById(R.id.tagLine)));
    viewBinders.add(new EventsViewBinder(this, getAemHost(), (RecyclerView) findViewById(R.id.eventsList)));
    ...
    initApp(viewBinders);
}

private void initApp(final List<ViewBinder> viewBinders) {
    final String aemUrl = getAemUrl(); // -> http://localhost:4503/content/wknd-mobile/en/api/events.mobile.json
    JsonObjectRequest request = new JsonObjectRequest(aemUrl, null,
        new Response.Listener<JSONObject>() {
            @Override
            public void onResponse(JSONObject response) {
                for (final ViewBinder viewBinder : viewBinders) {
                    viewBinder.bind(response);
                }
            }
        },
        ...
    );
}
```

`onCreate(..)` 是行動應用程式的初始化掛接，並註冊負責剖析JSON `ViewBinders` 及將值系結至元素的3個自訂 `View` 掛接。

`initApp(...)` 接著呼叫，這會使AEM Publish上的HTTP GET要求成為AEM Content Services端點，以收集內容。收到有效的JSON回應時，JSON回應會傳遞給負責剖析JSON並將其系結至行動裝置`View`元素的每個`ViewBinder`。

#### 剖析JSON回應

接下來，我們將看`LogoViewBinder`，它很簡單，但強調了幾個重要的考慮。

```
public class LogoViewBinder implements ViewBinder {
    ...
    public void bind(JSONObject jsonResponse) throws JSONException, IOException {
        final JSONObject components = jsonResponse.getJSONObject(":items").getJSONObject("root").getJSONObject(":items");

        if (components.has("image")) {
            final Image image = objectMapper.readValue(components.getJSONObject("image").toString(), Image.class);

            final String imageUrl = aemHost + image.getSrc();
            Glide.with(context).load(imageUrl).into(logo);
        } else {
            Log.d("WKND", "Missing Logo");
        }
    }
}
```

`bind(...)`的第一行透過&#x200B;**:items → root → :items**&#x200B;鍵，導覽至JSON回應，此鍵代表已新增元件的AEM版面容器。

在此處會檢查名為&#x200B;**image**&#x200B;的索引鍵，此索引鍵代表Image元件（同樣，此節點名稱→JSON索引鍵是穩定的）。 如果此對象存在，則通過Jackson `ObjectMapper`庫讀取並映射到[自定義映像POJO](#image-pojo)。 下面將探索映像POJO。

最後，標誌的`src`會使用[!DNL Glide]輔助程式庫載入Android ImageView。

請注意，我們必須將AEM架構、主機和連接埠（透過`aemHost`）提供給AEM Publish實例，因為AEM Content Services將只提供JCR路徑(即 `/content/dam/wknd-mobile/images/wknd-logo.png`)至參考內容。

#### 映像POJO{#image-pojo}

雖然可選，但是使用[Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html)或其他程式庫（例如Gson）提供的類似功能，可協助將複雜的JSON結構對應至Java POJO，而不需直接處理原生JSON物件本身。 在此簡單案例中，我們會透過`@JSONProperty`註解，將`image` JSON物件的`src`索引鍵直接對應至「影像POJO」中的`src`屬性。

```
package com.adobe.aem.guides.wknd.mobile.android.models;

import com.fasterxml.jackson.annotation.JsonProperty;

public class Image {
    @JsonProperty(value = "src")
    private String src;

    public String getSrc() {
        return src;
    }
}
```

Event POJO需要從JSON物件中選取更多資料點，它比簡單影像更有好處，我們只要`src`。

## 探索行動應用程式體驗

現在您已瞭解AEM Content Services如何推動原生Mobile體驗，請使用您學到的功能來執行下列步驟，並查看您在Mobile App中所反映的變更。

在每個步驟後，拉進以重新整理行動應用程式，並驗證行動體驗的更新。

1. 建立並發佈新&#x200B;**內容片段**[!DNL Event]
1. 取消發佈現有的[!DNL Event]內容片段&#x200B;****
1. 發佈更新至&#x200B;**標籤行**

## 恭喜

**您已完成AEM Headless教學課程！**

若要進一步瞭解AEM Content Services和AEM做為無頭CMS，請造訪Adobe的其他檔案與啟用教材：

* [使用內容片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [AEM WCM核心元件使用指南](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/introduction.html)
* [AEM WCM核心元件元件庫](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM WCM核心元件GitHub專案](https://github.com/adobe/aem-core-wcm-components)
* [AEM WCM核心元件——詢問專家](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-content-services.html)
* [元件導出器的代碼示例](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/bundle/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
