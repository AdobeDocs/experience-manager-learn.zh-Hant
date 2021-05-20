---
title: 設定AEM for SPA Editor和遠端SPA
description: 需要AEM專案來設定支援的設定和內容需求，以允許AEM SPA編輯器製作遠端SPA。
topic: 無頭、SPA、開發
feature: SPA編輯器，核心元件， API，開發
role: Developer, Architect
level: Beginner
kt: 7631
thumbnail: kt-7631.jpeg
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '1220'
ht-degree: 1%

---


# 設定AEM for SPA Editor

雖然SPA程式碼基底是在AEM外部管理，但需要AEM專案才能設定支援的設定和內容需求。 本章逐步說明包含必要設定之AEM專案的建立：

+ AEM WCM核心元件Proxy
+ AEM遠端SPA頁面代理
+ AEM遠端SPA頁面範本
+ 基線遠端SPA AEM頁面
+ 定義SPA至AEM URL對應的子專案
+ OSGi配置資料夾

## 建立AEM專案

建立AEM專案，以管理設定和基準內容。

_請一律使用最新版 [AEM原型](https://github.com/adobe/aem-project-archetype)。_


```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ mvn -B archetype:generate \
 -D archetypeGroupId=com.adobe.aem \
 -D archetypeArtifactId=aem-project-archetype \
 -D archetypeVersion=27 \
 -D aemVersion=cloud \
 -D appTitle="WKND App" \
 -D appId="wknd-app" \
 -D groupId="com.adobe.aem.guides.wkndapp" \
 -D frontendModule="react"
$ mv ~/Code/wknd-app/wknd-app ~/Code/wknd-app/com.adobe.aem.guides.wknd-app
```

_最後一個命令只需重新命名AEM專案資料夾，如此即可清楚知道這是AEM專案，且不與遠端SPA混淆__

指定`frontendModule="react"`時，`ui.frontend`專案不會用於遠端SPA使用案例。 SPA是從外部開發及管理，僅使用AEM作為內容API。 項目包含`spa-project` AEM Java™相依性並設定遠程SPA頁面模板時需要`frontendModule="react"`標誌。

AEM專案原型會產生下列元素，用以設定AEM以與SPA整合。

+ __AEM WCM核心元件代理程__ 式  `ui.content/src/.../apps/wknd-app/components`
+ __AEM SPA遠端頁面__ 代碼  `ui.content/src/.../apps/wknd-app/components/remotepage`
+ __AEM頁面范__ 本  `ui.content/src/.../conf/wknd-app/settings/wcm/templates`
+ __定義內容映射的子項__ 目  `ui.content/src/...`
+ __基線遠端SPA AEM頁__ 面  `ui.content/src/.../content/wknd-app`
+ __OSGi配置資__ 料夾  `ui.config/src/.../apps/wknd-app/osgiconfig`

產生基本AEM專案後，進行一些調整以確保SPA Editor與Remote SPA相容。

## 移除ui.frontend專案

由於SPA是遠端SPA，因此假設它是在AEM專案外部開發及管理。 要避免衝突，請刪除`ui.frontend`項目，使其不進行部署。 如果未移除`ui.frontend`專案，則會在AEM SPA編輯器中同時載入`ui.frontend`專案和遠端SPA中提供的兩個SPA(預設SPA)。

1. 在IDE中開啟AEM專案(`~/Code/wknd-app/com.adobe.aem.guides.wknd-app`)
1. 開啟根`pom.xml`
1. 從`<modules>`清單中注釋`<module>ui.frontend</module`

   ```
   <modules>
       <module>all</module>
       <module>core</module>
   
       <!-- <module>ui.frontend</module> -->
   
       <module>ui.apps</module>
       <module>ui.apps.structure</module>
       <module>ui.config</module>
       <module>ui.content</module>
       <module>it.tests</module>
       <module>dispatcher</module>
       <module>ui.tests</module>
       <module>analyse</module>
   </modules>
   ```

   `pom.xml`檔案應該如下所示：

   ![從reactor pom中移除ui.frontend模組](./assets/aem-project/uifrontend-reactor-pom.png)

1. 開啟`ui.apps/pom.xml`
1. 注釋`<artifactId>wknd-app.ui.frontend</artifactId>`上的`<dependency>`

   ```
   <dependencies>
   
       <!-- Remote SPA project will provide all frontend resources
       <dependency>
           <groupId>com.adobe.aem.guides.wkndapp</groupId>
           <artifactId>wknd-app.ui.frontend</artifactId>
           <version>${project.version}</version>
           <type>zip</type>
       </dependency>
       --> 
   </dependencies>
   ```

   `ui.apps/pom.xml`檔案應該如下所示：

   ![從ui.apps移除ui.frontend相依性](./assets/aem-project/uifrontend-uiapps-pom.png)

如果AEM專案是在這些變更前建置的，請從`ui.apps/src/main/content/jcr_root/apps/wknd-app/clientlibs/clientlib-react`的`ui.apps`專案手動刪除`ui.frontend`產生的用戶端程式庫。

## AEM內容對應

若要讓AEM在SPA編輯器中載入遠端SPA，必須建立SPA路由與用於開啟和製作內容的AEM頁面之間的對應。

稍後將探討此設定的重要性。

對應可以使用[`/etc/map`中定義的](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html#root-level-mappings-1) Sling對應完成。

1. 在IDE中，開啟`ui.content`子項目
1. 導航到  `src/main/content/jcr_root/etc`
1. 建立資料夾`map`
1. 在`map`中，建立資料夾`http`
1. 在`http`中，建立包含以下內容的檔案`.content.xml`:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping">
       <localhost_any/>
   </jcr:root>
   ```

1. 在`http`中，建立資料夾`localhost_any`
1. 在`localhost_any`中，建立包含以下內容的檔案`.content.xml`:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="localhost\\.\\d+">
       <wknd-app-routes-adventure/>
   </jcr:root>
   ```

1. 在`localhost_any`中，建立資料夾`wknd-app-routes-adventure`
1. 在`wknd-app-routes-adventure`中，建立包含以下內容的檔案`.content.xml`:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   
   <!--
   The 'wknd-app-routes-adventure' mapping, maps requests to the SPA's adventure route 
   to it's corresponding page in AEM at /content/wknd-app/us/en/home/adventure/xxx.
   
   Note the adventure AEM pages will be created directly in AEM.
   -->
   
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="adventure:.*/([^/]+)/?$"
       sling:internalRedirect="/content/wknd-app/us/en/home/adventure/$1"/>
   ```

1. 將映射節點添加到`ui.content/src/main/content/META-INF/vault/filter.xml`中，使其包含在AEM包中。

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <workspaceFilter version="1.0">
       <filter root="/conf/wknd-app" mode="merge"/>
       <filter root="/content/wknd-app" mode="merge"/>
       <filter root="/content/dam/wknd-app/asset.jpg" mode="merge"/>
       <filter root="/content/experience-fragments/wknd-app" mode="merge"/>
   
       <!-- Add the Sling Mapping rules for the WKND App -->
       <filter root="/etc/map" mode="merge"/>
   </workspaceFilter>
   ```

資料夾結構和`.context.xml`檔案應如下所示：

![Sling對應](./assets/aem-project/sling-mapping.png)

`filter.xml`檔案應該如下所示：

![Sling對應](./assets/aem-project/sling-mapping-filter.png)

現在，部署AEM專案時，會自動加入這些設定。

Sling對應會影響在`http`和`localhost`上執行的AEM，因此僅支援本機開發。 部署至AEM as aCloud Service時，必須新增類似的Sling對應，將目標`https`和適當的AEM作為Cloud Service網域。 如需詳細資訊，請參閱[Sling對應檔案](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html)。

## 跨原始資源共用安全性原則

接下來，設定AEM以保護內容，讓只有此SPA可以存取AEM內容。 C在AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/develop-for-cross-origin-resource-sharing.html)中設定[跨原始資源共用。

1. 在IDE中，開啟`ui.config` Maven子項目
1. 導覽 `src/main/content/jcr_root/apps/wknd-app/osgiconfig/config`
1. 建立名為`com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json`的檔案
1. 將下列項目新增至檔案：

   ```
   {
       "supportscredentials":true,
       "exposedheaders":[
           ""
       ],
       "supportedmethods":[
           "GET",
           "HEAD",
           "POST",
           "OPTIONS"
       ],
       "alloworigin":[
           "https://external-hosted-app", "localhost:3000"
       ],
       "maxage:Integer":1800,
       "alloworiginregexp":[
           ".*"
       ],
       "allowedpaths":[
           ".*"
       ],
       "supportedheaders":[
           "Origin",
           "Accept",
           "X-Requested-With",
           "Content-Type",
           "Access-Control-Request-Method",
           "Access-Control-Request-Headers",
           "Authorization"
       ]
   }
   ```

`com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json`檔案應該如下所示：

![SPA編輯器CORS設定](./assets/aem-project/cors-configuration.png)

關鍵設定元素為：

+ `alloworigin` 指定允許哪些主機從AEM擷取內容。
   + `localhost:3000` 新增以支援SPA在本機執行
   + `https://external-hosted-app` 作為預留位置，以Remote SPA托管的網域取代。
+ `allowedpaths` 指定此CORS設定涵蓋的AEM路徑。預設允許存取AEM中的所有內容，但此值只能限定為SPA可存取的特定路徑，例如：`/content/wknd-app`。

## 將AEM頁面設為遠端SPA頁面範本

AEM專案原型會產生專案，以便與遠端SPA整合，但需要對自動產生的AEM頁面結構進行小幅但重要的調整。 自動產生的AEM頁面必須將其類型變更為&#x200B;__遠端SPA頁面__，而非&#x200B;__SPA頁面__。

1. 在IDE中，開啟`ui.content`子項目
1. 開啟至`src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml`
1. 使用以下內容更新此`.content.xml`檔案：

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
           jcr:primaryType="cq:Page">
       <jcr:content
           cq:template="/conf/wknd-app/settings/wcm/templates/spa-remote-page"
           jcr:primaryType="cq:PageContent"
           jcr:title="WKND App Home Page"
           sling:resourceType="wknd-app/components/remotepage">
           <root
               jcr:primaryType="nt:unstructured"
               sling:resourceType="wcm/foundation/components/responsivegrid">
               <responsivegrid
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="wcm/foundation/components/responsivegrid">
                   <text
                       jcr:primaryType="nt:unstructured"
                       sling:resourceType="wknd-app/components/text"
                       text="&lt;p>Hello World!&lt;/p>"
                       textIsRich="true">
                       <cq:responsive jcr:primaryType="nt:unstructured"/>
                   </text>
               </responsivegrid>
           </root>
       </jcr:content>
   </jcr:root>
   ```

關鍵更改是對`jcr:content`節點的更新：

+ `cq:template` 至 `/conf/wknd-app/settings/wcm/templates/spa-remote-page`
+ `sling:resourceType` 至 `wknd-app/components/remotepage`

`src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml`檔案應該如下所示：

![首頁.content.xml更新](./assets/aem-project/home-content-xml.png)

這些變更可讓此頁面(作為AEM中的SPA根)在SPA Editor中載入遠端SPA。

>[!NOTE]
>
>如果此專案先前是AEM專案，請務必將AEM頁面刪除為&#x200B;__Sites > WKND應用程式> us > en > WKND應用程式首頁__，因為`ui.content`專案已設為&#x200B;__merge__&#x200B;節點，而非&#x200B;__update__。

此頁面也可以移除，並重新建立為AEM本身的遠端SPA頁面，但由於此頁面是在`ui.content`專案中自動建立，因此最好在程式碼基底中更新它。

## 將AEM專案部署至AEM SDK

1. 確認AEM製作服務在連接埠4502上執行
1. 從命令列，導覽至AEM Maven專案的根目錄
1. 使用Maven將專案部署至本機AEM SDK製作服務

   ```
   $ mvn clean install -PautoInstallSinglePackage
   ```

   ![mvn清潔安裝 — PautoInstallSinglePackage](./assets/aem-project/mvn-install.png)

## 設定根AEM頁面

部署AEM專案後，準備SPA Editor以載入遠端SPA的最後一步。 在AEM中，標示與AEM專案原型產生之SPA根`/content/wknd-app/us/en/home`對應的AEM頁面。

1. 登入AEM作者
1. 導覽至「__Sites > WKND應用程式> us > en__」
1. 選擇&#x200B;__WKND應用首頁__，然後點選&#x200B;__屬性__

   ![WKND應用首頁 — 屬性](./assets/aem-content/edit-home-properties.png)

1. 導覽至&#x200B;__SPA__&#x200B;標籤
1. 填寫&#x200B;__遠程SPA配置__
   + __SPA主機URL__:  `http://localhost:3000`
      + 遠端SPA的根目錄URL

   ![WKND應用首頁 — 遠程SPA配置](./assets/aem-content/remote-spa-configuration.png)

1. 點選&#x200B;__儲存並關閉__

請記住，我們將此頁面的類型變更為&#x200B;__遠端SPA頁面__&#x200B;的類型，這可讓我們在其&#x200B;__頁面屬性__&#x200B;中查看&#x200B;__SPA__&#x200B;標籤。

只有在AEM頁面上設定此設定，且此頁面與SPA的根目錄對應。 此頁面下方的所有AEM頁面都會繼承值。

## 恭喜

您現在已準備AEM設定，並部署至本機AEM作者！ 您現在知道如何：

+ 移除AEM專案原型產生的SPA，方法是在`ui.frontend`中註解相依性
+ 將SPA路由對應至AEM中資源的Sling對應新增至AEM
+ 設定AEM跨原始資源共用安全性原則，讓遠端SPA從AEM使用內容
+ 將AEM專案部署至本機AEM SDK製作服務
+ 使用AEM主機URL頁面屬性，將SPA頁面標示為遠端SPA根

## 後續步驟

在配置AEM後，我們可以專注於使用AEM SPA編輯器引導遠程SPA](./spa-bootstrap.md)並支援可編輯區域！[