---
title: 部署範例
description: 取得在本機AEM Forms執行個體上執行的使用案例
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6602
thumbnail: 6602.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: cdfae631-86d7-438f-9baf-afd621802723
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 1%

---

# 部署範例

若要讓此使用案例在您的系統上運作，請遵循下列指示：

>[!NOTE]
>假設您在4502埠上運行AEM Forms。


## 建立資料庫

此示例使用MySQL資料庫來儲存最適化表單資料。 您需要建立 [通過導入架構檔案實現資料庫架構](assets/data-base-schema.sql) 進入MySQL Workbench。

## 建立資料源

您需要建立資料源，稱為 **StoreAndRetrieveAfData**. OSGi套件中的程式碼會使用此資料來源名稱

## 建立表單資料模型

表單資料模型需根據此資料來源建立，稱為 **StoreAndRetrieveAfData**. 此表單資料模型用於擷取與應用程式ID相關聯的行動電話號碼。 表單資料模型可以是 [從這裡下載。](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## 使用nexmo建立開發人員帳戶

使用建立開發人員帳戶 [Nexmo](https://dashboard.nexmo.com/) 用於發送和驗證OTP代碼。 記下API金鑰和API密鑰。 已為您針對此服務建立資料來源和表單資料模型，並包含在前一個步驟提及的資產中。

## 部署以下OSGi套件組合

部署具有 [儲存資料庫中資料的代碼](assets/FetchPartiallyCompletedForm.PartiallyCompletedForm.core-1.0-SNAPSHOT.jar)
下載並解壓縮 [developingwithserviceuser.zip](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip).
使用Felix Web主控台部署DevelopingWithServiceUser.jar檔案。

## 部署客戶端庫

範例使用2個用戶端程式庫。 匯入這些 [用戶庫](assets/client-libraries.zip) 進入AEM。

## 匯入自訂最適化表單範本

本示範中使用的範例表單是以自訂範本為基礎。 匯入 [自訂範本至AEM](assets/custom-template-with-page-component.zip)

## 匯入範例最適化表單

組成此範例的2個表單必須匯入至AEM。 範例表單可以是 [從此處下載](assets/sample-forms.zip)

開啟 [MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html) 在編輯模式中。 在最適化表單的適當欄位中指定API金鑰和API密碼值。

## 測試解決方案

預覽 [StoreAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)
輸入您的手機號碼，包括國家/地區代碼、填寫您的用戶詳細資訊並添加一些附件。 按一下「儲存並退出」按鈕，儲存最適化表單及其附件


## 使用案例的展示

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)
