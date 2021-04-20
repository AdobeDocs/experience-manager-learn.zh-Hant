---
title: 建立檔案片段
description: '這是建立您第一個互動式通訊檔案的多步驟教學課程第5部分。 在本部分，我們將建立文檔片段以保存收件人的名稱和地址。 '
seo-description: '這是建立您第一個互動式通訊檔案的多步驟教學課程第5部分。 在本部分，我們將建立文檔片段以保存收件人的名稱和地址。 '
uuid: 7fd8a0f2-a921-4e70-91c9-908dae9aeab2
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 47d3aa97-0bff-48e0-8a65-55e5332f811b
kt: 5958
thumbnail: 22350.jpg
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 1%

---


# 建立檔案片段

在本部分，我們將建立文檔片段以保存收件人的名稱和地址。

>[!VIDEO](https://video.tv.adobe.com/v/22350/?quality=9&learn=on)

檔案片段可容納互動式通訊檔案的文字內容。 此文本內容可以是靜態文本或從基礎資料模型元素值插入。 例如&#x200B;**親愛的&#x200B;_{name}_**，其中「親愛的」是靜態文字，「名稱」是表單資料模型元素名稱。 在執行時期，這將會解析為&#x200B;**親愛的Gloria Rios**或&#x200B;**親愛的John Jacobs**，視名稱元素的值而定。

豐富式文字編輯器具備足夠的直覺性，讓商業使用者能夠製作文字並插入表單資料元素。 檔案片段編輯器可設定文字格式、指定字型類型和樣式、插入特殊字元並建立超連結。

檔案片段編輯器也能將內嵌條件插入文字，如本[影片](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)所示

>[!NOTE]
>
>請確定您插入檔案片段的表單資料模型元素是根元素的後代。 例如，在此使用案例中，請確定您選擇的用戶對象的元素是餘額對象的子項

