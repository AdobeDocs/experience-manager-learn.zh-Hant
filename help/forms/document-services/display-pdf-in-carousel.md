---
title: 顯示多個PDF文檔
description: 以自適應形式循環查看多個pdf文檔。
version: 6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
kt: 10292
exl-id: c1d248c3-8208-476e-b0ae-cab25575cd6a
last-substantial-update: 2021-10-12T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 4%

---

# 在旋轉傳送器中顯示多個PDF文檔

常見的使用情形是在提交表單之前，將多個PDF文檔顯示到表單填充器中以進行審閱。

要完成此使用案例，我們利用 [Adobe PDF嵌入式API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html)。

[在此可體驗此示例的現場演示。](https://forms.enablementadobe.com/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)

執行了以下步驟以完成整合

## 建立自定義元件以顯示多個PDF文檔

已建立自定義元件（pdf — 旋轉盤）以循環使用pdf文檔

## 用戶端資源庫

已建立客戶端庫以使用Adobe PDF嵌入API顯示PDF。 要顯示的PDF在pdf — 旋轉傳送元件中指定。

## 建立自適應窗體

建立基於某些標籤的自適應表單（此示例有3個標籤）在前兩個標籤中添加一些自適應表單元件在第三個標籤中添加pdf轉盤元件配置pdf轉盤元件，如下面的螢幕快照所示
![pdf旋轉](assets/pdf-carousel-af-component.png)

**嵌入PDFAPI密鑰**  — 這是可用於嵌入pdf的鍵。 此密鑰僅與localhost一起使用。 您可以建立 [你自己的鑰匙](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) 並與其他域關聯。

**指定PDF文檔**  — 在此處，您可以指定要在旋轉傳送器中顯示的pdf文檔。


## 在伺服器上部署示例

要在本地伺服器上test此項，請執行以下步驟：

1. [導入客戶端庫](assets/pdf-carousel-client-lib.zip) 進入本地實AEM例 [使用包管理器](http://localhost:4502/crx/packmgr/index.jsp)
1. [導入pdf旋轉傳送元件](assets/pdf-carousel-component.zip) 進入本地實AEM例 [使用包管理器](http://localhost:4502/crx/packmgr/index.jsp)
1. [導入自適應窗體 ](assets/adaptive-form-pdf-carousel.zip) 進入本地實AEM例 [使用包管理器](http://localhost:4502/crx/packmgr/index.jsp)
1. [導入要顯示的示例pdf](assets/pdf-carousel-sample-documents.zip) 進入本地實AEM例 [使用assets檔案上載連結](http://localhost:4502/assets.html/content/dam)
1. [預覽自適應窗體](http://localhost:4502/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)
1. 頁籤。 在旋轉木馬元件中應看到三個PDF文檔。
