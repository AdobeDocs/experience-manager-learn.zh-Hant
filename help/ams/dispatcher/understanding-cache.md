---
title: Dispatcher了解快取
description: 了解Dispatcher模組如何運作其快取。
topic: Administration, Performance
version: 6.5
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '1747'
ht-degree: 1%

---


# 了解快取

[目錄](./overview.md)

[&lt; — 上一個：組態檔說明](./explanation-config-files.md)

本檔案將說明Dispatcher快取的發生方式，以及如何設定

## 快取目錄

我們在基線安裝中使用以下預設快取目錄

- 作者
   - `/mnt/var/www/author`
- 發佈者
   - `/mnt/var/www/html`

當每個請求周遊Dispatcher時，請求會遵循已設定的規則，以保留本機快取版本，以回應符合條件的項目

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

我們刻意將已發佈的工作量與製作工作量分開，因為當Apache在DocumentRoot中尋找檔案時，它不知道它來自哪個AEM執行個體。 因此，即使您已在製作伺服器陣列中停用快取，如果作者的DocumentRoot與發佈者相同，則會在有檔案時從快取中提供檔案。 這表示您會從已發佈的快取中提供作者檔案，並為訪客提供非常糟糕的混合比對體驗。

為不同的已發佈內容保留不同的DocumentRoot目錄也是一個非常糟糕的主意。 您必須建立多個重新快取的項目，這些項目在客戶端等站點之間沒有差別，並且必須為您設定的每個DocumentRoot設定一個複製刷新代理。 每次頁面啟動時，增加頭頂的排清量。 請仰賴檔案的命名空間及其完整快取路徑，並避免發佈網站出現多個DocumentRoot。
</div>

## 組態檔

Dispatcher會控制 `/cache {` 任何伺服器陣列檔案的區段。 
在AMS基線設定場中，您會找到包含的項目，如下所示：


```
/cache { 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
    }
```


建立快取或不快取的規則時，請參閱本檔案 [此處](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)


## 快取作者

我們已看到許多實作中，使用者未快取作者內容。 
他們在效能和對作者的回應能力方面都未取得重大提升。

讓我們說說在將製作伺服器陣列設定為正確快取時所採用的策略。

以下是基本作者 `/cache {` 作者群檔案的區段：


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

此處需注意的重要事項為 `/docroot` 已設為作者的快取目錄。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

請確定您的 `DocumentRoot` 在作者的 `.vhost` 檔案與伺服器陣列匹配 `/docroot` 參數
</div>

快取規則包括語句包括檔案 `/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any` 其中包含以下規則：

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

在製作案例中，內容會隨著時間和目的而變更。 您只想快取不會經常變更的項目。
我們有快取規則 `/libs` 因為它們是基準AEM安裝的一部分，在您安裝Service Pack、Cumulative Fix Pack、升級或Hotfix前，這些變更都會發生。 因此，快取這些元素很有意義，而且使用網站的使用者的製作體驗確實有巨大的好處。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

請記住，這些規則也會快取 <b>`/apps`</b> 這是自訂應用程式程式碼所在的位置。 如果您要在此執行個體上開發程式碼，則儲存檔案時會非常困惑，而且由於提供快取副本，因此UI中沒有反映。 其意圖是，如果您將程式碼部署至AEM中，也不常使用，部分部署步驟應是清除製作快取。 同樣的好處是讓您的可快取程式碼對一般使用者執行得更快。
</div>


## ServeOnStale(AKA Serve on Stale / SOS)

這是Dispatcher功能的其中一項。 如果發佈者正在載入中，或已停止回應，通常會擲回502或503 http回應代碼。 如果發生此情況並啟用此功能，即使不是新復本，系統仍會指示Dispatcher將任何內容仍保留在快取中的內容，以盡量提供。 如果您有，最好提供某個功能，而不只是顯示沒有功能的錯誤訊息。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

請記住，如果發佈者轉譯器有通訊端逾時或500錯誤訊息，則此功能不會觸發。 如果AEM無法連線，此功能就不會執行任何作業
</div>

此設定可在任何伺服器陣列中設定，但只有將其套用至發佈伺服器陣列檔案才可行。 以下是在伺服器陣列檔案中啟用功能的語法範例：

```
/cache { 
    /serveStaleOnError "1"
```

## 使用查詢參數/引數快取頁面

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

Dispatcher模組的一般行為之一，是如果請求在URI中有查詢參數（通常如下所示） `/content/page.html?myquery=value`)會略過快取檔案，然後直接前往AEM例項。 它認為此請求是動態頁面，不應進行快取。 這可能會對快取效率造成不良影響。
</div>
<br/>

看這個 [文章](https://github.com/adobe/aem-dispatcher-optimizer-tool/blob/main/docs/Rules.md#dot---the-dispatcher-publish-farm-cache-should-have-its-ignoreurlparams-rules-configured-in-an-allow-list-manner) 顯示重要查詢參數對網站效能的影響。

依預設，您要設定 `ignoreUrlParams` 規則允許 `*`.  這表示會忽略所有查詢參數，並允許快取所有頁面，而無論使用什麼參數。

以下是某人建立社交媒體深層連結參考機制的範例，該機制會使用URI中的引數參考來了解該人員來自何處。

<b>可忽略的範例：</b>

- https://www.we-retail.com/home.html?reference=android
- https://www.we-retail.com/home.html?reference=facebook

頁面可100%快取，但無法快取，因為引數存在。 
設定 `ignoreUrlParams` 作為允許清單有助於修正此問題：

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
    }
```

現在，當Dispatcher看到請求時，會忽略請求具有 `query` 參數 `?` 參考並仍對頁面進行快取

<b>動態範例：</b>

- https://www.we-retail.com/search.html?q=fruit
- https://www.we-retail.com/search.html?q=vegetables

請記住，如果您的查詢參數會對頁面進行變更，並將其呈現為輸出，則您必須將其從忽略的清單中移除，並讓頁面重新取消可快取。  例如，使用查詢參數的搜尋頁面會變更呈現的原始html。

以下是每個搜尋的html來源：

`/search.html?q=fruit`:

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

`/search.html?q=vegetables`:

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

如果您造訪 `/search.html?q=fruit` 首先，它會快取顯示結果的html。

然後您造訪 `/search.html?q=vegetables` 第二，它將顯示出水果的結果。
這是因為 `q` 會忽略。  若要避免此問題，您必須記下根據查詢參數而產生不同HTML的頁面，並拒絕這些頁面的快取。

範例:

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
        /0002 { /glob "q" /type "deny"  }
    }
```

透過Javascript使用查詢參數的頁面仍會完全忽略此設定中的參數。  因為他們不會在休息時變更html檔案。  他們可使用javascript在本機瀏覽器上即時更新瀏覽器。  這表示如果您使用javascript查詢參數，就很可能會在頁面快取時忽略此參數。  允許該頁面快取，並享受效能提升！

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

要跟蹤這些頁面，確實需要一些維護，但值得獲得效能提升。  您可以要求CSE在您的網站上執行報表流量，提供過去90天內使用查詢參數的所有頁面清單，供您分析，並確定您知道要查看哪些頁面以及不要忽略哪些查詢參數
</div>
<br/>

## 快取回應標題

很明顯，Dispatcher會快取 `.html` 頁面和clientlib(即 `.js`, `.css`)，但您知道它也可以將特定回應標題與檔案中具有相同名稱但 `.h` 副檔名。 這不僅可讓內容的下一個回應，也可讓快取中隨附的回應標題。

AEM可處理不只是UTF-8編碼

有時，項目會有特殊標頭，可協助控制快取TTL的編碼詳細資訊，以及上次修改的時間戳記。

預設會移除快取時的這些值，而Apache httpd webserver將自行執行處理資產的工作，使用其一般的檔案處理方法，這類方法通常僅限根據副檔名進行mime類型猜測。

如果您有Dispatcher快取資產和所需的標題，則可以公開適當的體驗，並向用戶端瀏覽器保證所有詳細資訊。

以下是具有要指定快取之標題的伺服器陣列範例：

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


在範例中，CDN已設定AEM以提供CDN尋找的標題，以知道何時使其快取失效。 這表示AEM現在可以根據標題適當指定哪些檔案失效。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

請記住，您無法使用規則運算式或全域比對。 這是要快取的標題的常值清單。 只將要快取的常值標題清單中放入。
</div>


## 自動使寬限期無效

在AEM系統中，如果作者有大量活動，但執行了多次頁面啟動，您可能會遇到競爭條件，導致重複無效判定。 重複的排清請求是不必要的，您可以建置一些容限，在寬限期清除前不重複排清。

### 其運作方式的範例：

如果您有5個要使之無效的請求 `/content/exampleco/en/` 都發生在3秒內。

使用此功能，將快取目錄無效 `/content/exampleco/en/` 5次

若開啟此功能，且將設為5秒，將會使快取目錄失效 `/content/exampleco/en/` <b>once</b>

以下是此功能針對5秒寬限期所設定的範例語法：

```
/cache { 
    /gracePeriod "5"
```

## TTL型失效

Dispatcher模組的較新功能為 `Time To Live (TTL)` 快取項目的失效選項。 快取項目時，會尋找快取控制標題是否存在，並在快取目錄中產生同名的檔案，並 `.ttl` 擴充功能。

以下是伺服器陣列設定檔案中所設定功能的範例：

```
/cache { 
    /enableTTL "1"
```

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>
請記住，AEM仍需設定為傳送TTL標頭，Dispatcher才能執行。 切換此功能只會讓Dispatcher知道何時要移除AEM已傳送快取控制標題的檔案。 如果AEM未開始傳送TTL標頭，Dispatcher就不會在此執行任何特殊動作。
</div>

## 快取篩選規則

以下是要在發佈者上快取的元素之基線設定的範例：

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

我們希望讓已發佈的網站盡可能貪婪，並快取所有內容。

如果快取時有中斷體驗的元素，您可以新增規則以移除快取該項目的選項。 如上例所示，csrf權杖不應進行快取且已排除。 有關編寫這些規則的更多詳情，請參見 [此處](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

[Next ->使用和了解變數](./variables.md)