---
title: 建立檔案片段
description: 這是建立第一個互動式通訊檔案的多步驟教學課程的第5部分。 在本部分中，我們將建立檔案片段以儲存收件者名稱和地址。
feature: Interactive Communication
doc-type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
discoiquuid: 47d3aa97-0bff-48e0-8a65-55e5332f811b
jira: KT-5958
thumbnail: 22350.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 2fe3f950-bc2a-4e91-8d91-00438691727a
duration: 217
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '224'
ht-degree: 0%

---

# 建立檔案片段

在本部分中，我們將建立檔案片段以儲存收件者名稱和地址。

>[!VIDEO](https://video.tv.adobe.com/v/22350?quality=12&learn=on)

檔案片段包含互動式通訊檔案的文字內容。 此文字內容可以是靜態文字，或從基礎資料模型元素值插入。 例如&#x200B;**親愛的&#x200B;_{name}_**，其中Dear是靜態文字，而name是表單資料模型元素名稱。 在執行階段，這將解析為&#x200B;**Gloria Rios**&#x200B;或&#x200B;**John Jacobs**，視名稱元素的值而定。

RTF編輯器非常直覺，可供業務使用者編寫文字及插入表單資料元素。 檔案片段編輯器可以設定文字格式、指定字型型別和樣式、插入特殊字元和建立超連結。

檔案片段編輯器也能插入文字中的內嵌條件，如此[影片](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)所示

>[!NOTE]
>
>請確定您插入到檔案片段中的表單資料模型元素是根元素的子代。 例如，在此使用案例中，請確定您選取的使用者物件元素是餘額物件的子項

## 後續步驟

[建立列印管道檔案](./create-print-channel-document.md)
