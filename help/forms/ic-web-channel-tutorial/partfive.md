---
title: 建立檔案片段以保存收件者名稱和位址
seo-title: 建立檔案片段以保存收件者名稱和位址
description: '這是建立您第一個互動式通訊檔案的多步驟教學課程第5部分。 在本部分，我們將建立文檔片段以保存收件人的名稱和地址。 '
seo-description: '這是建立您第一個互動式通訊檔案的多步驟教學課程第5部分。 在本部分，我們將建立文檔片段以保存收件人的名稱和地址。 '
uuid: 689931e4-a026-4e62-9acd-552918180819
feature: 互動式通訊
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 404eed65-ec55-492a-85b5-59773896b217
topic: 開發
role: 開發人員
level: 初學者
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 1%

---


# 建立文檔片段以保存收件人名稱和地址{#creating-document-fragments-to-hold-the-recipient-name-and-address}

在本部分，我們將建立文檔片段以保存收件人的名稱和地址。

>[!VIDEO](https://video.tv.adobe.com/v/22350/?quality=9&learn=on)

檔案片段可容納互動式通訊檔案的文字內容。 此文本內容可以是靜態文本或從基礎資料模型元素值插入。 例如，「親愛的{name}」，其中「親愛的」是靜態文字，「名稱」是表單資料元素名稱。 在執行時期，這會依名稱元素的值而解析給親愛的Gloria Rios或親愛的John Jacobs。

富格文字編輯器的直覺性足夠讓商業使用者編寫文字並插入表單資料元素。 檔案片段編輯器可設定文字格式、指定字型類型和樣式、插入特殊字元並建立超連結。

檔案片段編輯器還能將內嵌條件插入文字中，如本[影片](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)所示

>[!NOTE]
>
>請確定您插入檔案片段的表單資料模型元素是根元素的後代。 例如，在此使用案例中，請確定您選擇的用戶對象的元素是餘額對象的子項

