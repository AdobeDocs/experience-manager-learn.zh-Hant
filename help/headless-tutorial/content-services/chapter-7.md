---
title: 第7章 — 從行動應用程式使用AEM內容服務 — 內容服務
description: 教學課程的第7章會執行Android行動應用程式，從AEM Content Services使用製作內容。
feature: 內容片段、API
topic: 無頭式、內容管理
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1414'
ht-degree: 0%

---


# 第7章 — 從行動應用程式使用AEM內容服務

本教學課程的第7章使用原生Android行動應用程式來使用AEM內容服務中的內容。

## Android行動應用程式

本教學課程使用&#x200B;**簡單的原生Android行動應用程式**&#x200B;來使用和顯示AEM Content Services公開的事件內容。

使用[Android](https://developer.android.com/)基本上不重要，且使用中的行動應用程式可在任何行動平台（例如iOS）的任何架構中撰寫。

Android可用來進行教學課程，因為能夠在Windows、macOs和Linux上執行Android模擬器，其普及程度，而且可以以Java的形式撰寫，這是AEM開發人員非常了解的語言。

*本教學課程的Android行動應用程&#x200B;****式並非旨在指示如何建置Android行動應用程式或傳達Android開發最佳實務，而是說明如何從行動應用程式使用AEM內容服務。*

### AEM Content Services如何推動行動應用程式體驗

![行動應用程式與內容服務的對應](assets/chapter-7/content-services-mapping.png)

1. 由[!DNL Events API]頁面的&#x200B;**影像元件**&#x200B;所定義的&#x200B;**標誌**。
1. **標籤行**，如[!DNL Events API]頁的&#x200B;**文本元件**&#x200B;上所定義。
1. 此&#x200B;**事件清單**&#x200B;衍生自事件內容片段的序列化，透過設定的&#x200B;**內容片段清單元件**&#x200B;公開。

## 行動應用程式示範

>[!VIDEO](https://video.tv.adobe.com/v/28345/?quality=12&learn=on)

### 為非本地主機使用配置行動應用

如果未在&#x200B;**http://localhost:4503**&#x200B;上執行AEM Publish，可在行動應用程式的[!DNL Settings]中更新主機和連接埠，以指向屬性AEM Publish主機/連接埠。

>[!VIDEO](https://video.tv.adobe.com/v/28344/?quality=12&learn=on)

## 在本機執行行動應用程式

1. 下載並安裝[Android Studio](https://developer.android.com/studio/install)以安裝Android模擬器。
1. **** 下載Android [!DNL APK] 檔 [案GitHub >資產> wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. 開啟&#x200B;**Android Studio**
   * 首次啟動Android Studio時，會出現安裝[!DNL Android SDK]的提示。 接受預設值並完成安裝。
1. 開啟Android Studio，然後選取&#x200B;**設定檔或Debug APK**
1. 選取步驟2中下載的APK檔案(**wknd-mobile.x.x.x.apk**)，然後按一下&#x200B;**OK**
   * 如果提示&#x200B;**建立新資料夾**&#x200B;或&#x200B;**使用現有**，請選擇&#x200B;**使用現有**。
1. 在Android Studio的初次啟動時，以滑鼠右鍵按一下「專案」清單中的&#x200B;**wknd-mobile.x.x.x**，然後選取&#x200B;**開啟模組設定**。
   * 在「**模組> wknd-mobile.x.x.x >相依性」標籤下，選取「** Android API 29平台&#x200B;**」。**&#x200B;點選「確定」以關閉並儲存變更。
   * 若未這麼做，當您嘗試啟動模擬器時，會出現「請選取Android SDK」錯誤。
1. 通過選擇&#x200B;**工具> AVD管理器**&#x200B;或點選頂欄中的&#x200B;**AVD管理器**&#x200B;表徵圖，開啟&#x200B;**AVD管理器**。
1. 在&#x200B;**AVD管理器**&#x200B;窗口中，按一下&#x200B;**+建立虛擬設備……**&#x200B;如果尚未註冊設備。
   1. 在左側，選擇&#x200B;**Phone**&#x200B;類別。
   1. 選擇&#x200B;**Pixel 2**。
   1. 按一下&#x200B;**Next**&#x200B;按鈕。
   1. 選擇&#x200B;**Q**（**API級別29**）。
      * AVD管理器初次啟動時，系統會要求您下載版本控制API。 按一下「Q」版本旁的「下載」連結，然後完成下載和安裝。
   1. 按一下&#x200B;**Next**&#x200B;按鈕。
   1. 按一下&#x200B;**完成**&#x200B;按鈕。
1. 關閉&#x200B;**AVD管理器**&#x200B;窗口。
1. 在頂端功能表列中，從&#x200B;**執行/編輯設定**&#x200B;下拉式清單中選取&#x200B;**wknd-mobile.x.x.x**。
1. 點選所選&#x200B;**執行/編輯設定**&#x200B;旁的&#x200B;**執行**&#x200B;按鈕
1. 在快顯視窗中，選取新建立的&#x200B;**[!DNL Pixel 2 API 29]**&#x200B;虛擬裝置，然後點選&#x200B;**OK**
1. 如果[!DNL WKND Mobile]應用程式未立即載入，請在模擬器中從Android主畫面尋找並點選&#x200B;**[!DNL WKND]**&#x200B;圖示。
   * 如果模擬器啟動，但模擬器的螢幕保持黑色，請在模擬器窗口旁的模擬器的「工具」窗口中點選&#x200B;**power**&#x200B;按鈕。
   * 若要在虛擬裝置內捲動，請按一下並按住並拖曳。
   * 若要從AEM重新整理內容，請從上方下拉至「重新整理」圖示
顯示和發行。

>[!VIDEO](https://video.tv.adobe.com/v/28341/?quality=12&learn=on)

## 行動應用程式程式碼

本節重點說明最互動且依賴AEM Content Services的Android行動應用程式程式碼及其JSON輸出。

載入時，行動應用程式會將`HTTP GET`製作為`/content/wknd-mobile/en/api/events.model.json`，此為AEM Content Services端點，其設定為提供內容以驅動行動應用程式。

由於事件API(`/content/wknd-mobile/en/api/events.model.json`)的可編輯範本已鎖定，因此可對行動應用程式進行編碼，以在JSON回應中的特定位置中尋找特定資訊。

### 高階程式碼流量

1. 開啟[!DNL WKND Mobile]應用程式時，會在`/content/wknd-mobile/en/api/events.model.json`叫用向AEM Publish的`HTTP GET`請求，以收集要填入行動應用程式UI的內容。
2. 從AEM收到內容後，行動應用程式的三個檢視元素（**標誌、標籤行和事件清單**）的每個，都會以AEM的內容初始化。
   * 若要將AEM內容系結至行動應用程式的檢視元素，代表每個AEM元件的JSON會對應至Java POJO，而Java POJO又會系結至Android檢視元素。
      * 影像元件JSON →標誌POJO →標誌ImageView
      * 文字元件JSON → TagLine POJO →文字ImageView
      * 內容片段清單JSON →事件POJO →事件回收器檢視
   * *行動應用程式程式碼可將JSON對應至POJO，因為較大JSON回應內的已知位置。請記住， 「image」、「text」和「contentfragmentlist」的JSON索引鍵是由後備AEM元件的節點名稱指定。 如果這些節點名稱變更，行動應用程式將會中斷，因為它不知道如何從JSON資料中取得必要的內容。*

#### 叫用AEM Content Services端點

以下是行動應用程式`MainActivity`中程式碼的蒸餾，該程式碼負責叫用AEM內容服務以收集推動行動應用程式體驗的內容。

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

`onCreate(..)` 是行動應用程式的初始化連結，並註冊3個自 `ViewBinders` 訂，負責剖析JSON並將值系結至 `View` 元素。

`initApp(...)` 接著會被呼叫，這會向AEM Publish上的AEM Content Services端點提出HTTPGET要求，以收集內容。收到有效的JSON回應時，JSON回應會傳遞至每個`ViewBinder`，該負責剖析JSON並將其系結至行動`View`元素。

#### 剖析JSON回應

接下來，我們將看`LogoViewBinder`，它很簡單，但強調了幾個重要的注意事項。

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

`bind(...)`的第一行會透過索引鍵&#x200B;**:items → root →:items**&#x200B;向下導覽JSON回應，其代表元件所新增的AEM配置容器。

在此會檢查代表影像元件的&#x200B;**image**&#x200B;索引鍵(同樣地，此節點名稱→ JSON索引鍵很穩定)。 如果此物件存在，則會透過Jackson `ObjectMapper`資料庫讀取並對應至[自訂影像POJO](#image-pojo)。 以下探討影像POJO。

最後，標誌的`src`會使用[!DNL Glide]協助程式庫載入至Android ImageView中。

請注意，我們必須將AEM結構、主機和連接埠（透過`aemHost`）提供給AEM Publish執行個體，因為AEM Content Services只會提供JCR路徑(ie. `/content/dam/wknd-mobile/images/wknd-logo.png`)到參考的內容。

#### 影像POJO{#image-pojo}

雖然為選用項目，但使用[Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html)或其他資料庫（例如Gson）提供的類似功能，可協助將複雜的JSON結構對應至Java POJO，而無須繁瑣地直接處理原生JSON物件本身。 在此簡單案例中，我們會透過`@JSONProperty`附註，直接將`src`索引鍵從`image` JSON物件對應至影像POJO中的`src`屬性。

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

事件POJO需要從JSON物件中選取更多資料點，它比簡單影像更有好處，我們只想要`src`。

## 探索行動應用程式體驗

現在您已了解AEM Content Services如何促進原生行動體驗，運用您所學到的內容執行下列步驟，並查看行動應用程式中反映的變更。

在每個步驟後，提取並重新整理行動應用程式，並驗證行動體驗的更新。

1. 建立並發佈&#x200B;**新[!DNL Event]內容片段**
1. 取消發佈現有[!DNL Event]內容片段&#x200B;****
1. 發佈更新至&#x200B;**標籤行**

## 恭喜

**您已完成AEM Headless教學課程！**

若要深入了解AEM Content Services和AEM as a Headless CMS，請造訪Adobe的其他檔案和啟用資料：

* [使用內容片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [AEM WCM核心元件使用手冊](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/introduction.html)
* [AEM WCM核心元件程式庫](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM WCM核心元件GitHub專案](https://github.com/adobe/aem-core-wcm-components)
* [AEM WCM核心元件 — 詢問專家](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-content-services.html)
* [元件導出器的代碼示例](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/bundle/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
