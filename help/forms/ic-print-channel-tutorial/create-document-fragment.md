---
title: 建立檔案片段
description: 這是建立第一個互動式通訊檔案的多步驟教學課程的第5部分。 在本部分中，我們將建立檔案片段以儲存收件者名稱和地址。
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 47d3aa97-0bff-48e0-8a65-55e5332f811b
jira: KT-5958
thumbnail: 22350.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 2fe3f950-bc2a-4e91-8d91-00438691727a
duration: 235
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '224'
ht-degree: 0%

---

# 建立檔案片段

在本部分中，我們將建立檔案片段以儲存收件者名稱和地址。

>[!VIDEO](https://video.tv.adobe.com/v/22350?quality=12&learn=on)

檔案片段包含互動式通訊檔案的文字內容。 此文字內容可以是靜態文字，或從基礎資料模型元素值插入。 例如 **親愛的 _{name}_**，其中Dear是靜態文字，name是表單資料模型元素名稱。 在執行階段，這將會解析為&#x200B;**親愛的Gloria Rios**或&#x200B;**親愛的約翰·雅各布斯**視name元素的值而定。

RTF編輯器非常直覺，可供業務使用者編寫文字及插入表單資料元素。 檔案片段編輯器可以設定文字格式、指定字型型別和樣式、插入特殊字元和建立超連結。

檔案片段編輯器也可以在您的文字中插入內嵌條件，如以下所示 [視訊](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>請確定您插入到檔案片段中的表單資料模型元素是根元素的子代。 例如，在此使用案例中，請確定您選取的使用者物件元素是餘額物件的子項

## 後續步驟

[建立列印管道檔案](./create-print-channel-document.md)
