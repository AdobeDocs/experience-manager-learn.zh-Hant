---
title: 調度程式對快取的理解
description: 瞭解Dispatcher模組如何操作其快取。
topic: Administration, Performance
version: 6.5
role: Admin
level: Beginner
thumbnail: xx.jpg
exl-id: 66ce0977-1b0d-4a63-a738-8a2021cf0bd5
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '1747'
ht-degree: 1%

---

# 瞭解快取

[目錄](./overview.md)

[&lt; — 上一個：配置檔案的說明](./explanation-config-files.md)

本文檔將說明Dispatcher快取的發生方式以及如何配置

## 快取目錄

我們在基線安裝中使用以下預設快取目錄

- 作者
   - `/mnt/var/www/author`
- 發佈者
   - `/mnt/var/www/html`

當每個請求遍歷Dispatcher時，請求將遵循配置的規則，以保留本地快取的版本以響應符合條件的項

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

我們故意將發佈的工作負載與作者工作負載分開，因為當Apache在DocumentRoot中查找檔案時，它不知道它來AEM自哪個實例。 因此，即使在作者群中禁用了快取，如果作者的DocumentRoot與發佈者相同，則當存在時，它將從快取中提供檔案。 這意味著您將從已發佈的快取中為作者提供檔案，並為您的訪問者提供非常糟糕的混合匹配體驗。

為不同的已發佈內容保留單獨的DocumentRoot目錄也是一個非常糟糕的主意。 您必須建立多個重新快取的項目，這些項目在客戶端等站點之間並無差異，並且必須為您設定的每個DocumentRoot設定複製刷新代理。 增加每次頁激活時頭上的刷新量。 依靠檔案的命名空間及其完整快取路徑，並避免發佈站點出現多個DocumentRoot。
</div>

## 配置檔案

Dispatcher控制在 `/cache {` 檔案欄。 
在AMS基線配置場中，您將發現我們的包括，如下所示：


```
/cache { 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
    }
```


建立要快取或不要快取的規則時，請參閱文檔 [這裡](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)


## 快取作者

我們看到許多實施中，人們不快取作者內容。 
他們在表現和對作者的反應方面都錯過了巨大的提升。

讓我們討論一下將作者群配置為正確快取時採用的策略。

這是一個基本作者 `/cache {` 作者群檔案部分：


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

這裡需要注意的是 `/docroot` 設定為作者的快取目錄。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

確保 `DocumentRoot` 在作者的 `.vhost` 檔案與場匹配 `/docroot` 參數
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

在作者方案中，內容會隨時而有意地發生更改。 您只希望快取不會經常更改的項目。
我們有規則要快取 `/libs` 因為它們是基準安裝的一AEM部分，在您安裝了Service Pack 、 Cumulative Fix Pack 、 Upgrade或Hotfix之前，它們會發生更改。 所以快取這些元素是意義重大的並且真正具有巨大的好處是作者對使用網站的最終用戶的體驗。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

請記住，這些規則還快取 <b>`/apps`</b> 這是自定義應用程式碼所在的位置。 如果您正在此實例上開發代碼，則保存檔案時會非常混亂，並且由於檔案提供了快取副本而看不到UI中是否有反映。 此處的意圖是，如果您也將代碼部署到該代碼AEM中，則不會頻繁，而部署步驟的一部分應是清除作者快取。 同樣，它的好處是讓您的可快取代碼能夠更快地為最終用戶運行。
</div>


## ServeOnStale(AKA Serve on Stale/SOS)

這是Dispatcher的一個特點。 如果發佈伺服器已載入或已無響應，則通常會拋出502或503 http響應代碼。 如果發生這種情況並啟用此功能，將指示Dispatcher仍為快取中的任何內容提供服務，以盡最大努力，即使它不是新副本。 如果您有功能，則最好提供服務，而不是只顯示不提供功能的錯誤消息。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

請記住，如果發佈者呈現器有套接字超時或500錯誤消息，則此功能將不會觸發。 如果AEM僅無法訪問，此功能將不執行任何操作
</div>

此設定可以在任何場中設定，但只有將其應用於發佈場檔案才有意義。 下面是在場檔案中啟用的功能的語法示例：

```
/cache { 
    /serveStaleOnError "1"
```

## 使用查詢參數/參數快取頁

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

Dispatcher模組的一種正常行為是，如果請求在URI中具有查詢參數（通常顯示如下） `/content/page.html?myquery=value`)將跳過快取檔案並直接轉到實AEM例。 它正在考慮此請求是動態頁，不應快取。 這會對快取效率造成不良影響。
</div>
<br/>

查看 [文章](https://github.com/adobe/aem-dispatcher-optimizer-tool/blob/main/docs/Rules.md#dot---the-dispatcher-publish-farm-cache-should-have-its-ignoreurlparams-rules-configured-in-an-allow-list-manner) 顯示查詢參數對站點效能的重要程度。

預設情況下，您要設定 `ignoreUrlParams` 規則允許 `*`。  意味著忽略所有查詢參數，並允許快取所有頁，而不考慮使用的參數。

這裡有一個示例，其中某人構建了社交媒體深度連結引用機制，該機制使用URI中的參數引用來瞭解此人來自何處。

<b>可忽略的示例：</b>

- https://www.we-retail.com/home.html?reference=android
- https://www.we-retail.com/home.html?reference=facebook

該頁面是100%可快取的，但不快取，因為存在參數。 
配置 `ignoreUrlParams` 作為允許清單，將有助於解決此問題：

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
    }
```

現在，當調度程式看到請求時，它將忽略請求具有 `query` 參數 `?` 引用並仍然快取頁面

<b>動態示例：</b>

- https://www.we-retail.com/search.html?q=fruit
- https://www.we-retail.com/search.html?q=vegetables

請記住，如果確實有使頁面更改的查詢參數將呈現輸出，則您需要將它們從忽略的清單中刪除，並使頁面再次取消可快取。  例如，使用查詢參數的搜索頁會更改呈現的原始html。

下面是每個搜索的html源：

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

如果您訪問 `/search.html?q=fruit` 首先，它會將html快取，並顯示結果。

然後你去 `/search.html?q=vegetables` 第二，它會顯示水果的效果。
這是因為 `q` 正在忽略快取。  要避免此問題，您需要注意根據查詢參數呈現不同HTML的頁面，並拒絕這些頁面的快取。

範例:

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
        /0002 { /glob "q" /type "deny"  }
    }
```

通過Javascript使用查詢參數的頁面仍將完全忽略此設定中的參數。  因為它們不會在靜止時更改html檔案。  它們使用javascript在本地瀏覽器上即時更新瀏覽器域。  這意味著如果使用Javascript查詢參數，很可能會忽略此參數進行頁面快取。  允許該頁快取並享受效能提升！

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

跟蹤這些頁面確實需要一些維護，但非常值得獲得效能提升。  您可以要求CSE在您的網站上運行報告流量，以向您提供一個清單，列出過去90天內使用查詢參數進行分析並確保您知道要查看哪些頁面以及哪些查詢參數不能忽略
</div>
<br/>

## 快取響應標頭

很明顯，調度程式快取 `.html` 頁和客戶端(即 `.js`。 `.css`)，但您是否知道，它還可以將特定響應標頭與同名檔案中的內容一起快取 `.h` 檔案副檔名。 這不僅允許對內容進行下一次響應，還允許從快取中對應隨之進行的響應標頭。

可AEM以處理不止UTF-8編碼

有時，項目具有特殊的標頭，可幫助控制快取TTL的編碼詳細資訊和上次修改的時間戳。

預設情況下，快取時這些值會被刪除，Apache httpd webserver將使用其常規的檔案處理方法來處理資產，這通常僅限於基於檔案副檔名的mime類型猜測。

如果Dispatcher快取了資產和所需的標頭，則您可以公開適當的體驗，並確保將所有詳細資訊都提供給客戶端瀏覽器。

下面是具有指定要快取的標頭的場示例：

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


在示例中，它們AEM已配置為提供CDN查找的報頭，以確定何時使其快取失效。 這意味著AEM現在可以根據報頭正確指示哪些檔案被無效。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

請記住，不能使用規則運算式或全局匹配。 它是要快取的標頭的文字清單。 只將文本標題清單放入您希望其快取。
</div>


## 自動失效寬限期

在AEM具有大量來自作者的活動且沒有頁面激活的系統上，您可以具有重複無效發生的競爭條件。 重複刷新請求是不必要的，您可以構建某種容限，在寬限期清除之前不重複刷新。

### 此操作的示例：

如果您有5個要求失效 `/content/exampleco/en/` 都在3秒內發生。

通過此功能，將使快取目錄無效 `/content/exampleco/en/` 5倍

啟用此功能並將其設定為5秒，將使快取目錄失效 `/content/exampleco/en/` <b>一次</b>

以下是為5秒寬限期配置此功能的示例語法：

```
/cache { 
    /gracePeriod "5"
```

## 基於TTL的無效

Dispatcher模組的一個較新功能是 `Time To Live (TTL)` 快取項的基於無效選項。 當項被快取時，它會查找是否存在快取控制標頭，並在快取目錄中生成具有相同名稱和 `.ttl` 擴展。

下面是在場配置檔案中配置的功能的示例：

```
/cache { 
    /enableTTL "1"
```

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注：</b>
請記住，仍AEM需要配置為發送TTL報頭，以便Dispatcher執行這些報頭。 切換此功能僅使Dispatcher知道何時刪除已發送快取控AEM制標頭的檔案。 如AEM果不開始發送TTL標頭，則Dispatcher將不會在此處執行任何特殊操作。
</div>

## 快取篩選器規則

下面是發佈伺服器上要快取哪些元素的基線配置示例：

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

我們希望使發佈的站點盡可能貪婪，並將所有內容都快取。

如果有元素在快取時中斷體驗，則可以添加規則以刪除用於快取該項的選項。 如上例所示，csrf令牌不應被快取並已被排除。 有關編寫這些規則的詳細資訊，請參閱 [這裡](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

[下一個 — >使用和瞭解變數](./variables.md)
