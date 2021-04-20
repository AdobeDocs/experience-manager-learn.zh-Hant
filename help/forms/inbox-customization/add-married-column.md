---
title: 收件匣自訂
description: 新增自訂欄以顯示其他工作流程資料
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
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 1%

---


# 新增自訂欄

若要在收件匣中顯示工作流程資料，我們需要在工作流程中定義並填入變數。 必須將變數值設定後，才能將任務指派給使用者。 為了搶先一步，我們提供了可部署在您伺服器上的範例工作AEM流程。

* [登入AEM至](http://localhost:4502/crx/de/index.jsp)
* [匯入審核工作流程](assets/review-workflow.zip)
* [檢閱工作流程](http://localhost:4502/editor.html/conf/global/settings/workflow/models/reviewworkflow.html)

此工作流定義了兩個變數（isBonded和income），其值是使用set變數元件設定的。 這些變數將作為要添加到收件箱的列AEM可用

## 建立服務

對於每個需要顯示在收件箱中的列，我們需要編寫服務。 下列服務可讓我們新增欄來顯示isBringed變數的值

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
>您必須將AEM6.5.5 Uber.jar加入專案中，才能使用上述程式碼

![uber-jar](assets/uber-jar.PNG)

## 在伺服器上測試

* [登入網AEM頁主控台](http://localhost:4502/system/console/bundles)
* [部署和啟動收件箱自定義包](assets/inboxcustomization.inboxcustomization.core-1.0-SNAPSHOT.jar)
* [開啟您的收件匣](http://localhost:4502/aem/inbox)
* 按一下&#x200B;_建立_&#x200B;按鈕旁的&#x200B;_清單檢視_&#x200B;圖示，以開啟管理控制項
* 將「已婚」欄新增至「收件匣」並儲存您的變更
* [前往FormsAndDocuments UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [選擇「檔案」(File)「從創](assets/snap-form.zip) 建菜單上載」( _Uploadfrom_ Createmenu)，導入示例 __ 格式
* [預覽表格](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* 選擇&#x200B;_婚姻狀態_並提交表單
   [查看收件匣](http://localhost:4502/aem/inbox)

提交表單會觸發工作流程，並指派工作給「管理員」使用者。 您應該會在「已婚」欄下看到值，如此螢幕擷取畫面中所示

![已婚人士](assets/married-column.PNG)
