---
title: Dispatcher瞭解快取
description: 瞭解Dispatcher模組如何操作其快取。
topic: Administration, Performance
version: Experience Manager 6.5
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 66ce0977-1b0d-4a63-a738-8a2021cf0bd5
duration: 407
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1708'
ht-degree: 0%

---

# 瞭解快取

[目錄](./overview.md)

[&lt; — 上一步：組態檔說明](./explanation-config-files.md)

本檔案將說明Dispatcher快取如何發生以及如何進行設定

## 快取目錄

我們在基準安裝中使用下列預設快取目錄

- 作者
   - `/mnt/var/www/author`
- 發佈者
   - `/mnt/var/www/html`

當每個請求周遊Dispatcher時，這些請求遵循設定的規則以保留本機快取版本來回應合格專案

>[!NOTE]
>
>我們刻意將已發佈的工作負載與作者工作負載分開，因為當Apache在DocumentRoot中尋找檔案時，並不知道該檔案來自哪個AEM執行個體。 因此，即使您在作者陣列中停用快取，如果作者的DocumentRoot與publisher相同，它會在出現時從快取中提供檔案。 這表示您將會從發佈的快取中提供作者檔案，並為您的訪客創造非常糟糕的混合比對體驗。
>
>為不同的發佈內容保留單獨的DocumentRoot目錄也是個非常糟糕的想法。 您必須建立多個重新快取的專案，這些專案在clientlibs之類的網站之間沒有差異，並且必須為您設定的每個DocumentRoot設定復寫排清代理程式。 增加每次頁面啟動時的頭頂排清量。 依賴檔案的名稱空間及其完整快取路徑，並避免發佈網站有多個DocumentRoot。

## 組態檔

Dispatcher控制任何伺服器陣列檔案的`/cache {`區段中符合快取條件的專案。 
在AMS基準設定陣列中，您會找到包含專案，如下所示：


```
/cache { 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
    }
```


建立要快取或不快取的規則時，請參閱檔案[這裡](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=zh-Hant#configuring-the-dispatcher-cache-cache)


## 快取作者

我們已經看到許多使用者不會快取作者內容的實作。 
他們錯過了大幅提升的效能和對作者的回應能力。

讓我們來談談在設定作者陣列以正確快取時所採取的策略。

以下是作者伺服器陣列檔案的基本作者`/cache {`區段：


```
/cache { 
    /docroot "/mnt/var/www/author" 
    /statfileslevel "2" 
    /allowAuthorized "1" 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
    } 
    /invalidate { 
        /0000 { 
            /glob "*" 
            /type "allow" 
        } 
    } 
    /allowedClients { 
        /0000 { 
            /glob "*.*.*.*" 
            /type "deny" 
        } 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_invalidate_allowed.any" 
    } 
}
```

這裡要注意的重要事項是`/docroot`已設定為作者的快取目錄。

>[!NOTE]
>
>確定您在作者`.vhost`檔案中的`DocumentRoot`符合陣列`/docroot`引數

快取規則include陳述式包含檔案`/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any`，其中包含下列規則：

```
/0000 { 
 /glob "*" 
 /type "deny" 
} 
/0001 { 
 /glob "/libs/*" 
 /type "allow" 
} 
/0002 { 
 /glob "/libs/*.html" 
 /type "deny" 
} 
/0003 { 
 /glob "/libs/granite/csrf/token.json" 
 /type "deny" 
} 
/0004 { 
 /glob "/apps/*" 
 /type "allow" 
} 
/0005 { 
 /glob "/apps/*.html" 
 /type "deny" 
} 
/0006 { 
 /glob "/libs/cq/core/content/welcome.*" 
 /type "deny" 
}
```

在作者情境中，內容會隨時隨心所欲地變更。 您只想快取不會經常變更的專案。
我們有要快取`/libs`的規則，因為這些規則是基準AEM安裝的一部分，且在您安裝Service Pack、Cumulative Fix Pack、Upgrade或Hotfix之前可能會變更。 因此，快取這些元素相當合理，而且使用網站的一般使用者在製作體驗上也確實有極大的好處。

>[!NOTE]
>
>請記住，這些規則也會快取<b>`/apps`</b>這是自訂應用程式程式碼所在的位置。 如果您正在此執行個體上開發程式碼，當您儲存檔案時，會發現這會非常令人困惑，並且由於提供快取復本，看不到是否會反映在UI中。 這裡的用意是，如果您將程式碼部署到AEM中，頻率也會很低，而且部署步驟的一部分應該是要清除作者快取。 同樣地，其優點也是巨大的，可讓您的可快取程式碼更快速地為使用者執行。

## ServeOnStale （亦稱為陳舊/SOS服務）

這是Dispatcher功能的其中一項gem。 如果發佈者負載過重或變得無回應，通常會擲回502或503 http回應代碼。 如果發生上述情況並啟用此功能，系統將會指示Dispatcher盡最大努力仍提供快取中的內容，即使該內容不是全新復本。 如果您已擁有某樣功能，最好還是提供該功能，而不是只顯示錯誤訊息而不提供任何功能。

>[!NOTE]
>
>請記住，如果發佈者轉譯器發生通訊端逾時或500錯誤訊息，則此功能不會觸發。 如果AEM無法連線，此功能不會產生任何效用

此設定可以在任何伺服器陣列中設定，但只有在發佈伺服器陣列檔案上套用此設定才有意義。 以下是陣列檔案中啟用的功能之語法範例：

```
/cache { 
    /serveStaleOnError "1"
```

## 使用查詢引數/引數快取頁面

>[!NOTE]
>
>Dispatcher模組的其中一個正常行為是，如果要求在URI中有查詢引數（通常顯示為`/content/page.html?myquery=value`），它將略過快取檔案並直接前往AEM執行個體。 其將此請求視為動態頁面，不應加以快取。 這可能會對快取效率造成不良影響。

請參閱此[文章](https://github.com/adobe/aem-dispatcher-optimizer-tool/blob/main/docs/Rules.md#dot---the-dispatcher-publish-farm-cache-should-have-its-ignoreurlparams-rules-configured-in-an-allow-list-manner)，說明重要的查詢引數如何影響您的網站效能。

依預設，您想要將`ignoreUrlParams`規則設定為允許`*`。  這表示所有查詢引數都會被忽略，並允許所有頁面的快取，無論使用的引數為何。

以下是有人建立社群媒體深層連結參考機制的範例，此機制使用URI中的引數參考來瞭解此人的來源。

*可忽略的範例：*

- https://www.we-retail.com/home.html?reference=android
- https://www.we-retail.com/home.html?reference=facebook

此頁面可100%快取，但因為引數存在而未快取。 
將您的`ignoreUrlParams`設定為允許清單將有助於修正此問題：

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
    }
```

現在，當Dispatcher看到請求時，將會忽略請求具有`?`參考的`query`引數，並且仍快取頁面的事實

<b>動態範例：</b>

- https://www.we-retail.com/search.html?q=fruit
- https://www.we-retail.com/search.html?q=vegetables

請記住，如果您的查詢引數造成頁面變更，則頁面會呈現輸出，然後您需要將其從忽略的清單中排除，並再次將頁面取消快取。  例如，使用查詢引數的搜尋頁面會變更轉譯的原始html。

以下是每個搜尋的html來源：

`/search.html?q=fruit`：

```
<html>
...SNIP...
    <div id='results'>
        <div class='result'>
            Orange
        </div>
        <div class='result'>
            Apple
        </div>
        <div class='result'>
            Strawberry
        </div>
    </div>
</html>
```

`/search.html?q=vegetables`：

```
<html>
...SNIP...
    <div id='results'>
        <div class='result'>
            Carrot
        </div>
        <div class='result'>
            Cucumber
        </div>
        <div class='result'>
            Celery
        </div>
    </div>
</html>
```

如果您先造訪`/search.html?q=fruit`，則它會快取html以顯示結果的結果。

接著您造訪`/search.html?q=vegetables`秒，但結果會顯示水果。
這是因為`q`的查詢引數在快取方面被忽略。  若要避免此問題，您需要記錄會根據查詢引數呈現不同HTML的頁面，並拒絕快取。

範例：

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
        /0002 { /glob "q" /type "deny"  }
    }
```

透過Javascript使用查詢引數的頁面仍可完全運作，忽略此設定中的引數。  因為它們不會變更html靜態檔案。  他們使用javascript在本機瀏覽器上即時更新瀏覽器。  這表示如果您使用javascript查詢引數，就很可能忽略此引數以用於頁面快取。  允許該頁面快取並享受效能提升！

>[!NOTE]
>
>追蹤這些頁面確實需要一些維護，但絕對值得效能的提升。  您可以要求您的CSE對您的網站流量執行報告，提供您過去90天使用查詢引數的所有頁面清單，以便您分析並確保您知道要檢視哪些頁面以及不要忽略哪些查詢引數

## 快取回應標頭

很明顯，Dispatcher會快取`.html`頁面和clientlibs （亦即`.js`、`.css`），但您知道它也可以將特定回應標題連同內容快取，放在具有相同名稱，但副檔名為`.h`的檔案中。 這樣不僅可對內容進行下一個回應，還可對快取中應隨附的回應標頭進行回應。

AEM可處理的不僅僅是UTF-8編碼

有時專案具有特殊的標頭，可協助控制快取TTL的編碼詳細資訊和上次修改的時間戳記。

這些值在快取時依預設會移除，Apache httpd Webserver會以其一般檔案處理方法（通常僅限於根據檔案副檔名進行MIME型別猜測）自行處理資產。

如果您有Dispatcher快取資產和所需的標頭，您可以公開適當的體驗，並向使用者端瀏覽器保證所有詳細資訊。

以下是陣列範例，其具有要快取的標頭：

```
/cache { 
 /headers { 
  "Cache-Control" 
  "Content-Disposition" 
  "Content-Type" 
  "Expires" 
  "Last-Modified" 
  "X-Content-Type-Options" 
 } 
}
```


在此範例中，他們已設定AEM提供標頭，讓CDN尋找該標頭以瞭解何時讓它的快取失效。 這表示現在AEM可以根據標題正確指定哪些檔案會失效。

>[!NOTE]
>
>請記住，您不能使用規則運算式或glob比對。 這是要快取的標頭的常值清單。 只放入您要快取的常值標頭清單中。

## 自動讓寬限期失效

在擁有來自可執行許多頁面啟動之作者的大量活動的AEM系統上，您可能會有發生重複無效判定的競爭條件。 不需要大量重複的排清請求，您可以建置一些容許度，在寬限期清除之前不要重複排清。

### 此運作方式的範例：

如果您有5個使`/content/exampleco/en/`失效的請求，則所有請求都會在3秒內發生。

若關閉此功能，您將會使快取目錄`/content/exampleco/en/`失效5次

此功能開啟且設為5秒時，將會使快取目錄`/content/exampleco/en/` <b>失效</b>一次

以下是為5秒寬限期設定的此功能語法範例：

```
/cache { 
    /gracePeriod "5"
```

## TTL型失效

Dispatcher模組的較新功能是針對快取的專案以`Time To Live (TTL)`個失效選項為基礎。 當專案被快取時，它會尋找是否存在快取控制標題，並在快取目錄中產生具有相同名稱和`.ttl`副檔名的檔案。

以下是在伺服器陣列設定檔案中設定的功能範例：

```
/cache { 
    /enableTTL "1"
```

>[!NOTE]
>
>請記住，仍需將AEM設定為傳送TTL標頭以供Dispatcher遵循。 切換此功能只會讓Dispatcher知道何時移除AEM傳送快取控制標題的檔案。 如果AEM未開始傳送TTL標題，則Dispatcher不會在此處執行任何特殊操作。

## 快取篩選規則

以下是要在發佈器上快取元素的基準設定範例：

```
/cache{ 
    /0000 { 
        /glob "*" 
        /type "allow" 
    } 
    /0001 { 
        /glob "/libs/granite/csrf/token.json" 
        /type "deny" 
    }
```

我們希望讓已發佈的網站儘可能貪婪，並快取所有內容。

如果快取時存在中斷體驗的元素，您可以新增規則以移除快取該專案的選項。 如上述範例所示，不應快取csrf代號且將其排除。 您可以在[這裡](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=zh-Hant#configuring-the-dispatcher-cache-cache)找到撰寫這些規則的詳細資料

[下一個 — >使用和瞭解變數](./variables.md)
