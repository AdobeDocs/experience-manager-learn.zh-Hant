---
title: 建立檔案片段以保留收件者名稱和地址
description: 這是建立第一個互動式通訊檔案的多步驟教學課程的第5部分。 在本部分中，我們將建立檔案片段以儲存收件者名稱和地址。
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 404eed65-ec55-492a-85b5-59773896b217
topic: Development
role: Developer
level: Beginner
exl-id: 1d7093a8-3765-46ec-912a-b5a5503fd5af
duration: 229
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 0%

---

# 建立檔案片段以保留收件者名稱和地址 {#creating-document-fragments-to-hold-the-recipient-name-and-address}

在本部分中，我們將建立檔案片段以儲存收件者名稱和地址。

>[!VIDEO](https://video.tv.adobe.com/v/22350?quality=12&learn=on)

檔案片段包含互動式通訊檔案的文字內容。 此文字內容可以是靜態文字，或從基礎資料模型元素值插入。 例如，親愛的 {name}，其中Dear是靜態文字和 {name} 是表單資料元素名稱。 在執行階段，這將根據name元素的值解析為Dear Gloria Rios或Dear John Jacobs。

RTF編輯器是足夠直覺的，可供業務使用者編寫文字及插入表單資料元素。 檔案片段編輯器可以設定文字格式、指定字型型別和樣式、插入特殊字元和建立超連結。

檔案片段編輯器也可以在您的文字中插入內嵌條件，如以下所示 [視訊](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>請確定您插入到檔案片段中的表單資料模型元素是根元素的子代。 例如，在此使用案例中，請確定您選取的使用者物件元素是餘額物件的子項

## 後續步驟

[建立互動式通訊檔案](./partsix.md)