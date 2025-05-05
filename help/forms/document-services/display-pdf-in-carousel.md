---
title: 顯示多個pdf檔案
description: 在最適化表單中循環瀏覽多個pdf檔案。
version: Experience Manager 6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
jira: KT-10292
exl-id: c1d248c3-8208-476e-b0ae-cab25575cd6a
last-substantial-update: 2021-10-12T00:00:00Z
duration: 66
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 1%

---

# 在浮動切換中顯示多個pdf檔案

常見的使用案例是向表單填寫工具顯示多個PDF檔案，以便在提交表單前進行稽核。

為了完成此使用案例，我們已運用[Adobe PDF Embed API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html)。

[您可以在這裡體驗此範例的即時示範。](https://forms.enablementadobe.com/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)

已執行下列步驟以完成整合

## 建立自訂元件以顯示多個PDF檔案

已建立自訂元件(pdf-carousel)以便在pdf檔案中循環

## 用戶端資源庫

已建立使用者端資料庫，以使用Adobe PDF Embed API顯示PDF。 要顯示的PDF以pdf轉盤元件指定。

## 建立最適化表單

使用某些標籤建立最適化表單（此範例有3個標籤）
在前兩個索引標籤中新增一些最適化表單元件
在第三個索引標籤中新增pdf轉盤元件
設定pdf-carousel元件，如下方熒幕擷圖所示
![pdf-carousel](assets/pdf-carousel-af-component.png)

**內嵌PDF API金鑰** — 此金鑰可用來內嵌PDF。 此金鑰僅適用於localhost。 您可以建立[您自己的金鑰](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html)，並將其與其他網域建立關聯。

**指定PDF檔案** — 您可在此指定要在傳送中顯示的pdf檔案。


## 在您的伺服器上部署範例

若要在本機伺服器上測試此專案，請遵循下列步驟：

1. [使用封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)將使用者端資料庫[&#128279;](assets/pdf-carousel-client-lib.zip)匯入您的本機AEM執行個體
1. [使用封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)將pdf轉盤元件[&#128279;](assets/pdf-carousel-component.zip)匯入您的本機AEM執行個體
1. [使用封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)將最適化表單[&#128279;](assets/adaptive-form-pdf-carousel.zip)匯入您的本機AEM執行個體
1. [使用資產檔案上傳連結](http://localhost:4502/assets.html/content/dam)匯入範例PDF以顯示[&#128279;](assets/pdf-carousel-sample-documents.zip)至您的本機AEM執行個體
1. [預覽最適化表單](http://localhost:4502/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)
1. 按Tab鍵至「要檢閱的檔案」標籤。 您應該會在轉盤元件中看到三份PDF檔案。
