---
title: CSRF保護
description: 瞭解如何為已驗證身分的使用者產生並新增AEM CSRF Token以允許POST、PUT和刪除至AEM的請求。
version: Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
doc-type: Code Sample
last-substantial-update: 2023-07-14T00:00:00Z
jira: KT-13651
thumbnail: KT-13651.jpeg
source-git-commit: 9cd2c5b6337ae7a4b05380d6a3fb945f28a14299
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 0%

---


# CSRF保護

瞭解如何為已驗證身分的使用者產生並新增AEM CSRF Token以允許POST、PUT和刪除至AEM的請求。

AEM需要傳送有效的CSRF權杖 __已驗證__ __POST__、__PUT或 __DELETE__ 向AEM Author和Publish服務發出HTTP請求。

CSRF權杖不需要 __GET__ 請求，或 __匿名__ 要求。

如果CSRF權杖未隨POST、PUT或DELETE請求傳送，AEM會傳回403禁止回應，而AEM會記錄下列錯誤：

```log
[INFO][POST path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] isValidRequest: empty CSRF token - rejecting
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] doFilter: the provided CSRF token is invalid
```

請參閱 [檔案，以取得有關AEM CSRF保護的更多詳細資料](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html).


## CSRF使用者端資源庫

AEM提供使用者端程式庫，可用來產生和新增CSRF權杖XHR，並透過修補核心原型功能形成POST請求。 功能由 `granite.csrf.standalone` 使用者端資料庫類別。

若要使用此方法，請新增 `granite.csrf.standalone` 作為在您的頁面上載入的使用者端程式庫的相依性。 例如，如果您使用 `wknd.site` 使用者端資料庫類別，新增 `granite.csrf.standalone` 作為在您的頁面上載入的使用者端程式庫的相依性。

## 具有CSRF保護的自訂表單提交

如果使用 [`granite.csrf.standalone` 使用者端資源庫](#csrf-client-library) 不適用於您的使用案例，您可以手動新增CSRF代號至表單提交。 以下範例說明如何將CSRF權杖新增至表單提交。

此程式碼片段示範如何在表單提交時，從AEM擷取CSRF權杖，並將其新增至名為的表單輸入 `:cq_csrf_token`. 由於CSRF權杖的生命週期短，因此最好在提交表單前立即擷取和設定CSRF權杖，以確保其有效性。

```javascript
// Attach submit handler event to form onSubmit
document.querySelector('form').addEventListener('submit', (e) => {
    e.preventDefault();

    const form = e.target;
    const response = await fetch('/libs/granite/csrf/token.json');
    const json = await response.json();
    
    // Create a form input named ``:cq_csrf_token`` with the CSRF token.
    let csrfTokenInput = form.querySelector('input[name=":cq_csrf_token"]');
    if (!csrfTokenInput?.value) {
        // If the form does not have a CSRF token input, add one.
        form.insertAdjacentHTML('afterbegin', `<input type="hidden" name=":cq_csrf_token" value="${json.token}">`);
    } else {
        // If the form already has a CSRF token input, update the value.
        csrfTokenInput.value = json.token;
    }

    form.submit();
});
```

## 以CSRF保護擷取

如果使用 [`granite.csrf.standalone` 使用者端資源庫](#csrf-client-library) 不適用於您的使用案例，您可以手動將CSRF權杖新增至XHR或擷取請求。 以下範例說明如何將CSRF權杖新增至使用擷取建立的XHR。

此程式碼片段示範如何從AEM擷取CSRF權杖，並將其新增至擷取請求的 `CSRF-Token` HTTP要求標頭。 由於CSRF權杖的生命週期短，因此最好在發出擷取請求之前立即擷取和設定CSRF權杖，以確保其有效性。

```javascript
/**
 * Get CSRF token from AEM.
 * CSRF token expire after a few minutes, so it is best to get the token before each request.
 * 
 * @returns {Promise<string>} that resolves to the CSRF token.
 */
async getCsrfToken() {
    const response = await fetch('/libs/granite/csrf/token.json');
    const json = await response.json();
    return json.token;
}
...
// Fetch from AEM with CSRF token in a header named 'CSRF-Token'.
await fetch('/path/to/aem/endpoint', {
    method: 'POST',
    headers: {
        'CSRF-Token': await getCsrfToken()
    },
});
```

## Dispatcher 設定

在AEM Publish服務上使用CSRF權杖時，必須更新Dispatcher設定，以允許對CSRF權杖端點提出GET請求。 以下設定允許向AEM Publish服務上的CSRF權杖端點發出GET請求。 如果未新增此設定，CSRF權杖端點會傳回404 Not Found回應。

* `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
/0120 { /type "allow" /method "GET" /url "/libs/granite/csrf/token.json" }
...
```