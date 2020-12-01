---
title: 在AEM中設定翻譯規則
description: 「轉譯設定UI」可讓使用者管理在AEM網站中轉譯內容的規則。 此影片詳細說明如何為自訂元件建立新的轉譯規則。
feature: language-copy
topics: localization, content-architecture
audience: developer, administrator
doc-type: technical video
activity: setup
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '321'
ht-degree: 0%

---


# 設定翻譯規則{#set-up-translation-rules-in-aem}

「轉譯設定UI」可讓使用者管理在AEM網站中轉譯內容的規則。 此影片詳細說明如何為自訂元件建立新的轉譯規則。

>[!NOTE]
>
> AEM 6.3已錄制下列視訊。AEM 6.4+推出新的儲存庫結構，以儲存轉譯規則XML檔案。 在AEM 6.4+中使用「轉譯設定UI」時，規則會儲存至`/conf/global/settings/translation/rules/translation_rules.xml`位置。 如需詳細資訊，請參閱[識別要翻譯的內容。](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html)

>[!VIDEO](https://video.tv.adobe.com/v/18135/?quality=9&learn=on)

轉譯規則會識別AEM中要擷取以供轉譯的內容。 立即可用的轉換規則涵蓋常見的使用案例，例如「文字」元件和「影像」元件的替代文字。 視專案轉譯需求而定，可能需要其他規則。 一般而言，轉換規則可讓使用者指定：

1. 應根據路徑和／或資源類型轉換的屬性
2. 不應翻譯的屬性篩選
3. 應翻譯的參考內容（即影像或內容片段）

將更新翻譯xml檔案的翻譯規則編輯器。 翻譯配置UI可讓您更輕鬆地管理各種翻譯規則，並防止直接編輯XML時出現錯字。

訪問翻譯配置UI:

* **[!UICONTROL AEM Start Menu] >  [!UICONTROL Tools] >  [!UICONTROL General]  [[!UICONTROL > Translation Configuration]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**

## AEM 6.3 {#prior-to-aem}之前版本

在先前的AEM版本轉譯規則中，是透過編輯位於「轉譯」工作流程下的XML檔案來手動更新：`/etc/workflow/models/translation/translation_rules.xml`。

## 其他資源 {#additional-resources}

* [識別要翻譯的內容](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html)
* [翻譯多語言網站的內容](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [翻譯最佳做法](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)
