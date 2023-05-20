---
title: 設定Web渠道文檔的傳遞
seo-title: Setting up the delivery of web channel document
description: 這是建立第一個互動式通信文檔的多步驟教程的最後一部分。 在本部分，我們將查看通過電子郵件傳送Web渠道文檔。
seo-description: This is the final part of a multistep tutorial for creating your first interactive communications document. In this part, we look at the delivery of web channel document via email.
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
exl-id: 510d1782-59b9-41a6-a071-a16170f2cd06
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 0%

---

# 設定Web渠道文檔的傳遞 {#setting-up-the-delivery-of-web-channel-document}


在本部分，我們將查看通過電子郵件傳送Web渠道文檔。

定義並測試Web渠道交互通信文檔後，您需要一個傳遞機制將Web渠道文檔傳遞給收件人。

為了能夠將電子郵件用作我們的Web渠道文檔的傳遞機制，我們需要對表單資料模型做一些小的更改。

[瞭解有關通過電子郵件提供Web渠道的更多資訊](/help/forms/interactive-communications/delivery-of-web-channel-document-tutorial-use.md)

登錄AEM Forms。

* 導航到Forms->資料整合

* 在編輯模式下開啟RetirementAccountStatement資料模型。

* 選擇餘額對象，然後按一下編輯按鈕。

* 選擇「鉛筆」表徵圖以在編輯模式下開啟id參數。

* 將綁定更改為「RequestAttribute」。

* 在綁定值中設定帳號，如下所示。

* 這樣，我們將帳戶編號通過request屬性傳遞到表單資料模型

* 確保保存更改。
   ![fd](assets/requestattribute.gif)

## TestWeb渠道文檔的電子郵件傳遞 {#test-email-delivery-of-web-channel-document}

* [使用包管理器安裝示例資產](assets/webchanneldelivery.zip)
* [登錄到crx](http://localhost:4502/crx/de/index.jsp#)

* 導航到/home/users

* 在用戶節點下搜索管理員用戶。

* 選擇管理員用戶的配置檔案節點。

* 建立名為「accountnumber」的屬性。 確保屬性類型為字串。

* 將此accountnumber屬性的值設定為「3059827」。 您可以根據需要將此值設定為任意隨機數。

* [開啟getad.html](http://localhost:4502/content/getad.html)

* 與此URL關聯的代碼將獲取登錄用戶的帳號。 然後，此帳號將作為requestattribute傳遞給FDM。 然後，FDM將提取與此帳號關聯的資料並填充Web通道文檔。

>[!NOTE]
>
>請看 **/apps/AEMForms/fetchad/GET.jsp** 的下界。 請確保字串變數webChannelDocument指向有效的通信文檔路徑。

## 後續步驟

[設定電子郵件傳遞](../interactive-communications/delivery-of-web-channel-document-tutorial-use.md)