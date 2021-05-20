---
title: 建立檔案片段以保留收件者名稱和地址
seo-title: 建立檔案片段以保留收件者名稱和地址
description: '這是建立第一個互動式通訊檔案的多步驟教學課程的第5部分。 在本部分，我們將建立文檔片段以保存收件人名稱和地址。 '
seo-description: '這是建立第一個互動式通訊檔案的多步驟教學課程的第5部分。 在本部分，我們將建立文檔片段以保存收件人名稱和地址。 '
uuid: 689931e4-a026-4e62-9acd-552918180819
feature: 互動式通訊
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 404eed65-ec55-492a-85b5-59773896b217
topic: 開發
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 1%

---


# 建立文檔片段以保存收件者姓名和地址{#creating-document-fragments-to-hold-the-recipient-name-and-address}

在本部分，我們將建立文檔片段以保存收件人名稱和地址。

>[!VIDEO](https://video.tv.adobe.com/v/22350/?quality=9&learn=on)

文檔片段保存互動式通信文檔的文本內容。 此文本內容可以是靜態文本，也可以從基礎資料模型元素值插入。 例如，「親愛的{name}」，其中「親愛的」是靜態文本，「名稱」是表單資料元素名稱。 在執行階段中，這會根據名稱元素的值，解析給親愛的Gloria Rios或親愛的John Jacobs。

RTF編輯器的直覺性足以讓商務使用者製作文字和插入表單資料元素。 文檔片段編輯器能夠格式化文本、指定字型類型和樣式、插入特殊字元和建立超連結。

文檔片段編輯器還能夠在文本中插入內嵌條件，如本[video](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)所示

>[!NOTE]
>
>請確定您插入檔案片段的表單資料模型元素是根元素的後代。 例如，在此使用案例中，請確保所選用戶對象的元素是餘額對象的子項

