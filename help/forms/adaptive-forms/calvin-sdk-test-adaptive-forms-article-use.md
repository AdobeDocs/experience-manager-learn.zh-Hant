---
title: '使用自動化測試與自適AEM應Forms '
seo-title: '使用自動化測試與自適AEM應Forms '
description: Calvin SDK自動測試自適應Forms
seo-description: Calvin SDK自動測試自適應Forms
feature: 適用性表單
topics: development
audience: developer
doc-type: article
activity: develop
version: 6.3,6.4,6.5
uuid: 3ad4e6d6-d3b1-4e4d-9169-847f74ba06be
topic: 開發
role: 開發人員
level: 初學者
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 0%

---


# 使用自動測試AEM與自適應Forms{#using-automated-tests-with-aem-adaptive-forms}

Calvin SDK自動測試自適應Forms

Calvin SDK是一個公用API，可供Adaptive Forms開發人員用來測試AdaptiveForms。 Calvin SDK建立在[Hobbes.js測試架構](https://docs.adobe.com/docs/en/aem/6-3/develop/ref/test-api/index.html)之上。 Calvin SDK目前提供AEM Forms6.3版以上版本。

在本教學課程中，您將建立下列內容：

* 測試套件
* 測試套件將包含一或多個測試案例
* 測試案例將包含一或多個動作

## 開始使用{#getting-started}

[使用Package ](assets/testingadaptiveformsusingcalvinsdk1.zip)Manager下載和安裝資產此套件包含範例指令碼和數種Adaptive Forms。這些Adaptive Forms是使用AEM Forms6.3版構建的。如果您要在AEM Forms6.4或更新版本上測試此功能，建議您建立針對您的AEM Forms版本的新表格。 範例指令碼展示多種Calvin SDK API，可用來測試AdaptiveForms。 測試最適化Forms的一AEM般步驟包括：

* 導覽至需要測試的表單
* 設定欄位的值
* 提交最適化表單
* 檢查錯誤消息

套件中的範例指令碼會示範上述所有動作。
讓我們來探索`mortgageForm.js`的程式碼

```javascript
var mortgageFormTS = new hobs.TestSuite("Mortgage Form Test", {
        path: '/etc/clientlibs/testingAFUsingCalvinSDK/mortgageForm.js',
        register: true
})
```

上述程式碼會建立新的測試套裝。

* 在本例中，TestSuite的名稱為「 `Mortgage Form Test` 」。
* 提供包含測試套AEM裝之js檔案的絕對路徑。
* 設為&#39; `true` &#39;時的註冊參數，會讓測試UI中的測試套裝可供使用。

```javascript
.addTestCase(new hobs.TestCase("Calculate amount to borrow")
        // navigate to the mortgage form  which is to be tested
        .navigateTo("/content/forms/af/cal/mortgageform.html?wcmmode=disabled")
  .asserts.isTrue(function () {
            return calvin.isFormLoaded()
        })
```

>[!NOTE]
>
>如果您要在AEM Forms6.4或更高版本上測試此功能，請建立新的最適化表單，並使用它進行測試。不建議使用隨軟體包提供的最適化表單。

測試案例可新增至測試套裝，以針對最適化表單執行。

* 若要新增測試套裝的測試案例，請使用TestSuite物件的`addTestCase`方法。
* `addTestCase`方法將TestCase對象作為參數。
* 若要建立TestCase，請使用`hobs.TestCase(..)`方法。
* 注意：第一個參數是UI中將會出現的測試案例名稱。
* 建立測試案例後，您就可以將動作新增至測試案例。
* 包括`navigateTo`、`asserts.isTrue`的動作可新增為測試案例的動作。

## 運行自動測試{#running-the-automated-tests}

[Openthetestsuite](http://localhost:4502/libs/granite/testing/hobbes.html)展開測試套裝並執行測試。如果一切順利執行，您會看到下列輸出。

![calvinsdk](assets/calvinimage.png)

## 試用範例測試套裝{#try-out-the-sample-test-suites}

作為範例套件的一部分，另外有三個測試套件。 您可以在clientlibrary的js.txt檔案中加入適當的檔案，如下所示，以試用這些檔案：

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```
