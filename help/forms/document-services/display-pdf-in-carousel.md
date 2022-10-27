---
title: 顯示多個pdf文檔
description: 以最適化表單循環處理多個pdf檔案。
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

# 在輪播中顯示多個PDF檔案

常見的使用案例是在提交表單前，將多個PDF檔案顯示到表單填寫器，以便審核。

為了完成此使用案例，我們已使用 [Adobe PDF Embed API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

[您可在此體驗此範例的即時示範。](https://forms.enablementadobe.com/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)

執行下列步驟以完成整合

## 建立自訂元件以顯示多個PDF檔案

已建立自訂元件(pdf-carousel)，可循環瀏覽pdf檔案

## 用戶端資源庫

已建立用戶端程式庫，以使用Adobe PDF Embed API顯示PDF。 要顯示的PDF會在pdf-carousel元件中指定。

## 建立最適化表單

根據某些索引標籤建立最適化表單（此範例有3個索引標籤）在前兩個索引標籤中新增一些最適化表單元件在第三個索引標籤中新增pdf輪播元件如下方螢幕擷取所示設定pdf輪播元件
![pdf-carousel](assets/pdf-carousel-af-component.png)

**內嵌PDFAPI金鑰**  — 這是您用來內嵌pdf的索引鍵。 此鍵只能與localhost一起使用。 您可以建立 [您自己的密鑰](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) 並將其與其他網域建立關聯。

**指定PDF文檔**  — 您可以在此處指定要顯示在輪播中的PDF檔案。


## 在伺服器上部署示例

若要在本機伺服器上測試，請執行下列步驟：

1. [匯入用戶端程式庫](assets/pdf-carousel-client-lib.zip) 到您的本機AEM例項 [使用套件管理器](http://localhost:4502/crx/packmgr/index.jsp)
1. [匯入pdf轉盤元件](assets/pdf-carousel-component.zip) 到您的本機AEM例項 [使用套件管理器](http://localhost:4502/crx/packmgr/index.jsp)
1. [匯入最適化表單 ](assets/adaptive-form-pdf-carousel.zip) 到您的本機AEM例項 [使用套件管理器](http://localhost:4502/crx/packmgr/index.jsp)
1. [匯入範例pdf以顯示](assets/pdf-carousel-sample-documents.zip) 到您的本機AEM例項 [使用assets檔案上傳連結](http://localhost:4502/assets.html/content/dam)
1. [預覽最適化表單](http://localhost:4502/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)
1. 頁簽到要查看的文檔頁簽。 您應該會在轉盤元件中看到三個PDF檔案。
