---
title: 使用AEM Forms的Acroforms
description: 將Acroforms與AEM Forms整合的第1部分。 使用Acroform建立最適化表單並合併資料以取得PDF。
feature: Adaptive Forms
doc-type: Tutorial
version: Experience Manager 6.5
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 144
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 0%

---


# 建立Acroform

Acroform是使用Acrobat建立的表單。 您可以使用Acrobat從頭開始建立新表單，或採用在Microsoft Word中建立的現有表單，並使用Acrobat將其轉換為Acroform。 您必須依照下列步驟，將在Microsoft Word中建立的表單轉換為Acroform。

* 使用Acrobat開啟Word檔案
* 使用Acrobat準備表單工具來識別表單上的表單欄位。
* 儲存pdf。 請確認檔案名稱不含任何空格。


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=12&learn=on)

>[!NOTE]
>
>如果您想要傳送可填寫的Acroform以供使用Acrobat Sign簽署，請為欄位命名。 例如，您可以命名欄位&#x200B;**`Sig_es_:signer1:signature`**。 這是Acrobat Sign瞭解的語法。

>[!NOTE]
>
>如果您要傳送以XFA為基礎的檔案，則需要將檔案平面化，且Acrobat Sign簽名標籤必須以靜態文字的形式顯示在檔案中。

[Acrobat Sign文字標籤檔案](https://helpx.adobe.com/sign/using/text-tag.html)

>[!NOTE]
>
>請確定Acroform檔案名稱不含任何空格。 目前的範常式式碼不處理空格。
>
>表單欄位名稱只能包含下列內容：
>
>* 單一空間
>* 單底線
>* 英數字元
