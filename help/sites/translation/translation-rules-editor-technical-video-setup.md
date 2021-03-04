---
title: 設定轉換規則於
description: 翻譯配置UI允許用戶管理在AEM Sites翻譯內容的規則。 此影片詳細說明如何為自訂元件建立新的轉譯規則。
feature: 語言副本
topics: localization, content-architecture
audience: developer, administrator
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: 本土化
role: 業務從業人員
level: 初學者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 3%

---


# 設定翻譯規則{#set-up-translation-rules-in-aem}

翻譯配置UI允許用戶管理在AEM Sites翻譯內容的規則。 此影片詳細說明如何為自訂元件建立新的轉譯規則。

>[!NOTE]
>
> 以下視訊已錄AEM制於6.3。AEM 6.4+引入了新的儲存庫結構，用於儲存翻譯規則XML檔案。 在6.4+中使用翻譯配置UIAEM時，規則將保存到`/conf/global/settings/translation/rules/translation_rules.xml`位置。 如需詳細資訊，請參閱[識別要翻譯的內容。](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html)

>[!VIDEO](https://video.tv.adobe.com/v/18135/?quality=9&learn=on)

翻譯規則可識別要AEM提取以供翻譯的內容。 立即可用的轉換規則涵蓋常見的使用案例，例如「文字」元件和「影像」元件的替代文字。 視專案轉譯需求而定，可能需要其他規則。 一般而言，轉換規則可讓使用者指定：

1. 應根據路徑和／或資源類型轉換的屬性
2. 不應翻譯的屬性篩選
3. 應翻譯的參考內容（即影像或內容片段）

將更新翻譯xml檔案的翻譯規則編輯器。 翻譯配置UI可讓您更輕鬆地管理各種翻譯規則，並防止直接編輯XML時出現錯字。

訪問翻譯配置UI:

* **[!UICONTROL AEM開始菜單] >工 [!UICONTROL 具] >一 [!UICONTROL 般] >轉 [[!UICONTROL 譯配置]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**

## 6.AEM3之前版本{#prior-to-aem}

在舊版AEM翻譯規則中，可以通過編輯位於「翻譯」工作流程下的XML檔案來手動更新：`/etc/workflow/models/translation/translation_rules.xml`。

## 其他資源 {#additional-resources}

* [識別要翻譯的內容](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html)
* [翻譯多語言網站的內容](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [翻譯最佳做法](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)
