---
title: 建立文檔片段
description: 這是建立第一個互動式通信文檔的多步教程的第5部分。 在此部分，我們將建立文檔片段以保存收件人名稱和地址。
seo-description: This is part 5 of a multi-step tutorial for creating your first interactive communications document. In this part, we will create document fragment to hold the recipient name and address.
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
exl-id: 2fe3f950-bc2a-4e91-8d91-00438691727a
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 0%

---

# 建立文檔片段

在此部分，我們將建立文檔片段以保存收件人名稱和地址。

>[!VIDEO](https://video.tv.adobe.com/v/22350?quality=12&learn=on)

文檔片段保存互動式通信文檔的文本內容。 此文本內容可以是靜態文本或從基礎資料模型元素值插入。 例如 **親愛的 _{name}_**，其中Dear為靜態文本，name為表單資料模型元素名稱。 在運行時，此操作將解析為&#x200B;**親愛的格洛麗亞·里奧斯**或&#x200B;**親愛的約翰·雅各布斯**取決於name元素的值。

富格文本編輯器足夠直觀，使業務用戶能夠編寫文本並插入表單資料元素。 文檔片段編輯器能夠設定文本格式、指定字型類型和樣式、插入特殊字元和建立超連結。

文檔片段編輯器還能夠在文本中插入內聯條件，如本中所示 [視頻](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>確保插入到文檔片段中的「表單資料模型」元素是根元素的後代。 例如，在此使用情形中，請確保您選擇的用戶對象的元素是餘額對象的子項

## 後續步驟

[建立打印渠道文檔](./create-print-channel-document.md)
