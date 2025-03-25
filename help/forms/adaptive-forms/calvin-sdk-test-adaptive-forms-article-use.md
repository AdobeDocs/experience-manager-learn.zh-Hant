---
title: 搭配AEM Adaptive Forms使用自動化測試
description: 使用Calvin SDK自動測試最適化Forms
feature: Adaptive Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 5a1364f3-e81c-4c92-8972-4fdc24aecab1
last-substantial-update: 2020-09-10T00:00:00Z
duration: 101
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 0%

---

# 搭配AEM Adaptive Forms使用自動化測試 {#using-automated-tests-with-aem-adaptive-forms}

使用Calvin SDK自動測試最適化Forms

Calvin SDK是Adaptive Forms開發人員用來測試Adaptive Forms的公用程式API。 Calvin SDK是建置在[Hobbes.js測試架構](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html)之上。 Calvin SDK自AEM Forms 6.3起即可使用。

在本教學課程中，您將建立下列專案：

* 測試套裝
* 測試套裝將包含一或多個測試案例
* 測試案例將包含一或多個動作

## 快速入門 {#getting-started}

[使用封裝管理員下載及安裝Assets](assets/testingadaptiveformsusingcalvinsdk1.zip)此封裝包含範例指令碼和數個最適化Forms。這些最適化Forms是使用AEM Forms 6.3版本建置的。 如果您要在AEM Forms 6.4或更新版本上測試新表單，建議您建立與您的AEM Forms版本專屬的新表單。 範例指令碼示範各種Calvin SDK API，可用於測試最適化Forms。 測試AEM Adaptive Forms的一般步驟為：

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

* 在此案例中，TestSuite的名稱為&#39; `Mortgage Form Test`&#39;。
* 提供AEM中內含測試套裝之js檔案的絕對路徑。
* 當設定為&#39; `true`&#39;時，登入引數會使測試套裝在測試UI中可供使用。

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
>如果您是在AEM Forms 6.4或以上版本上測試此功能，請建立新的調適型表單並使用它進行您的測試。不建議使用套件隨附的調適型表單。

可將測試案例新增至測試套裝，以針對調適型表單執行。

* 若要將測試案例新增至測試套裝，請使用TestSuite物件的`addTestCase`方法。
* `addTestCase`方法將TestCase物件當成引數。
* 若要建立TestCase，請使用`hobs.TestCase(..)`方法
* 注意：第一個引數是將出現在UI中的測試案例的名稱。
* 建立測試案例後，您就可以將動作新增至測試案例。
* 包含`navigateTo`、`asserts.isTrue`的動作可以新增為測試案例的動作。

## 執行自動化測試 {#running-the-automated-tests}

[Openthetestsuite](http://localhost:4502/libs/granite/testing/hobbes.html)展開測試套件並執行測試。 如果一切順利執行，您將會看到下列輸出。

![calvinsdk](assets/calvinimage.png)

## 請嘗試範例測試套裝 {#try-out-the-sample-test-suites}

作為範例套件的一部分，還有三個額外的測試套裝。 您可以在clientlibrary的js.txt檔案中加入適當的檔案來嘗試這些做法，如下所示：

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```
