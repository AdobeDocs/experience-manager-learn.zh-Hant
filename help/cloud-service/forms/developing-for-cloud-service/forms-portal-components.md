---
title: 啟用AEM Forms Portal元件
description: 使用核心元件建置AEM Forms入口網站
solution: Experience Manager
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Core Components
jira: KT-10373
exl-id: ab01573a-e95f-4041-8ccf-16046d723aba
duration: 88
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '332'
ht-degree: 2%

---

# Forms Portal元件

AEM Forms提供下列立即可用的入口網站元件：

**搜尋與清單製作者**：此元件可讓您將表單存放庫中的表單清單至入口網站頁面，並提供設定選項以根據指定條件列出表單。

**草稿和提交**：雖然「搜尋並製表器」元件會顯示由Forms作者公開的表單，但「草稿並提交」元件會顯示儲存為草稿以供稍後完成並提交的表單。 此元件可為任何登入使用者提供個人化體驗。

**連結**：此元件可讓您在頁面上的任何位置建立表單的連結。

## 啟用Forms Portal元件

啟動IntelliJ，並開啟在 [先前的步驟。](./getting-started.md) 展開ui.apps->src->main->content->jcr_root->apps.bankingapplication->components

若要在Adobe Experience Manager (AEM)網站中使用任何核心元件（包括現成可用的入口網站元件），您必須建立Proxy元件並為您的網站啟用它。
新建立的Proxy元件需要指向現成可用的表單元件，以便繼承元件的一切。 變更Proxy元件content.xml中的resourceSuperType即可完成這項作業。 在content.xml中，我們也會指定標題和元件群組。
>[!NOTE]
>
> 您可以建構每個的資源超級型別 [從這裡取得這些元件](https://github.com/adobe/aem-core-forms-components/tree/master/ui.apps/src/main/content/jcr_root/apps/core/fd/components/formsportal)


### 草稿和提交

製作現有元件的副本(例如 `button`)，並將其命名為 _draftsandsubmissions_.
![draftsandsubmissions](assets/forms-portal-components2.png)
取代 `.content.xml` 使用下列XML：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Drafts And Submissions"
          sling:resourceSuperType="core/fd/components/formsportal/draftsandsubmissions/v1/draftsandsubmissions"
          componentGroup="BankingApplication - Content"/>
```

### 搜尋和列表器

製作按鈕元件的副本並將其重新命名為 _searchandlister_.
取代 `.content.xml` 使用下列XML：


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Search And Lister"
          sling:resourceSuperType="core/fd/components/formsportal/searchlister/v1/searchlister"
          componentGroup="BankingApplication - Content"/>
```

### 連結元件

製作按鈕元件的副本並將其重新命名為 _連結_.
取代 `.content.xml` 使用下列XML：


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Link to Adaptive Form"
          sling:resourceSuperType="core/fd/components/formsportal/link/v2/link"
          componentGroup="BankingApplication - Content"/>
```

部署專案後，您應該就能在AEM頁面中使用這些元件來建立Forms入口網站。

## 後續步驟

[包含雲端服務設定](./azure-storage-fdm.md)
