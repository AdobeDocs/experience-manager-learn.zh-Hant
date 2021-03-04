---
title: 收件匣自訂
description: 新增自訂欄，以使用Sightly範本顯示其他工作流程資料
feature: 適用性表單
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5.5
kt: 5830
topic: 開發
role: 開發人員
level: 經驗豐富
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 1%

---

# 使用Sightly範本顯示收件匣資料

您可以使用Sightly範本來格式化要顯示在收件匣欄中的資料。 在此範例中，我們會根據收入欄的值來顯示coral-ui圖示。 以下螢幕抓圖顯示收入列中表徵圖的使用
![income-icons](assets/income-column.PNG)

[本文](assets/sightly-template.zip) 中提供用來顯示自訂珊瑚UI圖示的美觀範本。

## Sightly範本

Following is the sightly template. 範本中的程式碼會根據收入顯示圖示。 這些圖示是隨附於[coral ui圖示庫](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html#availableIcons)的一部分AEM。

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

以下代碼是用於顯示收入列的服務實施。

第12行將欄與Sightly範本建立關聯

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

## 在伺服器上測試

>[!NOTE]
>
>本文假設您已安裝本系列中[先前文章](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/inbox-customization/add-married-column.md)的[範例工作流程](assets/review-workflow.zip)和[範例表單](assets/snap-form.zip)。

* [以管理員使用者身分登入crx](http://localhost:4502/crx/de/index.jsp)
* [匯入高雅的範本](assets/sightly-template.zip)
* [登入網AEM頁主控台](http://localhost:4502/system/console/bundles)
* [部署和啟動收件箱自定義包](assets/income-column-customization.jar)
* [開啟您的收件匣](http://localhost:4502/aem/inbox)
* 按一下「建立」按鈕旁的「清單檢視」，以開啟「管理控制項」
* 將收入列添加到收件箱並保存您所做的更改
* [預覽表格](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* 選擇&#x200B;_婚姻狀態_&#x200B;並提交表單
* [查看收件箱](http://localhost:4502/aem/inbox)

提交表單會觸發工作流程，並指派工作給「管理員」使用者。 您應該會在收入欄下看到適當的圖示
