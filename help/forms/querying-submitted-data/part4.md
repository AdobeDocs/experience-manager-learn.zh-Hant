---
title: AEM Forms，帶JSON架構和資料[Part4]
seo-title: AEM Forms with JSON Schema and Data[Part4]
description: 多部分教程，引導您完成建立帶JSON架構的自適應表單和查詢提交資料所涉及的步驟。
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
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---

# 查詢已提交資料


下一步是查詢提交的資料並以表格形式顯示結果。 要完成此任務，我們將使用以下軟體

[查詢生成器](https://querybuilder.js.org/)  — 用於建立查詢的UI元件

[資料表](https://datatables.net/) — 以表格形式顯示查詢結果。

生成以下UI以啟用查詢已提交的資料。 只有JSON架構中標籤為必需的元素才可用於查詢。 在下面的螢幕截圖中，我們正在查詢所有提交，其中deliverypref是SMS。

用於查詢已提交資料的示例UI不使用QueryBuilder中提供的所有高級功能。 我們鼓勵你自己試試。

![querybuilder](assets/querybuilderui.gif)

>[!NOTE]
>
>本教程的當前版本不支援查詢多列。

當您選擇表單以執行查詢時，將調用GET **/bin/getdatakeysfromschema**。 此GET調用返回與表單架構關聯的必填欄位。 然後，將在QueryBuilder的下拉清單中填入所需欄位，以便生成查詢。

以下代碼段調用JSONSchemaOperations服務的getRequiredColumnsFromSchema方法。 我們將架構的屬性和所需元素傳遞給此方法調用。 此函式調用返回的陣列隨後用於填充查詢生成器下拉清單

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

按一下GetResult按鈕時，將調用Get **&quot;/bin/querydata&quot;**。 通過查詢參數將QueryBuilder UI生成的查詢傳遞給Servlet。 然後，Servlet將此查詢按摩到可用於查詢資料庫的SQL查詢中。 例如，如果您正在搜索以檢索名為「Mouse」的所有產品，則Query Builder查詢字串將為$.productname = &#39;Mouse&#39;。 然後，此查詢將轉換為以下

選擇 &#42; 從aemformswithjson中。  formsmissions，其中JSON_EXTRACT(formsmissions .formdata,&quot;$.productName &quot;)= &#39;Mouse&#39;

然後返回此查詢的結果以填充UI中的表。

要使此示例在本地系統上運行，請執行以下步驟

1. [確保已執行此處提到的所有步驟](part2.md)
1. [使用包管理器導入Dashboardv2.zipAEM。](assets/dashboardv2.zip) 此包包含查詢資料所需的所有捆綁包、配置設定、自定義提交和示例頁。
1. 使用示例json架構建立自適應表單
1. 將Adaptive Form配置為提交到「customsubmithelpx」自定義提交操作
1. 填寫表單並提交
1. 將瀏覽器指向 [dashboard.html](http://localhost:4502/content/AemForms/dashboard.html)
1. 選擇表單並執行簡單查詢
