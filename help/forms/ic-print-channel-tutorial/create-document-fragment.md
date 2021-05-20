---
title: 建立檔案片段
description: '這是建立第一個互動式通訊檔案的多步驟教學課程的第5部分。 在本部分，我們將建立文檔片段以保存收件人名稱和地址。 '
seo-description: '這是建立第一個互動式通訊檔案的多步驟教學課程的第5部分。 在本部分，我們將建立文檔片段以保存收件人名稱和地址。 '
uuid: 7fd8a0f2-a921-4e70-91c9-908dae9aeab2
feature: 互動式通訊
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 47d3aa97-0bff-48e0-8a65-55e5332f811b
kt: 5958
thumbnail: 22350.jpg
topic: 開發
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 1%

---


# 建立檔案片段

在本部分，我們將建立文檔片段以保存收件人名稱和地址。

>[!VIDEO](https://video.tv.adobe.com/v/22350/?quality=9&learn=on)

文檔片段保存互動式通信文檔的文本內容。 此文本內容可以是靜態文本，也可以從基礎資料模型元素值插入。 例如，**親愛的&#x200B;_{name}_**，其中「親愛的」是靜態文本，「名稱」是表單資料模型元素名稱。 在執行階段，這會根據名稱元素的值，解析為&#x200B;**親愛的Gloria Rios**或&#x200B;**親愛的John Jacobs**。

RTF編輯器的直覺性足以讓商務使用者製作文字和插入表單資料元素。 文檔片段編輯器能夠格式化文本、指定字型類型和樣式、插入特殊字元和建立超連結。

文檔片段編輯器還能夠在文本中插入內嵌條件，如本[video](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)所示

>[!NOTE]
>
>請確定您插入檔案片段的表單資料模型元素是根元素的後代。 例如，在此使用案例中，請確保所選用戶對象的元素是餘額對象的子項

