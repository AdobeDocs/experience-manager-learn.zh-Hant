---
title: '搭配AEM適用性Forms使用自動化測試 '
seo-title: '搭配AEM適用性Forms使用自動化測試 '
description: 使用Calvin SDK自動測試適用性Forms
seo-description: 使用Calvin SDK自動測試適用性Forms
feature: 適用性表單
topics: development
audience: developer
doc-type: article
activity: develop
version: 6.3,6.4,6.5
uuid: 3ad4e6d6-d3b1-4e4d-9169-847f74ba06be
topic: 開發
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '463'
ht-degree: 0%

---


# 搭配AEM適用性Forms {#using-automated-tests-with-aem-adaptive-forms}使用自動化測試

使用Calvin SDK自動測試適用性Forms

Calvin SDK是一個公用程式API，供適用性Forms開發人員測試適用性Forms。 Calvin SDK建置在[Hobbes.js測試架構](https://docs.adobe.com/docs/en/aem/6-3/develop/ref/test-api/index.html)之上。 Calvin SDK隨AEM Forms 6.3起提供。

在本教學課程中，您將建立下列內容：

* 測試套裝
* 測試套裝將包含一或多個測試案例
* 測試案例將包含一或多個動作

## 入門{#getting-started}

[使用套件管理程式下載](assets/testingadaptiveformsusingcalvinsdk1.zip)和安裝資產此套件包含範例指令碼和數個最適化Forms。這些最適化Forms是使用AEM Forms 6.3版建置。如果您要在AEM Forms 6.4或更新版本上測試，建議您建立AEM Forms版本專屬的新表單。 示例指令碼演示了可用於測試適用性Forms的各種Calvin SDK API。 測試AEM適用性Forms的一般步驟為：

* 導覽至需要測試的表單
* 設定欄位的值
* 提交最適化表單
* 檢查錯誤訊息

套件中的範例指令碼會示範上述所有動作。
讓我們探索`mortgageForm.js`的程式碼

```javascript
var mortgageFormTS = new hobs.TestSuite("Mortgage Form Test", {
        path: '/etc/clientlibs/testingAFUsingCalvinSDK/mortgageForm.js',
        register: true
})
```

上述程式碼會建立新的測試套裝。

* 在本例中， TestSuite的名稱為「 `Mortgage Form Test` 」。
* 提供AEM中包含測試套裝之js檔案的絕對路徑。
* 設為「 `true` 」時的註冊參數會讓測試套裝可在測試UI中使用。

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
>如果您要在AEM Forms 6.4或更新版本上測試此功能，請建立新的適用性表單並使用它進行測試。不建議使用隨套件提供的適用性表單。

可將測試案例新增至要針對適用性表單執行的測試套裝。

* 若要將測試案例新增至測試套裝，請使用TestSuite物件的`addTestCase`方法。
* `addTestCase`方法以TestCase對象為參數。
* 要建立TestCase，請使用`hobs.TestCase(..)`方法。
* 注意：第一個參數是將出現在UI中的測試案例名稱。
* 建立測試案例後，您就可以將動作新增至您的測試案例。
* 可將包含`navigateTo`、`asserts.isTrue`的動作新增為測試案例的動作。

## 運行自動測試{#running-the-automated-tests}

[](http://localhost:4502/libs/granite/testing/hobbes.html)開啟testsuite展開測試套裝並執行測試。如果所有項目都成功執行，您會看到下列輸出。

![calvinsdk](assets/calvinimage.png)

## 請試用範例測試套裝{#try-out-the-sample-test-suites}

作為範例套件的一部分，另有三個測試套裝。 您可以在clientlibrary的js.txt檔案中加入適當的檔案，以嘗試使用，如下所示：

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```
