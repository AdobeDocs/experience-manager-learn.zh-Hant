---
title: 使用AEMas a Cloud Service快取頁面變體
description: 了解如何設定和使用AEM as a cloud service以支援快取頁面變體。
role: Architect, Developer
topic: Development
feature: CDN Cache, Dispatcher
exl-id: fdf62074-1a16-437b-b5dc-5fb4e11f1355
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '559'
ht-degree: 1%

---

# 快取頁面變體

了解如何設定和使用AEM as a cloud service以支援快取頁面變體。

## 範例使用案例

+ 任何根據使用者的地理位置以及含有動態內容之頁面的快取，提供不同服務組合和對應定價選項的服務提供者，都應在CDN和Dispatcher進行管理。

+ 零售客戶在全國各地都有商店，而每家商店都會根據其所在位置提供不同的優惠方案，且含有動態內容的頁面快取應在CDN和Dispatcher中管理。

## 解決方案概覽

+ 識別變體索引鍵及其可能具有的值數。 在我們的範例中，我們依美國各州而異，因此最大數為50。 這個小到不會造成CDN的變體限制問題。 [查看變型限制部分](#variant-limitations).

+ AEM程式碼必須設定cookie __&quot;x-aem-variant&quot;__ 訪客的偏好狀態(例如 `Set-Cookie: x-aem-variant=NY`)填入初始HTTP要求的對應HTTP回應。

+ 訪客的後續請求會傳送該Cookie(例如 `"Cookie: x-aem-variant=NY"`)，且Cookie會在CDN層級轉換為預先定義的標題(即 `x-aem-variant:NY`)，此資訊會傳遞至dispatcher。

+ Apache重寫規則會修改請求路徑，將頁首值加入頁面URL中，成為Apache Sling選擇器(例如 `/page.variant=NY.html`). 這可讓AEM Publish根據選取器來提供不同內容，而Dispatcher可針對每個變體快取一個頁面。

+ AEM Dispatcher傳送的回應必須包含HTTP回應標題 `Vary: x-aem-variant`. 這會指示CDN針對不同的標題值儲存不同的快取副本。

>[!TIP]
>
>每當Cookie設定時(例如 Set-Cookie:x-aem-variant=NY)回應不應可快取(應具有快取控制：專用或快取控制：無快取)

## HTTP要求流程

![變體快取請求流](./assets/variant-cache-request-flow.png)

>[!NOTE]
>
>以上的初始HTTP要求流程必須在請求使用變體的任何內容之前進行。

## 使用狀況

1. 為了演示此功能，我們將使用 [WKND](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)的實作範例。

1. 實作 [SlingServletFilter](https://sling.apache.org/documentation/the-sling-engine/filters.html) 在AEM中設定 `x-aem-variant` HTTP回應上的cookie，具有變體值。

1. AEM CDN自動轉換 `x-aem-variant` 將cookie移入相同名稱的HTTP標題中。

1. 將Apache Web伺服器mod_rewrite規則新增至 `dispatcher` 專案，此專案會修改請求路徑以包含變體選取器。

1. 使用Cloud Manager部署篩選和重寫規則。

1. 測試整體請求流程。

## 程式碼範例

+ 要設定的範例SlingServletFilter `x-aem-variant` cookie與AEM中的值。

   ```
   package com.adobe.aem.guides.wknd.core.servlets.filters;
   
   import javax.servlet.*;
   import java.io.IOException;
   
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.api.SlingHttpServletResponse;
   import org.apache.sling.servlets.annotations.SlingServletFilter;
   import org.apache.sling.servlets.annotations.SlingServletFilterScope;
   import org.osgi.service.component.annotations.Component;
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   
   
   // Invoke filter on  HTTP GET /content/wknd.*.foo|bar.html|json requests.
   // This code and scope is for example purposes only, and will not interfere with other requests.
   @Component
   @SlingServletFilter(scope = {SlingServletFilterScope.REQUEST},
           resourceTypes = {"cq:Page"},
           pattern = "/content/wknd/.*",
           extensions = {"html", "json"},
           methods = {"GET"})
   public class PageVariantFilter implements Filter {
       private static final Logger log = LoggerFactory.getLogger(PageVariantFilter.class);
       private static final String VARIANT_COOKIE_NAME = "x-aem-variant";
   
       @Override
       public void init(FilterConfig filterConfig) throws ServletException { }
   
       @Override
       public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
           SlingHttpServletResponse slingResponse = (SlingHttpServletResponse) servletResponse;
           SlingHttpServletRequest slingRequest = (SlingHttpServletRequest) servletRequest;
   
           // Check is the variant was previously set
           final String existingVariant = slingRequest.getCookie(VARIANT_COOKIE_NAME).getValue();
   
           if (existingVariant == null) {
               // Variant has not been set, so set it now
               String newVariant = "NY"; // Hard coding as an example, but should be a calculated value
               slingResponse.setHeader("Set-Cookie", VARIANT_COOKIE_NAME + "=" + newVariant + "; Path=/; HttpOnly; Secure; SameSite=Strict");
               log.debug("x-aem-variant cookie is set with the value {}", newVariant);
           } else {
               log.debug("x-aem-variant previously set with value {}", existingVariant);
           }
   
           filterChain.doFilter(servletRequest, slingResponse);
       }
   
       @Override
       public void destroy() { }
   }
   ```

+ 中的重寫規則範例 __dispatcher/src/conf.d/rewrite.rules__ 檔案，在Git中以原始碼管理，並使用Cloud Manager進行部署。

   ```
   ...
   
   RewriteCond %{REQUEST_URI} ^/us/.*  
   RewriteCond %{HTTP:x-aem-variant} ^.*$  
   RewriteRule ^([^?]+)\.(html.*)$ /content/wknd$1.variant=%{HTTP:x-aem-variant}.$2 [PT,L] 
   
   ...
   ```

## 變體限制

+ AEM CDN最多可管理200個變體。 這表示 `x-aem-variant` 標題最多可以有200個不重複值。 如需詳細資訊，請檢閱 [CDN設定限制](https://docs.fastly.com/en/guides/resource-limits).

+ 請務必小心，以確保您選擇的變體金鑰不會超過此數字。  例如，使用者ID不是好索引鍵，因為大部分網站都很容易超過200個值，而一個國家/地區若少於200個州，則較適合該國家/地區。

>[!NOTE]
>
>當變體超過200個時，CDN會以「太多變體」回應來回應，而非以頁面內容回應。
