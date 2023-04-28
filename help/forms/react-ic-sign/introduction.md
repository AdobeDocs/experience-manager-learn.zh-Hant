---
title: 使用AEM Forms和Acrobat Sign React應用程式
description: Acrobat Sign和AEM Forms可讓複雜的交易自動化，並納入合法的電子簽名，以提供順暢的數位體驗。
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: 2ff7be5b-884c-420d-9a06-f0e2a99d3ef3
source-git-commit: 4709035983a5c6705c4e807d877ee71145f48987
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 1%

---

# AEM Forms與Acrobat Sign網路表單


本教學課程會逐步帶您了解如何使用 [React](https://react.dev/) 應用程式和呈現產生的檔案，以使用Acrobat Sign webform進行簽署。

以下是使用案例的流程

* 使用者在React應用程式中填寫表單。
* 表單資料會提交至AEM Forms端點，以產生互動式通訊檔案。
* 使用產生的檔案建立Acrobat Sign Widget URL。
* 將介面工具集URL呈現給呼叫應用程式，供使用者簽署檔案。

## 必備條件

使用案例需要下列項目才能運作：

* 具有Forms附加套件的AEM伺服器
* 安 [Acrobat Sign應用程式的整合金鑰](https://helpx.adobe.com/sign/kb/how-to-create-an-integration-key.html)

## 後續步驟

撰寫 [自定義OSGi服務以生成互動式通信文檔](./create-ic-document.md) 使用記錄的API
