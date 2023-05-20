---
title: 無AEM頭移動部署
description: 瞭解移動無頭部署的AEM部署注意事項。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10796
thumbnail: KT-10796.jpg
exl-id: 1f536079-b3ce-4807-be88-804378e75d37
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 2%

---

# 無AEM頭移動部署

無AEM頭移動部署是iOS、Android™等的本機移動應用。 以無頭方式消費和AEM與內容交互。

移動部署需要最少的配置，AEM因為到無頭API的HTTP連接不是在瀏覽器上下文中啟動的。

## 部署配置

以下部署配置必須就地用於移動應用部署。

| 移動應用連接到 | AEM 作者 | AEM 發佈 | 預AEM覽 |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [調度器篩選器](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| 跨源資源共用(CORS) | ✘ | ✘ | ✘ |
| [AEM主機](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## 移動應用示例

Adobe提供了iOS和Android™移動應用示例。

<div class="columns is-multiline">
    <!-- iOS app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="iOS app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/ios-swiftui-app.md" title="iOS應用" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/ios-swiftui-app/ios-app-card.png" alt="iOS應用">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/ios-swiftui-app.md" title="iOS應用">iOS應用</a></p>
                   <p class="is-size-6">用SwiftUI編寫的iOS應用程式示例，它使用來自無AEM頭GraphQLAPI的內容。</p>
                   <a href="../example-apps/ios-swiftui-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">查看示例</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
    <!-- Android app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Android app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/android-app.md" title="Android™應用" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/android-java-app/android-app-card.png" alt="Android應用">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/android-app.md" title="Android™應用">Android™應用</a></p>
                   <p class="is-size-6">Java™ Android™應用程式示例，它消耗來自無頭GraphQLAPIAEM的內容。</p>
                   <a href="../example-apps/android-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">查看示例</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
