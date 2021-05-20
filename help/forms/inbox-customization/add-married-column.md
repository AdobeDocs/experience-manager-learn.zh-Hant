---
title: 收件匣自訂
description: 新增自訂欄以顯示其他工作流程資料
feature: 適用性表單
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5.5
kt: 5830
topic: 開發
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '309'
ht-degree: 0%

---


# 新增自訂欄

若要在收件匣中顯示工作流程資料，我們需要定義並填入工作流程中的變數。 必須先設定變數的值，才能將任務指派給使用者。 為了搶先一步，我們提供了可部署在AEM伺服器上的範例工作流程。

* [登入AEM](http://localhost:4502/crx/de/index.jsp)
* [匯入審核工作流程](assets/review-workflow.zip)
* [檢閱工作流程](http://localhost:4502/editor.html/conf/global/settings/workflow/models/reviewworkflow.html)

此工作流程有兩個已定義的變數（isFribed和income），其值是使用設定的變數元件來設定。 這些變數將可作為欄供新增至AEM收件匣

## 建立服務

對於需要在收件箱中顯示的每一列，我們都需要編寫服務。 下列服務可讓我們新增欄，以顯示isFribed變數的值

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
>您必須在專案中納入AEM 6.5.5 Uber.jar，前述程式碼才能運作

![uber-jar](assets/uber-jar.PNG)

## 在伺服器上測試

* [登入AEM Web Console](http://localhost:4502/system/console/bundles)
* [部署和啟動收件箱自定義包](assets/inboxcustomization.inboxcustomization.core-1.0-SNAPSHOT.jar)
* [開啟收件匣](http://localhost:4502/aem/inbox)
* 按一下&#x200B;_Create_&#x200B;按鈕旁的&#x200B;_清單檢視_&#x200B;圖示，開啟管理控制項
* 將「已婚」列添加到「收件箱」中並保存更改
* [轉到FormsAndDocuments UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [通過選擇「檔案」(File)「從](assets/snap-form.zip) 建立」(Createmenu)中 _的「檔案_ 上傳」(Upload __ from)來導入示例表單
* [預覽表單](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* 選擇&#x200B;_婚姻狀態_並提交表格
   [檢視收件匣](http://localhost:4502/aem/inbox)

提交表單會觸發工作流程，並將任務指派給「管理員」使用者。 您應會在「已婚」欄下方看到值，如此螢幕擷取畫面所示

![已婚人士](assets/married-column.PNG)
