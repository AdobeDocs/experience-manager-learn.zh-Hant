---
title: 第7章 — 從行動應用程式使用AEM內容服務 — 內容服務
description: 教學課程的第7章會執行Android行動應用程式，從AEM Content Services使用製作內容。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: d6b6d425-842a-43a9-9041-edf78e51d962
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1391'
ht-degree: 0%

---

# 第7章 — 從行動應用程式使用AEM內容服務

本教學課程的第7章使用原生Android行動應用程式來使用AEM內容服務中的內容。

## Android行動應用程式

本教學課程使用 **簡單原生Android行動應用程式** 來使用和顯示AEM Content Services公開的事件內容。

使用 [Android](https://developer.android.com/) 基本上不重要，且耗用的行動應用程式可在任何行動平台(例如iOS)的任何架構中編寫。

Android可用來進行教學課程，因為能夠在Windows、macOs和Linux上執行Android模擬器，其普及程度，而且可以以Java的形式撰寫，這是AEM開發人員非常了解的語言。

*本教學課程的Android行動應用程式為&#x200B;**not**旨在指示如何建置Android行動應用程式或傳達Android開發最佳實務，而非說明如何從行動應用程式使用AEM內容服務。*

### AEM Content Services如何推動行動應用程式體驗

![行動應用程式與內容服務的對應](assets/chapter-7/content-services-mapping.png)

1. 此 **標誌** 由定義 [!DNL Events API] page&#39;s **影像元件**.
1. 此 **標籤行** 定義於 [!DNL Events API] page&#39;s **文字元件**.
1. 此 **事件清單** 衍生自事件內容片段的序列化，透過設定 **內容片段清單元件**.

## 行動應用程式示範

>[!VIDEO](https://video.tv.adobe.com/v/28345/?quality=12&learn=on)

### 為非本地主機使用配置行動應用

如果未在上執行AEM Publish **http://localhost:4503** 可在行動應用程式的 [!DNL Settings] 指向屬性AEM Publish主機/連接埠。

>[!VIDEO](https://video.tv.adobe.com/v/28344/?quality=12&learn=on)

## 在本機執行行動應用程式

1. 下載並安裝 [Android Studio](https://developer.android.com/studio/install) 安裝Android模擬器。
1. **下載** Android [!DNL APK] 檔案 [GitHub >資產> wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. 開啟 **Android Studio**
   * 初次啟動Android Studio時，系統會提示安裝 [!DNL Android SDK] 會出現。 接受預設值並完成安裝。
1. 開啟Android Studio並選取 **設定檔或除錯APK**
1. 選取APK檔案(**wknd-mobile.x.x.x.apk**)下載，然後按一下 **確定**
   * 如果系統提示 **建立新資料夾**，或 **使用現有**，選取 **使用現有**.
1. 在Android Studio初次啟動時，以滑鼠右鍵按一下 **wknd-mobile.x.x.x** 在「項目」清單中，然後選擇 **開啟模組設定**.
   * 在 **「模組」>「 wknd-mobile.x.x.x」>「相依性」標籤**，選取 **Android API 29平台**. 點選「確定」以關閉並儲存變更。
   * 若未這麼做，當您嘗試啟動模擬器時，會出現「請選取Android SDK」錯誤。
1. 開啟 **AVD管理員** 選取 **工具> AVD管理器** 或點選 **AVD管理員** 圖示。
1. 在 **AVD管理員** 按一下 **+建立虛擬設備……** 如果尚未註冊設備。
   1. 在左側，選取 **電話** 類別。
   1. 選取 **像素2**.
   1. 按一下 **下一個** 按鈕。
   1. 選擇 **Q** with **API層級29**.
      * 首次啟動AVD管理器時，系統會要求您下載版本控制API。 按一下「Q」版本旁的「下載」連結，然後完成下載和安裝。
   1. 按一下 **下一個** 按鈕。
   1. 按一下 **完成** 按鈕。
1. 關閉 **AVD管理員** 窗口。
1. 在頂端功能表列中選取 **wknd-mobile.x.x.x** 從 **運行/編輯配置** 下拉。
1. 點選 **執行** 按鈕 **運行/編輯配置**
1. 在快顯視窗中，選取新建立的 **[!DNL Pixel 2 API 29]** 虛擬裝置和點選 **確定**
1. 若 [!DNL WKND Mobile] 應用程式不會立即載入、尋找及點選 **[!DNL WKND]** 圖示。
   * 如果模擬器啟動，但模擬器的螢幕保持黑色，請點選 **功率** 按鈕（位於模擬器窗口旁）。
   * 若要在虛擬裝置內捲動，請按一下並按住並拖曳。
   * 若要從AEM重新整理內容，請從上方下拉至「重新整理」圖示顯示，然後發行。

>[!VIDEO](https://video.tv.adobe.com/v/28341/?quality=12&learn=on)

## 行動應用程式程式碼

本節重點說明最互動且依賴AEM Content Services的Android行動應用程式程式碼及其JSON輸出。

載入時，行動應用程式會 `HTTP GET` to `/content/wknd-mobile/en/api/events.model.json` 即AEM Content Services端點，設定為提供內容以驅動行動應用程式。

因為事件API的可編輯範本(`/content/wknd-mobile/en/api/events.model.json`)已鎖定，則行動應用程式可以編碼，以在JSON回應中的特定位置中尋找特定資訊。

### 高階程式碼流量

1. 開啟 [!DNL WKND Mobile] 應用調用 `HTTP GET` 請求AEM發佈於 `/content/wknd-mobile/en/api/events.model.json` 收集要填入行動應用程式UI的內容。
2. 從AEM收到內容後，行動應用程式的三個檢視元素中的每個元素， **標誌、標籤行和事件清單**，會以AEM中的內容初始化。
   * 若要將AEM內容系結至行動應用程式的檢視元素，代表每個AEM元件的JSON會對應至Java POJO，而Java POJO又會系結至Android檢視元素。
      * 影像元件JSON →標誌POJO →標誌ImageView
      * 文字元件JSON → TagLine POJO →文字ImageView
      * 內容片段清單JSON →事件POJO →事件回收器檢視
   * *行動應用程式程式碼可將JSON對應至POJO，因為較大JSON回應內的已知位置。 請記住， 「image」、「text」和「contentfragmentlist」的JSON索引鍵是由後備AEM元件的節點名稱指定。 如果這些節點名稱變更，行動應用程式將會中斷，因為它不知道如何從JSON資料取得必要的內容。*

#### 叫用AEM Content Services端點

以下是行動應用程式中程式碼的精餾程式 `MainActivity` 負責叫用AEM Content Services以收集推動行動應用程式體驗的內容。

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

`onCreate(..)` 是行動應用程式的初始化連結，並註冊3個自訂 `ViewBinders` 負責剖析JSON並將值系結至 `View` 元素。

`initApp(...)` 接著會被呼叫，這會向AEM Publish上的AEM Content Services端點提出HTTPGET要求，以收集內容。 收到有效的JSON回應時，JSON回應會傳遞至每個 `ViewBinder` 負責剖析JSON並將其系結至行動裝置 `View` 元素。

#### 剖析JSON回應

接下來，我們將查看 `LogoViewBinder`，此方法相當簡單，但會強調數個重要考量。

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

第一行 `bind(...)` 透過索引鍵向下導覽JSON回應 **:items →根→:items** 代表元件所新增的AEM Layout Container。

在此會檢查名為 **影像**，代表影像元件(同樣地，此節點名稱→ JSON金鑰為穩定)。 如果此物件存在，則會讀取並對應至 [自訂影像POJO](#image-pojo) 通過傑克森 `ObjectMapper` 程式庫。 以下探討影像POJO。

最後，標誌 `src` 會使用 [!DNL Glide] 協助程式庫。

請注意，我們必須提供AEM結構、主機和連接埠(透過 `aemHost`)以AEM Content Services形式發佈至AEM Publish例項只會提供JCR路徑(ie. `/content/dam/wknd-mobile/images/wknd-logo.png`)到參考的內容。

#### 影像POJO{#image-pojo}

若為選用項目，請使用 [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) 或其他程式庫（例如Gson）提供的類似功能，可協助將複雜的JSON結構對應至Java POJO，而不需直接處理原生JSON物件本身。 在這個簡單的案例中，我們會將 `src` 鍵 `image` JSON物件，至 `src` 屬性(透過 `@JSONProperty` 註解。

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

事件POJO需要從JSON物件中選取更多資料點，這項技術的優點多於簡單影像，我們只想要 `src`.

## 探索行動應用程式體驗

現在您已了解AEM Content Services如何促進原生行動體驗，運用您所學到的內容執行下列步驟，並查看行動應用程式中反映的變更。

在每個步驟後，提取並重新整理行動應用程式，並驗證行動體驗的更新。

1. 建立和發佈 **new [!DNL Event] 內容片段**
1. 取消發佈 **現有 [!DNL Event] 內容片段**
1. 將更新發佈至 **標籤行**

## 恭喜

**您已完成AEM Headless教學課程！**

若要深入了解AEM Content Services和AEM as a Headless CMS，請造訪Adobe的其他檔案和啟用資料：

* [使用內容片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [AEM WCM核心元件使用手冊](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)
* [AEM WCM核心元件程式庫](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM WCM核心元件GitHub專案](https://github.com/adobe/aem-core-wcm-components)
* [元件導出器的代碼示例](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
