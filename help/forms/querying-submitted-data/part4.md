---
title: AEM Forms搭配JSON結構描述和資料[Part4]
seo-title: AEM Forms with JSON Schema and Data[Part4]
description: 多部分教學課程，逐步引導您完成使用JSON結構描述建立適用性表單和查詢提交資料的相關步驟。
seo-description: Multi-Part tutorial to walk you through the steps involved in creating Adaptive Form with JSON schema and querying the submitted data.
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: a8d8118d-f4a1-483f-83b4-77190f6a42a4
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 0%

---

# 查詢已提交的資料


下一步是查詢提交的資料，並以表格方式顯示結果。 為此，我們使用下列軟體：

[QueryBuilder](https://querybuilder.js.org/)  — 用於建立查詢的UI元件

[資料表](https://datatables.net/) — 以表格形式顯示查詢結果。

已建立下列UI，以啟用查詢已提交的資料。 只有JSON結構中標示為必要的元素可供查詢。 在下方的螢幕擷取中，我們會查詢所有傳送首頁為SMS的提交。

查詢提交資料的範例UI不會使用QueryBuilder中提供的所有進階功能。 你被鼓勵自己試試。

![querybuilder](assets/querybuilderui.gif)

>[!NOTE]
>
>本教學課程的目前版本不支援查詢多欄。

當您選取表單以執行查詢時，會呼叫GET **/bin/getdatakeyfromschema**. 此GET呼叫會傳回與表單結構相關聯的必要欄位。 接著，必填欄位會填入QueryBuilder下拉式清單中，供您建立查詢。

以下代碼段對JSONSchemaOperations服務的getRequiredColumnsFromSchema方法進行調用。 我們會將架構的屬性和必要元素傳遞至此方法呼叫。 此函式呼叫傳回的陣列隨後將用來填入查詢產生器下拉式清單

```java
public JSONArray getData(String formName) throws SQLException, IOException {

  org.json.JSONArray arrayOfDataKeys = new org.json.JSONArray();
  JSONObject jsonSchema = jsonSchemaOperations.getJSONSchemaFromDataBase(formName);
  Map<String, String> refKeys = new HashMap<String, String>();

  try {
   JSONObject properties = jsonSchema.getJSONObject("properties");
   JSONArray requiredFields = jsonSchema.has("required") ? jsonSchema.getJSONArray("required") : null;
   jsonSchemaOperations.getRequiredColumnsFromSchema(properties, arrayOfDataKeys, "", jsonSchema, refKeys,
     requiredFields);
  } catch (JSONException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return arrayOfDataKeys;

 }
```

按一下GetResult按鈕時，會對 **&quot;/bin/querydata&quot;**. 我們會透過查詢參數將QueryBuilder UI建立的查詢傳遞至servlet。 然後，Servlet將此查詢按摩為SQL查詢，以用於查詢資料庫。 例如，如果您搜尋以擷取名為「滑鼠」的所有產品，查詢產生器的查詢字串就是 `$.productname = 'Mouse'`. 然後，此查詢將轉換為以下內容

選擇 &#42; 從aemformswithjson。  JSON_EXTRACT(formsubmissions.formdata,&quot;$.productName &quot;)= &#39;Mouse&#39;的表單提交

然後會傳回此查詢的結果，以填入UI中的表格。

要使此示例在本地系統上運行，請執行以下步驟

1. [請確定您已遵循此處提及的所有步驟](part2.md)
1. [使用AEM Package Manager匯入Dashboardv2.zip。](assets/dashboardv2.zip) 此套件包含所有必要的套件組合、組態設定、自訂提交及查詢資料的範例頁面。
1. 使用範例json結構描述建立最適化表單
1. 設定適用性表單以提交至「customsubmithelpx」自訂提交動作
1. 填寫表格並提交
1. 將瀏覽器指向 [dashboard.html](http://localhost:4502/content/AemForms/dashboard.html)
1. 選取表單並執行簡單查詢
