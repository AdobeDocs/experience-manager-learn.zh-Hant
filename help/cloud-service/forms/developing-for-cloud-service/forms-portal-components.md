---
title: 啟用AEM Forms門戶元件
description: 使用核心元件構建AEM Forms門戶
solution: Experience Manager
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 10373
exl-id: ab01573a-e95f-4041-8ccf-16046d723aba
source-git-commit: 69cd5022d136e9fa84f33d2fc5ca249ac0fb6490
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 0%

---

# Forms門戶元件

AEM Forms提供以下現成的門戶元件：

**搜索與李斯特**:此元件允許您將表單從表單儲存庫列到門戶頁面，並提供了配置選項，以根據指定的條件列出表單。

**草稿和提交**:Search &amp; Lister元件顯示由Forms作者公開的表單，而Drafts &amp; Submissions元件則顯示保存為草稿的表單，以供日後完成和提交的表單。 此元件為任何登錄用戶提供個性化體驗。

**連結**:此元件允許您建立指向頁面上任意位置的表單的連結。

## 啟用Forms門戶元件

啟動IntelliJ並開啟在 [前一步。](./getting-started.md) 展開ui.apps->src->main->content->jcr_root->apps.bankingapplication->元件

要在Adobe Experience Manager(AEM)站點中使用任何核心元件（包括現成門戶元件），必須建立代理元件並為您的站點啟用它。
新建立的代理元件需要指向現成的表單元件，以便它們從這些元件繼承所有內容。 通過更改代理元件content.xml中的resourceSuperType來完成此操作。 在content.xml中，我們還指定標題和元件組。
>[!NOTE]
>
> 可以為每個資源構造資源超類型 [這些元件](https://github.com/adobe/aem-core-forms-components/tree/master/ui.apps/src/main/content/jcr_root/apps/core/fd/components/formsportal)


### 草稿和提交

複製現有元件(例如 `button`)，將其命名為 _草案和提交_。
![草案和提交](assets/forms-portal-components2.png)
替換 `.content.xml` XML中的所有內容：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Drafts And Submissions"
          sling:resourceSuperType="core/fd/components/formsportal/draftsandsubmissions/v1/draftsandsubmissions"
          componentGroup="BankingApplication - Content"/>
```

### 搜索和李斯特

建立按鈕元件的副本並將其更名為 _塞勒斯特_。
替換 `.content.xml` XML中的所有內容：


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Search And Lister"
          sling:resourceSuperType="core/fd/components/formsportal/searchlister/v1/searchlister"
          componentGroup="BankingApplication - Content"/>
```

### 連結元件

建立按鈕元件的副本並將其更名為 _連結_。
替換 `.content.xml` XML中的所有內容：


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Link to Adaptive Form"
          sling:resourceSuperType="core/fd/components/formsportal/link/v2/link"
          componentGroup="BankingApplication - Content"/>
```

部署項目後，您應該能夠使用頁面中的這些組AEM件建立Forms門戶。
