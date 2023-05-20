---
title: 在中設定翻譯規AEM則
description: 翻譯配置UI允許用戶管理用於翻譯AEM Sites內容的規則。 此視頻詳細介紹了為自定義元件建立新轉換規則的過程。
feature: Language Copy
topics: localization, content-architecture
audience: developer, administrator
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: Localization
role: User
level: Beginner
exl-id: 359da531-839c-4680-abf9-c880cc700159
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '321'
ht-degree: 6%

---

# 設定轉換規則 {#set-up-translation-rules-in-aem}

翻譯配置UI允許用戶管理用於翻譯AEM Sites內容的規則。 此視頻詳細介紹了為自定義元件建立新轉換規則的過程。

>[!NOTE]
>
> 6.3錄AEM制了以下視頻。AEM 6.4+引入了新的儲存庫結構來儲存轉換規則XML檔案。 在6.4+中使用「轉換配置」UIAEM時，規則將保存到位置 `/conf/global/settings/translation/rules/translation_rules.xml`。 請參閱 [確定要翻譯的內容](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html) 的子菜單。

>[!VIDEO](https://video.tv.adobe.com/v/18135?quality=12&learn=on)

翻譯規則標識要AEM提取的內容以供翻譯。 現成翻譯規則涵蓋常見用例，如「文本」元件和「影像」元件的「替代文字」。 根據項目的翻譯要求，可能需要其他規則。 一般來說，翻譯規則允許用戶指定：

1. 應根據路徑和/或資源類型進行轉換的屬性
2. 不應轉換的屬性的篩選器
3. 應翻譯的引用內容（即影像或內容片段）

將更新轉換xml檔案的轉換規則編輯器。 翻譯配置UI使管理各種翻譯規則和防止直接編輯XML時出現拼寫錯誤變得更加容易。

訪問翻譯配置UI:

* **[!UICONTROL 開始AEM菜單] > [!UICONTROL 工具] > [!UICONTROL 常規] > [[!UICONTROL 翻譯配置]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**

## 6AEM.3之前 {#prior-to-aem}

在以前AEM的版本轉換規則是通過編輯「轉換」工作流下的XML檔案來手動更新的： `/etc/workflow/models/translation/translation_rules.xml`。

## 其他資源 {#additional-resources}

* [識別要翻譯的內容](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html)
* [翻譯多語言網站的內容](https://helpx.adobe.com/tw/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [翻譯最佳實務](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)
