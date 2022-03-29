---
title: 用於調試SDK的其AEM他工具
description: 其他多種工具可幫助調試AEMSDK的本地快速啟動。
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 11fb83e9-dbaf-46e5-8102-ae8cc716c6ba
source-git-commit: 467b0c343a28eb573498a013b5490877e4497fe0
workflow-type: tm+mt
source-wordcount: '555'
ht-degree: 1%

---

# 用於調試SDK的其AEM他工具

多種其他工具可幫助在SDK的本地快速啟動AEM上調試應用程式。

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Lite是一個基於Web的介面，用於與JCR（資料儲存庫）AEM交互。 CRXDE Lite提供對JCR的完全可見性，包括節點、屬性、屬性值和權限。

CRXDE Lite位於：

+ 工具>常規>CRXDE Lite
+ 或 [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

### 調試內容

CRXDE Lite可直接訪問JCR。 通過CRXDE Lite可見的內容受授予用戶的權限限制，這意味著您可能無法根據您的訪問權限查看或修改JCR中的所有內容。

+ 使用左導航窗格導航和操作JCR結構
+ 在左側導航窗格中選擇一個節點，將在底部窗格中顯示節點屬性。
   + 可以從窗格中添加、刪除或更改屬性
+ 按兩下左導航中的檔案節點，在右上窗格中開啟檔案的內容
+ 按一下左上角的「Save All（全部保存）」按鈕以保持更改，或按一下「Save All to Revert any unsaved changes（全部保存以還原所有未保存的更改）」旁邊的下箭頭。

![CRXDE Lite — 調試內容](./assets/other-tools/crxde-lite__debugging-content.png)

通過CRXDE Lite直接對AEMSDK進行的任何更改都可能難以跟蹤和管理。 確保通過CRXDE Lite進行的更改返回到項目AEM的可變內容包(`ui.content`)，並致力於Git。 理想情況下，所有應用程式內容更改都源於代碼庫，並通過部署流AEM入SDK，而不是通過CRXDE Lite直接對AEMSDK進行更改。

### 調試訪問控制

CRXDE Lite提供了test和評估特定用戶或組（又稱主體）的特定節點上的訪問控制的方法。

要在CRXDE Lite中訪問Test訪問控制控制台，請導航到：

+ CRXDE Lite>工具>Test訪問控制……

![CRXDE Lite-Test訪問控制](./assets/other-tools/crxde-lite__test-access-control.png)

1. 使用「路徑」欄位，選擇要計算的JCR路徑
1. 使用「承擔者」(Principal)欄位，選擇用戶或組以根據
1. 點擊Test按鈕

結果顯示如下：

+ __路徑__ 重申已評估的路徑
+ __主__ 重申已評估路徑的用戶或組
+ __承擔者__ 列出選定承擔者所屬的所有承擔者。
   + 這有助於瞭解可通過繼承提供權限的傳遞組成員資格
+ __路徑上的權限__ 列出所選主體在評估路徑上擁有的所有JCR權限

## 說明查詢

![說明查詢](./assets/other-tools/explain-query.png)

在AEMSDK的本地快速啟動中解釋基於Web的查詢工具，該工具提供解釋和執行查詢AEM的關鍵見解，並且是確保以效能方式執行查詢的非常有價值的工AEM具。

解釋查詢位於：

+ 「工具」>「診斷」>「查詢效能」>「解釋查詢」頁籤
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) >解釋查詢頁籤

## QueryBuilder調試器

![QueryBuilder調試器](./assets/other-tools/query-debugger.png)

QueryBuilder調試器是基於Web的工具，可幫助您使用Web [查詢生成器](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html) 語法。

QueryBuilder調試器位於：

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)
