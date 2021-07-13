---
title: 在AEM中設定翻譯規則
description: 翻譯設定UI可讓使用者管理AEM Sites中轉譯內容的規則。 此影片詳細說明如何建立自訂元件的新翻譯規則。
feature: 語言副本
topics: localization, content-architecture
audience: developer, administrator
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: 本土化
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 3%

---


# 設定翻譯規則 {#set-up-translation-rules-in-aem}

翻譯設定UI可讓使用者管理AEM Sites中轉譯內容的規則。 此影片詳細說明如何建立自訂元件的新翻譯規則。

>[!NOTE]
>
> 以下影片已記錄在AEM 6.3中。AEM 6.4+推出新的存放庫結構，可儲存翻譯規則XML檔案。 使用AEM 6.4+中的翻譯設定UI時，規則會儲存至`/conf/global/settings/translation/rules/translation_rules.xml`位置。 如需詳細資訊，請參閱[識別要翻譯的內容](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html) 。

>[!VIDEO](https://video.tv.adobe.com/v/18135/?quality=9&learn=on)

翻譯規則會識別AEM中要擷取的內容，以供翻譯。 現成可用的翻譯規則涵蓋常見的使用案例，例如「文字」元件和「影像」元件的替代文字。 視專案翻譯需求而定，可能需要其他規則。 一般翻譯規則可讓使用者指定：

1. 應根據路徑和/或資源類型轉譯的屬性
2. 不應翻譯屬性的篩選器
3. 應翻譯的參考內容（例如影像或內容片段）

將更新翻譯xml檔案的翻譯規則編輯器。 翻譯設定UI可讓您更輕鬆地管理各種翻譯規則，並防止直接編輯XML時出現錯字。

訪問翻譯配置UI:

* **[!UICONTROL AEM開始功能表]  >  [!UICONTROL 工具]  >  [!UICONTROL 一般]  >  [[!UICONTROL 翻譯設定]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**

## AEM 6.3之前 {#prior-to-aem}

在先前的AEM版本翻譯規則中，是透過編輯位於「翻譯」工作流程下的XML檔案來手動更新：`/etc/workflow/models/translation/translation_rules.xml`。

## 其他資源 {#additional-resources}

* [識別要翻譯的內容](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html)
* [轉譯多語言網站的內容](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [翻譯最佳實務](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)
