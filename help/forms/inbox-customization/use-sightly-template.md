---
title: 使用美觀的模板顯示收件箱資料
description: 添加自定義列以使用美觀的模板顯示工作流的其他資料
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
kt: 5830
topic: Development
role: Developer
level: Experienced
exl-id: d09b46ed-3516-44cf-a616-4cb6e9dfdf41
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 0%

---

# 使用美觀的模板顯示收件箱資料

可以使用美觀的模板來格式化要顯示在收件箱列中的資料。 在本示例中，我們將根據收入列的值顯示coral-ui表徵圖。 以下螢幕快照顯示了收入列中表徵圖的使用
![收入表徵圖](assets/income-column.PNG)

[美觀的模板](assets/sightly-template.zip) 本文提供了用於顯示自定義coral ui表徵圖的部分。

## 美觀模板

下面是美觀的模板。 模板中的代碼將根據收入顯示表徵圖。 這些表徵圖可作為 [coral ui表徵圖](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html#availableIcons) 隨之而來AEM。

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

## 服務實施

以下代碼是顯示收入列的服務實現。

第12行將列與美觀模板關聯

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

## Test伺服器

>[!NOTE]
>
>本文假定您已安裝 [示例工作流](assets/review-workflow.zip) 和 [樣式](assets/snap-form.zip) 從 [前文](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/inbox-customization/add-married-column.html) 在此系列中。

* [以管理員用戶身份登錄crx](http://localhost:4502/crx/de/index.jsp)
* [導入美觀模板](assets/sightly-template.zip)
* [登錄AEMWeb控制台](http://localhost:4502/system/console/bundles)
* [部署和啟動收件箱自定義包](assets/income-column-customization.jar)
* [開啟收件箱](http://localhost:4502/aem/inbox)
* 通過按一下「建立」按鈕旁的「清單視圖」開啟管理控制項
* 將收入列添加到收件箱並保存更改
* [預覽窗體](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* 選擇 _婚姻狀況_ 並提交表格
* [查看收件箱](http://localhost:4502/aem/inbox)

提交表單將觸發工作流，並且任務已分配給「admin」用戶。 您應在收入列下看到相應的表徵圖
