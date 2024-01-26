---
title: 設定Web Channel檔案傳遞
description: 這是建立第一個互動式通訊檔案的多步驟教學課程的最後一部分。 在本部分中，我們將探討透過電子郵件傳送Web Channel檔案的方式。
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 1a7cf095-c5d8-4d92-a018-883cda76fe70
topic: Development
role: Developer
level: Beginner
exl-id: 510d1782-59b9-41a6-a071-a16170f2cd06
duration: 90
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 0%

---

# 設定Web Channel檔案傳遞 {#setting-up-the-delivery-of-web-channel-document}


在本部分中，我們將探討透過電子郵件傳送Web Channel檔案的方式。

定義並測試Web Channel互動式通訊檔案後，您需要一種傳遞機制來將Web Channel檔案傳遞給收件者。

為了能夠使用電子郵件作為Web Channel檔案的傳送機制，我們需要對表單資料模型進行微幅變更。

[若要進一步瞭解透過電子郵件進行的網路通道傳遞](/help/forms/interactive-communications/delivery-of-web-channel-document-tutorial-use.md)

登入AEM Forms。

* 導覽至Forms ->資料整合

* 在編輯模式中開啟RetilationAccountStatement資料模型。

* 選取餘額物件，然後按一下編輯按鈕。

* 選取「鉛筆」圖示，在編輯模式中開啟id引數。

* 將繫結變更為「RequestAttribute」。

* 在繫結值中設定帳號，如下所示。

* 這樣，我們就能將accountnumber透過請求屬性傳至表單資料模型

* 請務必儲存變更。
  ![fdm](assets/requestattribute.gif)

## 測試Web Channel檔案的電子郵件傳送 {#test-email-delivery-of-web-channel-document}

* [使用封裝管理程式安裝範例資產](assets/webchanneldelivery.zip)
* [登入crx](http://localhost:4502/crx/de/index.jsp#)

* 導覽至/home/users

* 在使用者的節點下搜尋管理員使用者。

* 選取管理員使用者的設定檔節點。

* 建立名為「accountnumber」的屬性。 確定屬性型別為字串。

* 將此accountnumber屬性的值設為「3059827」。 您可以視需要將此值設定為任何隨機數字。

* [開啟getad.html](http://localhost:4502/content/getad.html)

* 與此URL相關聯的程式碼將取得登入使用者的帳號。 然後，此帳號會作為requestattribute傳遞至FDM。 然後，FDM將擷取與此帳號關聯的資料，並填入Web Channel檔案。

>[!NOTE]
>
>請檢視 **/apps/AEMForms/fetchad/GET.jsp** crx中的檔案。 請確定字串變數webChannelDocument指向有效的通訊檔案路徑。

## 後續步驟

[設定電子郵件傳遞](../interactive-communications/delivery-of-web-channel-document-tutorial-use.md)