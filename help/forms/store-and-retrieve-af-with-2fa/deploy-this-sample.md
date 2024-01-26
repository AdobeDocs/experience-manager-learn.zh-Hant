---
title: 部署範例
description: 在本機AEM Forms執行個體上取得正在執行的使用案例
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6602
thumbnail: 6602.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: cdfae631-86d7-438f-9baf-afd621802723
duration: 98
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '373'
ht-degree: 1%

---

# 部署範例

若要讓此使用案例在您的系統上正常運作，請遵循下列指示：

>[!NOTE]
>假設您正在連線埠4502上執行AEM Forms。


## 建立資料庫

此範例使用MySQL資料庫來儲存最適化表單資料。 您需要建立 [資料庫結構描述，透過匯入結構描述檔案](assets/data-base-schema.sql) 移入MySQL Workbench。

## 建立資料來源

您需要建立一個名為的Apache Sling Connection Pooled DataSource **StoreAndRetrieveAfData** 指向在先前步驟中建立的資料庫綱要。 OSGi套件組合中的程式碼會使用此資料來源名稱。

## 建立表單資料模型

需要根據這個名為的資料來源建立表單資料模型 **StoreAndRetrieveAfData**. 此表單資料模型用於擷取與應用程式ID相關聯的行動電話號碼。 表單資料模型可以 [已從此處下載。](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## 使用nexmo建立開發人員帳戶

建立開發人員帳戶，使用 [Nexmo](https://dashboard.nexmo.com/) 用於傳送及驗證OTP代碼。 記下API金鑰和API秘密金鑰。 已針對此服務為您建立資料來源和表單資料模型，並包含在先前步驟中所述的資產中。

## 部署下列OSGi套裝

部署具有 [儲存及擷取資料庫資料的程式碼](assets/SaveAndResume.core-1.0.0-SNAPSHOT.jar)
下載並解壓縮 [developingwithserviceuser.zip](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip).
使用Felix Web主控台部署DevelopingWithServiceUser.jar檔案。

## 部署使用者端程式庫

範例使用2個使用者端資料庫。 匯入這些 [使用者端資料庫](assets/store-af-with-attachments-client-lib.zip) 到AEM中。

## 匯入自訂最適化表單範本

此示範中使用的範例表單是根據自訂範本。 匯入 [將自訂範本移入AEM](assets/custom-template-with-page-component.zip)

## 匯入最適化表單範例

構成此範例的2份表單需要匯入至AEM。 範例表單可以是 [已從此處下載](assets/sample-forms.zip)

開啟 [我的帳戶表單](http://localhost:4502/editor.html/content/forms/af/myaccountform.html) 在編輯模式中。 在最適化表單的適當欄位中指定Vonage API金鑰和API密碼值。

## 測試解決方案

預覽 [StoreAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)
輸入您的手機號碼，包括國家/地區代碼，填寫您的使用者詳細資料並新增一些附件。 按一下「儲存並退出」按鈕，儲存最適化表單及其附件


## 使用案例示範

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)
