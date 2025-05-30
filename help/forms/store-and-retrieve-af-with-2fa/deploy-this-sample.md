---
title: 部署範例
description: 在本機AEM Forms執行個體上取得正在執行的使用案例
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6602
thumbnail: 6602.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: cdfae631-86d7-438f-9baf-afd621802723
duration: 186
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '373'
ht-degree: 1%

---

# 部署範例

若要讓此使用案例在您的系統上正常運作，請遵循下列指示：

>[!NOTE]
>假設您正在連線埠4502上執行AEM Forms。


## 建立資料庫

此範例使用MySQL資料庫來儲存最適化表單資料。 您必須將結構描述檔案[&#128279;](assets/data-base-schema.sql)匯入MySQL Workbench，以建立資料庫結構描述。

## 建立資料來源

您必須建立名稱為&#x200B;**StoreAndRetrieveAfData**&#x200B;的Apache Sling Connection Pooled DataSource，並指向在先前步驟中建立的資料庫結構描述。 OSGi套件組合中的程式碼會使用此資料來源名稱。

## 建立表單資料模型

需要根據這個名為&#x200B;**StoreAndRetrieveAfData**&#x200B;的資料來源建立表單資料模型。 此表單資料模型用於擷取與應用程式ID相關聯的行動電話號碼。 可從這裡[下載表單資料模型。](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## 使用nexmo建立開發人員帳戶

建立具有[Nexmo](https://dashboard.nexmo.com/)的開發人員帳戶，以傳送及驗證OTP代碼。 記下API金鑰和API秘密金鑰。 已針對此服務為您建立資料來源和表單資料模型，並包含在先前步驟中所述的資產中。

## 部署下列OSGi套裝

部署具有[程式碼的套件組合，以儲存及擷取資料庫](assets/SaveAndResume.core-1.0.0-SNAPSHOT.jar)中的資料
下載並解壓縮[developingwithserviceuser.zip](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=zh-Hant)。
使用Felix Web主控台部署DevelopingWithServiceUser.jar檔案。

## 部署使用者端程式庫

範例使用2個使用者端資料庫。 將這[個使用者端資料庫](assets/store-af-with-attachments-client-lib.zip)匯入AEM。

## 匯入自訂最適化表單範本

此示範中使用的範例表單是根據自訂範本。 將[自訂範本匯入AEM](assets/custom-template-with-page-component.zip)

## 匯入最適化表單範例

構成此範例的2份表單需要匯入至AEM。 範例表單可從這裡[&#128279;](assets/sample-forms.zip)下載

在編輯模式中開啟[MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html)。 在最適化表單的適當欄位中指定Vonage API金鑰和API密碼值。

## 測試解決方案

預覽[StoreAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)
輸入您的手機號碼，包括國家/地區代碼，填寫您的使用者詳細資料並新增一些附件。 按一下「儲存並退出」按鈕，儲存最適化表單及其附件


## 使用案例示範

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)
