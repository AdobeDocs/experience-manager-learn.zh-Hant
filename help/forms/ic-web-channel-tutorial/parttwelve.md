---
title: 設定Web頻道檔案的傳送
seo-title: 設定Web頻道檔案的傳送
description: 這是建立您第一個互動式通訊檔案的多步驟教學課程的最後部分。 在本部分，我們將討論透過電子郵件傳送Web通路檔案的方式。
seo-description: 這是建立您第一個互動式通訊檔案的多步驟教學課程的最後部分。 在本部分，我們將討論透過電子郵件傳送Web通路檔案的方式。
uuid: c1066600-1abd-4401-b04f-b93c28603cc7
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 1a7cf095-c5d8-4d92-a018-883cda76fe70
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 1%

---


# 設定Web頻道檔案{#setting-up-the-delivery-of-web-channel-document}的傳送


在本部分，我們將討論透過電子郵件傳送Web通路檔案的方式。

在您定義並測試網路頻道互動式通訊檔案後，您需要一種傳送機制，將網路頻道檔案傳送給收件者。

為了能夠將電子郵件當做我們Web通路檔案的傳送機制，我們需要對表單資料模型做小幅變更。

[若要進一步瞭解透過電子郵件傳送的網路管道](/help/forms/interactive-communications/delivery-of-web-channel-document-tutorial-use.md)

登入AEM Forms。

* 導覽至Forms->資料整合

* 在編輯模式下開啟RetirementAccountStatement資料模型。

* 選擇餘額對象，然後按一下編輯按鈕。

* 選取「鉛筆」圖示，以在編輯模式中開啟id引數。

* 將系結變更為「RequestAttribute」。

* 在系結值中設定帳號，如下所示。

* 這樣，我們會透過request屬性將accountnumber傳入表單資料模型

* 請確定您已儲存變更。
   ![fdm](assets/requestattribute.gif)

## 測試Web Channel文檔的電子郵件傳送{#test-email-delivery-of-web-channel-document}

* [使用套件管理員安裝範例資產](assets/webchanneldelivery.zip)
* [登入crx](http://localhost:4502/crx/de/index.jsp#)

* 導覽至/home/users

* 在用戶節點下搜索管理員用戶。

* 選擇管理員用戶的配置檔案節點。

* 建立名為「accountnumber」的屬性。 請確定屬性類型為字串。

* 將此帳戶編號屬性的值設為&quot;3059827&quot;。 您可以視需要將此值設定為任何隨機數。

* [開啟getad.html](http://localhost:4502/content/getad.html)

* 與此URL關聯的程式碼將取得登入使用者的帳號。 然後，此帳戶編號將作為request屬性傳遞到FDM。 然後，FDM將讀取與此帳戶號碼關聯的資料，並填充Web渠道文檔。

>[!NOTE]
>
>請查看crx中的&#x200B;**/apps/AEMForms/fetchad/GET.jsp**&#x200B;檔案。 請確定字串變數webChannelDocument指向有效的通訊檔案路徑。
