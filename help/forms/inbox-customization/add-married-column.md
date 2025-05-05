---
title: 新增自訂欄
description: 新增自訂欄以顯示工作流程的其他資料
feature: Adaptive Forms
doc-type: article
version: Experience Manager 6.5
jira: KT-5830
topic: Development
role: Developer
level: Experienced
exl-id: 0b141b37-6041-4f87-bd50-dade8c0fee7d
duration: 75
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 0%

---

# 新增自訂欄

若要在收件匣中顯示工作流程資料，我們必須在工作流程中定義和填入變數。 必須先設定變數的值，才能將任務指派給使用者。 為了讓您開一個好頭，我們提供了準備部署在AEM伺服器上的範例工作流程。

* [登入AEM](http://localhost:4502/crx/de/index.jsp)
* [匯入稽核工作流程](assets/review-workflow.zip)
* [檢閱工作流程](http://localhost:4502/editor.html/conf/global/settings/workflow/models/reviewworkflow.html)

此工作流程已定義兩個變數（isMarried和income），其值是使用設定變數元件設定的。 這些變數可作為要新增至AEM收件匣的欄

## 建立服務

對於每個需要顯示在收件匣中的欄，我們需要編寫服務。 下列服務可讓我們新增欄以顯示isMarried變數的值

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
>您必須在專案中加入AEM 6.5.5 Uber.jar，上述程式碼才能運作

![uber-jar](assets/uber-jar.PNG)

## 在您的伺服器上測試

* [登入AEM Web主控台](http://localhost:4502/system/console/bundles)
* [部署和啟動收件匣自訂套裝](assets/inboxcustomization.inboxcustomization.core-1.0-SNAPSHOT.jar)
* [開啟您的收件匣](http://localhost:4502/aem/inbox)
* 按一下&#x200B;_建立_&#x200B;按鈕旁的&#x200B;_清單檢視_&#x200B;圖示以開啟Admin Control
* 將「已婚」欄新增至「收件匣」並儲存變更
* [移至FormsAndDocuments UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 從&#x200B;_建立_&#x200B;功能表選取&#x200B;_檔案上傳_，以匯入範例表單[&#128279;](assets/snap-form.zip)
* [預覽表單](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* 選取&#x200B;_婚姻狀況_並提交表單
  [檢視收件匣](http://localhost:4502/aem/inbox)

提交表單會觸發工作流程，且任務會指派給「管理員」使用者。 您應該會在「已婚」欄下看到值，如本熒幕擷取畫面所示

![已婚資料行](assets/married-column.PNG)

## 後續步驟

[顯示已婚欄](./use-sightly-template.md)
