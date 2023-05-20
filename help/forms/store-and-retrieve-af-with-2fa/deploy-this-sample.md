---
title: 部署示例
description: 獲取在本地AEM Forms實例上運行的用例
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

# 部署示例

要使此用例在您的系統上工作，請按照以下說明操作：

>[!NOTE]
>假定您在4502埠運行AEM Forms。


## 建立資料庫

此示例使用MySQL資料庫來儲存自適應表單資料。 您需要建立 [資料庫模式，方法是導入模式檔案](assets/data-base-schema.sql) 進入MySQL工作台。

## 建立資料源

您需要建立名為 **儲存和檢索AfData**。 OSGi捆綁包中的代碼使用此資料源名稱

## 建立表單資料模型

需要基於此名為的資料源建立窗體資料模型 **儲存和檢索AfData**。 此表單資料模型用於獲取與應用程式ID相關聯的行動電話號碼。 表單資料模型可以 [從此處下載。](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## 使用Nexmo建立開發人員帳戶

建立開發人員帳戶 [內克莫](https://dashboard.nexmo.com/) 用於發送和驗證OTP代碼。 記下API密鑰和API密鑰。 已經針對此服務為您建立了資料源和表單資料模型，並包含在上一步中提到的資產中。

## 部署以下OSGi捆綁包

部署具有 [從資料庫儲存和提取資料的代碼](assets/FetchPartiallyCompletedForm.PartiallyCompletedForm.core-1.0-SNAPSHOT.jar)
下載並解壓縮 [開發與serviceuser.zip](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip)。
使用Felix Web控制台部署DevelopingWithServiceUser.jar檔案。

## 部署客戶端庫

示例使用2個客戶端庫。 導入這些 [客戶端庫](assets/client-libraries.zip) 進AEM去。

## 導入自定義自適應表單模板

本演示中使用的示例表單基於自定義模板。 導入 [自定義模板AEM](assets/custom-template-with-page-component.zip)

## 導入示例自適應表單

組成此示例的2個窗體需要導入AEM。 示例表單可以 [從此處下載](assets/sample-forms.zip)

開啟 [我的帳戶表單](http://localhost:4502/editor.html/content/forms/af/myaccountform.html) 的子菜單。 在自適應表單的相應欄位中指定API密鑰和API密鑰值。

## Test解決方案

預覽 [儲存AFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)
輸入您的移動號碼（包括國家/地區代碼），填寫用戶詳細資訊並添加一些附件。 按一下「保存並退出」按鈕以保存自適應表單及其附件


## 演示使用案例

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)
