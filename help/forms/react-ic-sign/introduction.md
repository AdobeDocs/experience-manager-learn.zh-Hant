---
title: 使用AEM Forms和Acrobat Sign的React應用程式
description: Acrobat Sign和AEM Forms可讓複雜的交易自動化，並加入法律電子簽章，以提供順暢的數位體驗。
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
jira: KT-13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: 64172af3-2905-4bc8-8311-68c2a70fb39e
duration: 34
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 1%

---

# AEM Forms與Acrobat Sign網路表單


本教學課程將逐步引導您瞭解使用提交資料產生互動式通訊檔案的使用案例。 [React](https://react.dev/) 應用程式並展示產生的檔案，以供使用Acrobat Sign網頁表單簽名。

以下是使用案例的流程

* 使用者在React應用程式中填寫表單。
* 表單資料會提交至AEM Forms端點，以產生互動式通訊檔案。
* 使用產生的檔案建立Acrobat Sign Widget url。
* 將Widget URL呈現給呼叫應用程式，讓使用者簽署檔案。

## 先決條件

使用案例需要下列專案才能運作：

* 具有Forms附加套件的AEM伺服器
* 一個 [Acrobat Sign應用程式的整合金鑰](https://helpx.adobe.com/sign/kb/how-to-create-an-integration-key.html)

## 後續步驟

寫入 [自訂OSGi服務以產生互動式通訊檔案](./create-ic-document.md) 使用記錄的API
