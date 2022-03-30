---
title: 第7章 — 從AEMMobile應用程式使用內容服務 — 內容服務
description: 本教程的第7章將運行AndroidMobile應用程式，以從內容服務中AEM使用創作內容。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: d6b6d425-842a-43a9-9041-edf78e51d962
source-git-commit: 631fef25620c84e04c012c8337c9b76613e3ad46
workflow-type: tm+mt
source-wordcount: '1392'
ht-degree: 0%

---

# 第7章 — 從AEMMobile應用使用內容服務

本教程的第7章使用本機AndroidMobile應用程式從內容服務中AEM消費內容。

## 安卓Mobile應用

本教程使用 **簡單本機AndroidMobile應用** 以使用和顯示Content Services公開的事AEM件內容。

使用 [安卓](https://developer.android.com/) 這種技術在很大程度上並不重要，而且消費性移動應用可以在任何框架中編寫，適用於任何移動平台，比如iOS。

Android用於教程，是因為它能夠在Windows、macOs和Linux上運行Android模擬器，其流行性以及它可以以Java語言編寫，開發人員對這種語言有很好的了AEM解。

*本教程的AndroidMobile應用&#x200B;**不**旨在指導如何構建AndroidMobile應用或傳達Android開發最佳實踐，而是說明如何從AEMMobile應用程式使用內容服務。*

### Content Services如AEM何推動Mobile應用體驗

![MobileApp到內容服務映射](assets/chapter-7/content-services-mapping.png)

1. 的 **標誌** 定義 [!DNL Events API] 頁 **影像元件**。
1. 的 **標籤行** 定義 [!DNL Events API] 頁 **文本元件**。
1. 此 **事件清單** 源於通過配置的 **內容片段清單元件**。

## Mobile應用演示

>[!VIDEO](https://video.tv.adobe.com/v/28345/?quality=12&learn=on)

### 配置Mobile應用程式以使用非本地主機

如果未在上運行AEM發佈 **http://localhost:4503** 主機和埠可在Mobile應用中更新 [!DNL Settings] 指向屬性AEM發佈主機/埠。

>[!VIDEO](https://video.tv.adobe.com/v/28344/?quality=12&learn=on)

## 本地運行Mobile應用

1. 下載並安裝 [安卓工作室](https://developer.android.com/studio/install) 安裝Android模擬程式。
1. **下載** 安卓 [!DNL APK] 檔案 [GitHub >資產> wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. 開啟 **安卓工作室**
   * 在Android Studio的初始啟動時，將提示安裝 [!DNL Android SDK] 會出現。 接受預設值並完成安裝。
1. 開啟Android Studio並選擇 **配置檔案或調試APK**
1. 選擇APK檔案(**wknd-mobile.x.x.x.apk**)在步驟2中下載，然後按一下 **確定**
   * 如果系統提示 **建立新資料夾**&#x200B;或 **使用現有**&#x200B;選中 **使用現有**。
1. 在Android Studio的初始發佈時，按一下右鍵 **wknd-mobile.x.x.x** 在「項目」清單中，然後選擇 **開啟模組設定**。
   * 下 **「模組」>「wknd-mobile.x.x.x」>「依賴項」頁籤**&#x200B;選中 **Android API 29平台**。 按一下「OK（確定）」關閉並保存更改。
   * 如果不這樣做，則在嘗試啟動模擬器時將出現「請選擇Android SDK」錯誤。
1. 開啟 **AVD管理器** 通過 **「工具」>「AVD管理器」** 或點擊 **AVD管理器** 的雙曲餘切值。
1. 在 **AVD管理器** 窗口，按一下 **+建立虛擬設備……** 的子菜單。
   1. 在左側，選擇 **電話** 的子菜單。
   1. 選擇 **像素2**。
   1. 按一下 **下一個** 按鈕
   1. 選擇 **問** 與 **API級別29**。
      * 在AVD Manager初始啟動後，將要求您下載版本控制的API。 按一下「Q」版本旁邊的「Download（下載）」連結，然後完成下載和安裝。
   1. 按一下 **下一個** 按鈕
   1. 按一下 **完成** 按鈕
1. 關閉 **AVD管理器** 的子菜單。
1. 在頂部菜單欄中，選擇 **wknd-mobile.x.x.x** 從 **運行/編輯配置** 下拉。
1. 點擊 **運行** 按鈕 **運行/編輯配置**
1. 在彈出窗口中，選擇新建立的 **[!DNL Pixel 2 API 29]** 虛擬設備和分路 **確定**
1. 如果 [!DNL WKND Mobile] 應用不會立即載入、查找和點擊 **[!DNL WKND]** 表徵圖。
   * 如果模擬器啟動，但模擬器的螢幕保持黑色，請點擊 **電** 按鈕。
   * 要在虛擬設備內滾動，請按一下並按住並拖動。
   * 要從中刷新內AEM容，請從頂部向下拉，直到顯示「刷新」表徵圖，然後釋放。

>[!VIDEO](https://video.tv.adobe.com/v/28341/?quality=12&learn=on)

## Mobile應用代碼

本節重點介紹最能交互並依賴於內容服務及其AEMJSON輸出的AndroidMobile應用程式碼。

載入後，Mobile應用 `HTTP GET` 至 `/content/wknd-mobile/en/api/events.model.json` Content Services端AEM點配置為提供驅動Mobile應用的內容。

因為事件API的可編輯模板(`/content/wknd-mobile/en/api/events.model.json`)已鎖定，可對Mobile應用進行編碼，以在JSON響應中的特定位置查找特定資訊。

### 高級代碼流

1. 開啟 [!DNL WKND Mobile] 應用調用 `HTTP GET` 請求AEM發佈時間： `/content/wknd-mobile/en/api/events.model.json` 收集要填充Mobile應用的UI的內容。
2. 在從中收到內容AEM後，Mobile應用的三個視圖元素 **徽標、標籤行和事件清單**，已使用中的內容初始AEM化。
   * 要將內容綁定到AEMMobile應用的視圖元素，表示每個元件的JSON是映AEM射到Java POJO的對象，而Java POJO又綁定到Android View元素。
      * 影像元件JSON →徽標POJO →徽標ImageView
      * 文本元件JSON → TagLine POJO →文本ImageView
      * 內容片段清單JSON →事件POJO →事件回收器視圖
   * *Mobile應用代碼能夠將JSON映射到POJO，因為較大JSON響應中的已知位置。 請記住，「image」、「text」和「contentfragmentlist」的JSON鍵由後備元件的節AEM點名稱指定。 如果這些節點名稱更改，則Mobile應用將中斷，因為它不知道如何從JSON資料中源出所需內容。*

#### 調用AEMContent Services端點

以下是Mobile應用程式中代碼的摘要 `MainActivity` 負責調AEM用Content Services收集驅動Mobile應用體驗的內容。

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

`onCreate(..)` 是Mobile應用的初始化掛接，並註冊3個自定義 `ViewBinders` 負責分析JSON並將值綁定到 `View` 元素。

`initApp(...)` 然後調用該命令，使HTTPGET請求到AEMAEM發佈上的Content Services終點以收集內容。 在收到有效的JSON響應時，JSON響應將傳遞給每個 `ViewBinder` 負責解析JSON並將其綁定到移動 `View` 元素。

#### 分析JSON響應

下面我們將看 `LogoViewBinder`，這很簡單，但是也強調了幾個重要考慮。

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

第一行 `bind(...)` 通過鍵向下導航JSON響應 **:items →根→:item** 表示已添AEM加到的元件的佈局容器。

從此處對名為 **影像**，它表示映像元件(同樣，此節點名→ JSON鍵是穩定的)。 如果此對象存在，則它讀取並映射到 [自定義映像POJO](#image-pojo) 通過傑克森 `ObjectMapper` 的下界。 下面將探索Image POJO。

最後，標識 `src` 使用 [!DNL Glide] 幫助程式庫。

請注意，我們必須提AEM供架構、主機和埠(通過 `aemHost`)到AEM發佈實例，AEM因為Content Services將僅提供JCR路徑(即 `/content/dam/wknd-mobile/images/wknd-logo.png`)。

#### 影像POJO{#image-pojo}

可選， [Jackson對象映射器](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) 或Gson等其他庫提供的類似功能，可幫助將複雜的JSON結構映射到Java POJO，而無需直接處理本機JSON對象本身。 在這個簡單的例子中，我們 `src` 按鈕 `image` JSON對象，到 `src` 直接通過 `@JSONProperty` 注釋。

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

Event POJO需要從JSON對象中選擇更多資料點，它比簡單映像更有益，我們只希望 `src`。

## 瀏覽Mobile應用體驗

現在，您已瞭解了AEMContent Services如何推動Mobile本機體驗，請使用您學到的知識執行以下步驟並查看您在Mobile應用中所反映的更改。

在每個步驟之後，請拉出以刷新Mobile應用並驗證對移動體驗的更新。

1. 建立和發佈 **新 [!DNL Event] 內容片段**
1. 取消發佈 **現有 [!DNL Event] 內容片段**
1. 將更新發佈到 **標籤行**

## 恭喜

**您已完成無頭AEM教程！**

要瞭解有關內容服AEM務和作為無AEM頭CMS的更多資訊，請訪問Adobe的其他文檔和支援資料：

* [使用內容片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [WCMAEM核心元件使用手冊](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hant)
* [WCMAEM核心元件庫](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [WCMAEM核心元件GitHub項目](https://github.com/adobe/aem-core-wcm-components)
* [元件導出器的代碼示例](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
