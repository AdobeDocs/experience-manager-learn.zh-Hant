---
title: 使用自動TestAEM和自適應Forms
description: Calvin SDK在自適應Forms自動測試中的應用
feature: Adaptive Forms
doc-type: article
activity: develop
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 5a1364f3-e81c-4c92-8972-4fdc24aecab1
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 1%

---

# 使用自動TestAEM和自適應Forms {#using-automated-tests-with-aem-adaptive-forms}

Calvin SDK在自適應Forms自動測試中的應用

Calvin SDK是一個實用API，供適應性Forms開發者test適應性Forms. Calvin SDK是在 [霍布斯.js測試框架](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html)。 Calvin SDK在AEM Forms6.3以後提供。

在本教程中，您將建立以下內容：

* Test套件
* Test套件將包含一個或多個test案例
* Test案例將包含一個或多個操作

## 入門 {#getting-started}

[使用包管理器下載並安裝資產](assets/testingadaptiveformsusingcalvinsdk1.zip)軟體包包含示例指令碼和幾個自適應Forms。這些自適應Forms是使用AEM Forms6.3版構建的。 如果要在AEM Forms6.4或更高版本上測試此功能，建議建立特定於您的AEM Forms版本的新表單。 示例指令碼演示了各種Calvin SDK API，可用於testAdaptiveForms。 測試自適應Forms的AEM一般步驟是：

* 導航到需要測試的窗體
* 設定欄位的值
* 提交自適應表單
* 檢查錯誤消息

軟體包中的示例指令碼演示了上述所有操作。
讓我們來探索 `mortgageForm.js`

```javascript
var mortgageFormTS = new hobs.TestSuite("Mortgage Form Test", {
        path: '/etc/clientlibs/testingAFUsingCalvinSDK/mortgageForm.js',
        register: true
})
```

上面的代碼會建立新的Test套件。

* 本例中TestSuite的名稱為「 `Mortgage Form Test` 「 」。
* 提供了包含test套件AEM的js檔案的絕對路徑。
* 設定為「時的寄存器參數 `true` &#39;，使Test套件在測試UI中可用。

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
>如果您正在AEM Forms6.4或更高版本上測試此功能，請建立一個新的自適應表單，並使用它進行測試。不建議使用隨軟體包提供的自適應表單。

Test案例可以添加到test套件中，以針對自適應表單執行。

* 要將test案例添加到test套件，請使用 `addTestCase` TestSuite對象的方法。
* 的 `addTestCase` 方法將TestCase對象作為參數。
* 要建立TestCase，請使用 `hobs.TestCase(..)` 的雙曲餘切值。
* 注：第一個參數是將出現在UI中的Test事例的名稱。
* 建立test案例後，可以將操作添加到test案例。
* 包括 `navigateTo`。 `asserts.isTrue` 可以作為操作添加到test案例。

## 運行自動test {#running-the-automated-tests}

[Openthetestsuite](http://localhost:4502/libs/granite/testing/hobbes.html)展開Test套件並運行test。 如果所有操作都成功運行，您將看到以下輸出。

![卡文斯](assets/calvinimage.png)

## 試用示例test套件 {#try-out-the-sample-test-suites}

作為樣品包的一部分，還有另外三間test套房。 您可以通過在客戶端庫的js.txt檔案中包含相應的檔案來嘗試這些檔案，如下所示：

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```
