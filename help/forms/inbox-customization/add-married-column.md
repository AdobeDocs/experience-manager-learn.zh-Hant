---
title: 添加自定義列
description: 添加自定義列以顯示工作流的其他資料
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5.5
kt: 5830
topic: Development
role: Developer
level: Experienced
exl-id: 0b141b37-6041-4f87-bd50-dade8c0fee7d
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# 添加自定義列

要在收件箱中顯示工作流資料，我們需要定義並填充工作流中的變數。 需要在將任務分配給用戶之前設定變數的值。 為了讓您獲得先機，我們提供了準備部署在您的伺服器上的示例工AEM作流。

* [登錄到AEM](http://localhost:4502/crx/de/index.jsp)
* [導入審閱工作流](assets/review-workflow.zip)
* [查看工作流](http://localhost:4502/editor.html/conf/global/settings/workflow/models/reviewworkflow.html)

此工作流定義了兩個變數（isFonbed和income），其值使用設定變數元件進行設定。 這些變數將作為要添加到收件箱的列可用AEM

## 建立服務

對於需要在收件箱中顯示的每一列，我們都需要編寫一項服務。 以下服務允許我們添加一列來顯示isFringed變數的值

```java
import com.adobe.cq.inbox.ui.column.Column;
import com.adobe.cq.inbox.ui.column.provider.ColumnProvider;

import com.adobe.cq.inbox.ui.InboxItem;
import org.osgi.service.component.annotations.Component;

import java.util.Map;

/**
 * This provider does not require any sightly template to be defined.
 * It is used to display the value of 'ismarried' workflow variable as a column in inbox
 */
@Component(service = ColumnProvider.class, immediate = true)
public class MaritalStatusProvider implements ColumnProvider {@Override
public Column getColumn() {
return new Column("married", "Married", Boolean.class.getName());
}

// Return True or False if 'ismarried' is set. Else returns null
private Boolean isMarried(InboxItem inboxItem) {
Boolean ismarried = null;

Map metaDataMap = inboxItem.getWorkflowMetadata();
if (metaDataMap != null) {
if (metaDataMap.containsKey("isMarried")) {
    ismarried = (Boolean) metaDataMap.get("isMarried");
}
}

return ismarried;
}

@Override
public Object getValue(InboxItem inboxItem) {
return isMarried(inboxItem);
}
}
```

>[!NOTE]
>
>您需要將AEM6.5.5 Uber.jar納入您的項目，以便上述代碼能夠正常工作

![優步罐](assets/uber-jar.PNG)

## Test伺服器

* [登錄AEMWeb控制台](http://localhost:4502/system/console/bundles)
* [部署和啟動收件箱自定義包](assets/inboxcustomization.inboxcustomization.core-1.0-SNAPSHOT.jar)
* [開啟收件箱](http://localhost:4502/aem/inbox)
* 按一下以開啟管理控制 _清單視圖_ 表徵圖 _建立_ 按鈕
* 將「已婚」列添加到收件箱並保存更改
* [轉到FormsAndDocuments UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [導入示例表單](assets/snap-form.zip) 通過 _檔案上載_ 從 _建立_ 菜單
* [預覽窗體](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* 選擇 _婚姻狀況_ 並提交表格
   [查看收件箱](http://localhost:4502/aem/inbox)

提交表單將觸發工作流，並且任務已分配給「admin」用戶。 應在「已婚」列下看到一個值，如此螢幕抓圖中所示

![已婚人士](assets/married-column.PNG)
