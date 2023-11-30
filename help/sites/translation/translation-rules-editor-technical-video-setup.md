---
title: 在AEM中設定翻譯規則
description: 翻譯設定UI可讓使用者管理在AEM Sites中翻譯內容的規則。 此影片詳細說明如何為自訂元件建立新的翻譯規則。
feature: Language Copy
version: 6.4, 6.5
topic: Localization
role: User
level: Beginner
doc-type: Technical Video
exl-id: 359da531-839c-4680-abf9-c880cc700159
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '321'
ht-degree: 6%

---

# 設定翻譯規則 {#set-up-translation-rules-in-aem}

翻譯設定UI可讓使用者管理在AEM Sites中翻譯內容的規則。 此影片詳細說明如何為自訂元件建立新的翻譯規則。

>[!NOTE]
>
> 以下影片是在AEM 6.3上錄製的。AEM 6.4+引進了儲存翻譯規則XML檔案的新存放庫結構。 在AEM 6.4+中使用翻譯設定UI時，規則會儲存至該位置 `/conf/global/settings/translation/rules/translation_rules.xml`. 另請參閱 [識別要翻譯的內容](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html) 以取得更多詳細資料。

>[!VIDEO](https://video.tv.adobe.com/v/18135?quality=12&learn=on)

翻譯規則會識別AEM中要擷取以進行翻譯的內容。 現成的翻譯規則涵蓋常見的使用案例，例如文字元件和影像元件的替代文字。 根據專案翻譯要求，可能需要其他規則。 一般而言，翻譯規則允許使用者指定：

1. 應根據路徑和/或資源型別轉譯的屬性
2. 不應轉譯之屬性的篩選器
3. 應翻譯的參考內容（即影像或內容片段）

將更新翻譯xml檔案的翻譯規則編輯器。 翻譯設定UI可讓您更輕鬆地管理各種翻譯規則，並在直接編輯XML時防止出現拼寫錯誤。

存取翻譯設定UI：

* **[!UICONTROL AEM開始功能表] > [!UICONTROL 工具] > [!UICONTROL 一般] > [[!UICONTROL 翻譯設定]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**

## AEM 6.3之前 {#prior-to-aem}

在舊版AEM中，透過編輯位於翻譯工作流程下的XML檔案來手動更新翻譯規則： `/etc/workflow/models/translation/translation_rules.xml`.

## 其他資源 {#additional-resources}

* [識別要翻譯的內容](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html)
* [翻譯多語言網站的內容](https://helpx.adobe.com/tw/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [翻譯最佳做法](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)
