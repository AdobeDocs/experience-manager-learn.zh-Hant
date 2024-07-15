---
title: 使用Sightly範本顯示收件匣資料
description: 新增自訂欄以顯示使用Sightly範本的工作流程其他資料
feature: Adaptive Forms
doc-type: article
version: 6.5
jira: KT-5830
topic: Development
role: Developer
level: Experienced
exl-id: d09b46ed-3516-44cf-a616-4cb6e9dfdf41
duration: 68
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---

# 使用Sightly範本顯示收件匣資料

您可以使用Sightly範本來格式化要顯示在收件匣欄中的資料。 在此範例中，我們將根據收入欄的值顯示coral-ui圖示。 下列熒幕擷圖顯示收入欄中的圖示使用
![收入圖示](assets/income-column.PNG)

[本文中提供了用於顯示自訂coral ui圖示的sightly範本](assets/sightly-template.zip)。

## Sightly範本

以下是sightly範本。 樣版中的程式碼會根據收入顯示圖示。 這些圖示是AEM隨附的[coral ui icon library](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html#availableIcons)的一部分。

```java
<template data-sly-template.incomeTemplate="${@ item}>">
    <td is="coral-table-cell" class="payload-income-cell">
         <div data-sly-test="${(item.workflowMetadata && item.workflowMetadata.income)}" data-sly-set.income ="${item.workflowMetadata.income}">
                 <coral-icon icon="confidenceOne" size="M" data-sly-test="${income >=0 && income <10000}"></coral-icon>
                 <coral-icon icon="confidenceTwo" size="M" data-sly-test="${income >=10000 && income <100000}"></coral-icon>
                 <coral-icon icon="confidenceThree" size="M" data-sly-test="${income >=100000 && income <500000}"></coral-icon>
                 <coral-icon icon="confidenceFour" size="M" data-sly-test="${income >=500000}"></coral-icon>
          </div>
    </td>
</template>
```

## 服務實作

下列程式碼是顯示收入欄的服務實作。

第12行將欄與sightly範本建立關聯

```java
import java.util.Map;
import org.osgi.service.component.annotations.Component;
import com.adobe.cq.inbox.ui.InboxItem;
import com.adobe.cq.inbox.ui.column.Column;
import com.adobe.cq.inbox.ui.column.provider.ColumnProvider;

@Component(service = ColumnProvider.class, immediate = true)
public class IncomeProvider implements ColumnProvider {
@Override
public Column getColumn() {

return new Column("income", "Income", String.class.getName(),"inbox/customization/column-templates.html", "incomeTemplate");
}

@Override
public Object getValue(InboxItem inboxItem) {
Object val = null;

Map workflowMetadata = inboxItem.getWorkflowMetadata();

if (workflowMetadata != null && workflowMetadata.containsKey("income"))
    val = workflowMetadata.get("income");

return val;
}
}
```

## 在您的伺服器上測試

>[!NOTE]
>
>本文假設您已安裝此系列中[先前文章](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/inbox-customization/add-married-column.html)的[範例工作流程](assets/review-workflow.zip)和[範例表單](assets/snap-form.zip)。

* [以管理員使用者身分登入crx](http://localhost:4502/crx/de/index.jsp)
* [匯入sightly範本](assets/sightly-template.zip)
* [登入AEM網頁主控台](http://localhost:4502/system/console/bundles)
* [部署和啟動收件匣自訂套裝](assets/income-column-customization.jar)
* [開啟您的收件匣](http://localhost:4502/aem/inbox)
* 按一下「建立」按鈕旁的「清單檢視」以開啟「管理員控制項」
* 新增收入欄至收件匣並儲存變更
* [預覽表單](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* 選取&#x200B;_婚姻狀況_&#x200B;並提交表單
* [檢視收件匣](http://localhost:4502/aem/inbox)

提交表單會觸發工作流程，且任務會指派給「管理員」使用者。 您應該會在收入欄下方看到適當的圖示
