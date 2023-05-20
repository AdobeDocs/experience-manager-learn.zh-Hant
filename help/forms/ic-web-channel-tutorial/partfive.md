---
title: 建立文檔片段以保存收件人名稱和地址
seo-title: Creating Document Fragments to hold the recipient name and address
description: 這是建立第一個互動式通信文檔的多步教程的第5部分。 在此部分，我們將建立文檔片段以保存收件人名稱和地址。
seo-description: This is part 5 of a multi-step tutorial for creating your first interactive communications document. In this part, we will create document fragment to hold the recipient name and address.
uuid: 689931e4-a026-4e62-9acd-552918180819
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 404eed65-ec55-492a-85b5-59773896b217
topic: Development
role: Developer
level: Beginner
exl-id: 1d7093a8-3765-46ec-912a-b5a5503fd5af
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '244'
ht-degree: 0%

---

# 建立文檔片段以保存收件人名稱和地址 {#creating-document-fragments-to-hold-the-recipient-name-and-address}

在此部分，我們將建立文檔片段以保存收件人名稱和地址。

>[!VIDEO](https://video.tv.adobe.com/v/22350?quality=12&learn=on)

文檔片段保存互動式通信文檔的文本內容。 此文本內容可以是靜態文本或從基礎資料模型元素值插入。 例如，Dear {name}，其中Dear是靜態文本，{name}是表單資料元素名稱。 在運行時，這將根據名稱元素的值解析給親愛的Gloria Rios或親愛的John Jacobs。

富格文本編輯器足夠直觀，使業務用戶能夠編寫文本並插入表單資料元素。 文檔片段編輯器能夠設定文本格式、指定字型類型和樣式、插入特殊字元和建立超連結。

文檔片段編輯器還能夠在文本中插入內聯條件，如本中所示 [視頻](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>確保插入到文檔片段中的「表單資料模型」元素是根元素的後代。 例如，在此使用情形中，請確保您選擇的用戶對象的元素是餘額對象的子項

## 後續步驟

[建立互動式通信文檔](./partsix.md)