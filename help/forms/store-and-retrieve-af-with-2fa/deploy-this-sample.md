---
title: 部署範例
description: 取得在您本機AEM Forms例項上執行的使用案例
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6602
thumbnail: 6602.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 1%

---



# 部署範例

若要讓此使用案例在您的系統上運作，請依照下列指示進行：

>[!NOTE]
>假設您正在連接埠4502上執行AEM Forms。


## 建立資料庫

此示例使用MySQL資料庫來儲存自適應表單資料。 您需要通過將方案文 [件導入到MySQL工作台中](assets/data-base-schema.sql) ，建立資料庫方案。

## 建立資料來源

您需要建立名為 **StoreAndRetrieveAfData的資料來源**。 OSGi Bundle中的程式碼使用此資料來源名稱

## 建立表單資料模型

表單資料模型需要根據此資料來源(稱為 **StoreAndRetrieveAfData**)來建立。 此表單資料模型可用來擷取與應用程式ID相關的行動電話號碼。 您可從此處下載表 [單資料模型。](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## 使用Nexmo建立開發人員帳戶

使用 [Nexmo建立開發人員帳戶](https://dashboard.nexmo.com/) ，以傳送和驗證OTP代碼。 記下API金鑰和API機密金鑰。 資料來源和表單資料模型已針對本服務建立，並包含在上一步驟提及的資產中。

## 部署下列OSGi組合

部署包含程式碼的套件， [以儲存並擷取資料庫中的資料](assets/FetchPartiallyCompletedForm.PartiallyCompletedForm.core-1.0-SNAPSHOT.jar)部署 [DevelopingWithServiceUser Bundle](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)。

## 部署客戶端庫

範例使用2個用戶端程式庫。 將這些用 [戶端程式庫](assets/client-libraries.zip) 匯入AEM。

## 匯入自訂最適化表單範本

本展示中使用的範例表單以自訂範本為基礎。 將自訂 [範本匯入AEM](assets/custom-template-with-page-component.zip)

## 匯入範例最適化表單

組成此範例的2個表單必須匯入AEM。 您可從此處下 [載範例表格](assets/sample-forms.zip)

Open the [MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html) in edit mode. 在最適化表單的適當欄位中指定API金鑰和API密碼值。

## 測試解決方案

預覽 [StoreAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)輸入您的行動電話號碼（包括國家／地區代碼），填寫您的使用者詳細資料並新增一些附件。 按一下「保存並退出」按鈕以保存最適化表單及其附件


## 使用案例的展示

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)
