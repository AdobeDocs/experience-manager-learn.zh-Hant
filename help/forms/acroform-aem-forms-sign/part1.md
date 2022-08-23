---
title: AEM Forms
seo-title: Merge Adaptive Form data with Acroform
description: Acroforms和AEM Forms的整合。 使用Acroform建立自適應表單並合併資料以獲得PDF。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 0%

---


# 建立頂層

頂體是使用Acrobat建立的形式。 您可以使用Acrobat從頭建立新表單，或者使用在Microsoft Word中建立的現有表單，然後使用Acrobat將其轉換為Acroform。 需要執行以下步驟才能將在MicrosoftWord中建立的表單轉換為Acroform。

* 使用Acrobat的Open Word文檔
* 使用Acrobat準備表單工具標識表單上的表單域。
* 保存pdf。 確保檔案名中沒有空格。


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=9&learn=on)

>[!NOTE]
>
>如果要發送可填充的頂層表單以使用Adobe Sign進行簽名，請相應地命名欄位。 例如，您可以命名欄位 **簽名_:signer1:簽名**。 這是Adobe Sign所理解的語法。

>[!NOTE]
>
>如果發送的是基於XFA的文檔，則需要拼合文檔，而且Adobe Sign簽名標籤需要作為靜態文本出現在文檔中。

[Adobe Sign文本標籤文檔](https://helpx.adobe.com/sign/using/text-tag.html)

>[!NOTE]
>
>確保頂層檔案名中沒有任何空格。 當前示例代碼不處理空格。
>
>表單域名稱只能包含以下內容：
>
>* 單空間
>* 單下划線
>* 字母數字字元

