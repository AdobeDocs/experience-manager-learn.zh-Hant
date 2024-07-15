---
title: 第7章 — 從行動應用程式使用AEM Content Services — 內容服務
description: 教學課程的第7章會執行Android行動應用程式，以使用來自AEM Content Services的編寫內容。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: d6b6d425-842a-43a9-9041-edf78e51d962
duration: 467
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1350'
ht-degree: 0%

---

# 第7章 — 從行動應用程式使用AEM內容服務

教學課程的第7章使用原生Android行動應用程式，取用AEM Content Services的內容。

## Android行動應用程式

此教學課程使用&#x200B;**簡單的原生Android行動應用程式**，以使用及顯示AEM Content Services公開的事件內容。

使用[Android](https://developer.android.com/)基本上不重要，而且耗用的行動應用程式可以在任何行動平台的任何架構中撰寫，例如iOS。

Android之所以用於教學課程，是因為其可在Windows、macOs和Linux上執行Android模擬器，以及其普及度，以及可寫為Java (AEM開發人員完全瞭解的一種語言)。

*教學課程的Android行動應用程式&#x200B;**並非**旨在指示如何建置Android行動應用程式或傳達Android開發最佳實務，而是說明如何從行動應用程式使用AEM Content Services。*

### AEM Content Services如何推動行動應用程式體驗

![行動應用程式與內容服務的對應](assets/chapter-7/content-services-mapping.png)

1. 由[!DNL Events API]頁面的&#x200B;**影像元件**&#x200B;定義的&#x200B;**標誌**。
1. 在[!DNL Events API]頁面的&#x200B;**文字元件**&#x200B;上定義的&#x200B;**標籤行**。
1. 此&#x200B;**事件清單**&#x200B;衍生自事件內容片段的序列化，透過設定的&#x200B;**內容片段清單元件**&#x200B;公開。

## 行動應用程式示範

>[!VIDEO](https://video.tv.adobe.com/v/28345?quality=12&learn=on)

### 設定行動應用程式以供非localhost使用

如果AEM Publish不是在&#x200B;**http://localhost:4503**&#x200B;上執行，則可在行動應用程式的[!DNL Settings]中更新主機和連線埠，以指向屬性AEM Publish主機/連線埠。

>[!VIDEO](https://video.tv.adobe.com/v/28344?quality=12&learn=on)

## 在本機執行行動應用程式

1. 下載並安裝[Android Studio](https://developer.android.com/studio/install)以安裝Android模擬器。
1. **下載** Android [!DNL APK]檔案[GitHub > Assets > wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. 開啟&#x200B;**Android Studio**
   * 初次啟動Android Studio時，將會出現安裝[!DNL Android SDK]的提示。 接受預設值並完成安裝。
1. 開啟Android Studio並選取&#x200B;**設定檔或偵錯APK**
1. 選取在步驟2中下載的APK檔案(**wknd-mobile.x.x.x.apk**)，然後按一下[確定] ****
   * 如果系統提示&#x200B;**建立新資料夾**，或&#x200B;**使用現有**，請選取&#x200B;**使用現有**。
1. 初次啟動Android Studio時，在[專案]清單中的&#x200B;**wknd-mobile.x.x.x**&#x200B;上按一下滑鼠右鍵，然後選取&#x200B;**開啟模組設定**。
   * 在&#x200B;**模組> wknd-mobile.x.x.x >相依性標籤**&#x200B;下，選取&#x200B;**Android API 29平台**。 點選「確定」以關閉並儲存變更。
   * 如果不這麼做，您會在嘗試啟動模擬器時看到「請選取Android SDK」錯誤。
1. 選取&#x200B;**工具> AVD管理員**&#x200B;或點選頂端列中的&#x200B;**AVD管理員**&#x200B;圖示，開啟&#x200B;**AVD管理員**。
1. 如果您尚未註冊裝置，請在&#x200B;**AVD管理員**&#x200B;視窗中按一下&#x200B;**+建立虛擬裝置……**。
   1. 在左側，選取&#x200B;**電話**&#x200B;類別。
   1. 選取&#x200B;**畫素2**。
   1. 按一下&#x200B;**下一步**&#x200B;按鈕。
   1. 選取&#x200B;**API層級29**&#x200B;的&#x200B;**Q**。
      * AVD Manager初次啟動時，系統會要求您下載已建立版本的API。 按一下「Q」版旁的「下載」連結，完成下載和安裝。
   1. 按一下&#x200B;**下一步**&#x200B;按鈕。
   1. 按一下&#x200B;**完成**&#x200B;按鈕。
1. 關閉&#x200B;**AVD管理員**&#x200B;視窗。
1. 在頂端功能表列中，從&#x200B;**執行/編輯設定**&#x200B;下拉式清單中選取&#x200B;**wknd-mobile.x.x.x**。
1. 點選所選&#x200B;**執行/編輯設定**&#x200B;旁的&#x200B;**執行**&#x200B;按鈕
1. 在快顯視窗中，選取新建立的&#x200B;**[!DNL Pixel 2 API 29]**&#x200B;虛擬裝置，然後點選&#x200B;**確定**
1. 如果[!DNL WKND Mobile]應用程式未立即載入，請從模擬器的Android首頁畫面中尋找並點選&#x200B;**[!DNL WKND]**&#x200B;圖示。
   * 如果模擬器啟動，但模擬器的畫面保持黑色，請點選模擬器視窗旁的模擬器工具視窗中的&#x200B;**電源**&#x200B;按鈕。
   * 若要在虛擬裝置內捲動，請按住並拖曳。
   * 若要從AEM重新整理內容，請從頂端下拉直到「重新整理」圖示為止
顯示和發行。

>[!VIDEO](https://video.tv.adobe.com/v/28341?quality=12&learn=on)

## 行動應用程式程式碼

本節重點說明互動最多、且取決於AEM Content Services及其JSON輸出的Android行動應用程式程式碼。

載入時，行動應用程式會將`HTTP GET`設為`/content/wknd-mobile/en/api/events.model.json`，這是AEM Content Services的端點，設定為提供驅動行動應用程式的內容。

由於已鎖定Events API (`/content/wknd-mobile/en/api/events.model.json`)的可編輯範本，因此行動應用程式可經過編碼，以在JSON回應中的特定位置中尋找特定資訊。

### 高階程式碼流程

1. 開啟[!DNL WKND Mobile]應用程式會叫用`HTTP GET`對位於`/content/wknd-mobile/en/api/events.model.json`的AEM Publish的請求，以收集內容以填入行動應用程式的UI。
2. 收到來自AEM的內容時，行動應用程式的三個檢視元素（**標誌、標籤行和事件清單**）中的每一個都會以AEM中的內容初始化。
   * 若要將AEM內容繫結至行動應用程式的檢視元素，代表每個AEM元件的JSON將對映至Java POJO的物件，而該Java POJO又繫結至Android檢視元素。
      * 影像元件JSON →標誌POJO →標誌影像檢視
      * 文字元件JSON→TagLine POJO→文字ImageView
      * 內容片段清單JSON →事件POJO →事件RecyclerView
   * *行動應用程式程式碼可將JSON對應至POJO，因為較大的JSON回應中有知名的位置。 請記住，「image」、「text」和「contentfragmentlist」的JSON索引鍵是由後援AEM元件的節點名稱所指定。 如果這些節點名稱變更，行動應用程式將會中斷，因為它不知道如何從JSON資料取得必要內容。*

#### 叫用AEM Content Services端點

以下是行動應用程式`MainActivity`中負責叫用AEM Content Services來收集驅動行動應用程式體驗之內容的程式碼精選。

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

`onCreate(..)`是行動應用程式的初始化連結，並註冊3個負責剖析JSON並將值繫結至`View`元素的自訂`ViewBinders`。

然後呼叫`initApp(...)`，向AEM Publish上的AEM Content Services端點發出HTTPGET要求以收集內容。 收到有效的JSON回應時，JSON回應會傳遞至每個`ViewBinder`，負責剖析JSON並將其繫結至行動`View`元素。

#### 剖析JSON回應

接下來，我們將審視`LogoViewBinder`，其步驟很簡單，但會強調幾個重要考量。

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

`bind(...)`的第一行透過索引鍵&#x200B;**：items → root → ：items**&#x200B;向下導覽JSON回應，該索引鍵代表加入元件的AEM配置容器。

從這裡可以檢查名為&#x200B;**image**&#x200B;的索引鍵，它代表影像元件(同樣重要的是，此節點名稱→JSON索引鍵必須是穩定的)。 如果此物件存在，則會透過Jackson `ObjectMapper`資料庫讀取並對應至[自訂影像POJO](#image-pojo)。 以下探索影像POJO。

最後，會使用[!DNL Glide]協助程式程式庫將標誌的`src`載入Android ImageView。

請注意，我們必須提供AEM結構描述、主機和連線埠（透過`aemHost`）給AEM Publish執行個體，因為AEM Content Services只會提供JCR路徑(即 `/content/dam/wknd-mobile/images/wknd-logo.png`)至參考的內容。

#### 影像POJO{#image-pojo}

使用[Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html)或其他類似程式庫（例如Gson）所提供的類似功能（雖然是選用專案），有助於將複雜的JSON結構對應至Java POJO，而不需要直接處理原生JSON物件本身的繁瑣。 在這個簡單的案例中，我們會直接透過`@JSONProperty`註解，將`image` JSON物件中的`src`索引鍵對應到影像POJO中的`src`屬性。

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

事件POJO需要從JSON物件選取更多資料點，比起簡單的影像（我們只想要的是`src`），這個技巧的好處更多。

## 探索行動應用程式體驗

現在您已瞭解AEM Content Services如何推動原生行動體驗，請使用您已學到的知識執行以下步驟，並檢視您的變更反映在行動應用程式中。

在每個步驟後，提取以重新整理行動應用程式，並驗證行動體驗的更新。

1. 建立並發佈&#x200B;**新[!DNL Event]內容片段**
1. 取消發佈&#x200B;**現有[!DNL Event]內容片段**
1. Publish **標籤行**&#x200B;的更新

## 恭喜

**您已完成AEM Headless教學課程！**

若要進一步瞭解AEM Content Services和AEM as a Headless CMS，請造訪Adobe的其他檔案和啟用資料：

* [使用內容片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [AEM WCM Core Components使用手冊](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hant)
* [AEM WCM核心元件元件庫](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM WCM核心元件GitHub專案](https://github.com/adobe/aem-core-wcm-components)
* [元件匯出程式的程式碼範例](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
