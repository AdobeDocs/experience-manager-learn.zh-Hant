---
title: 自訂文字編輯器
seo-title: Customize text editor
description: 了解如何自訂文字編輯器。
seo-description: Learn how to customize text editor.
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Beginner
feature: Interactive Communication
last-substantial-update: 2023-04-19T00:00:00Z
kt: 13126
source-git-commit: edba74f5ff5611687c05812de184243997ee7a35
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 0%

---

# 自訂文字編輯器{#customize-text-editor}

## 概觀 {#overview}

您可以自訂用於建立檔案片段的文字編輯器，以新增更多字型和字型大小。 這些字型包括英文和非英文，如日文字型。

您可以自訂以在字型設定中變更下列項目：

* 字型系列和大小
* 高度和字母間距等屬性
* 字型系列和大小、高度、字母間距和日期格式的預設值
* 項目符號縮進

要執行此操作，您需要：

1. [在CRX中編輯tbxeditor-config.xml檔案以自訂字型](#customizefonts)
1. [將自定義字型添加到客戶端電腦](#addcustomfonts)

## 在CRX中編輯tbxeditor-config.xml檔案以自訂字型 {#customizefonts}

要通過編輯tbxeditor-config.xml檔案來自定義字型，請執行以下操作：

1. 前往 `https://'[server]:[port]'/[ContextPath]/crx/de` 並以管理員身分登入。
1. 在應用程式資料夾中，使用類似設定資料夾（位於libs/fd/cm/config）的路徑/結構，建立名為config的資料夾，步驟如下：

   1. 按一下右鍵以下路徑上的項資料夾並選擇 **覆蓋節點**:

      `/libs/fd/cm/config`

      ![覆蓋節點](assets/overlay.png)

   1. 確定「覆蓋節點」對話方塊有下列值：

      **路徑：** /libs/fd/cm/config

      **位置：** /apps/

      **匹配節點類型：** 已選取

      ![覆蓋節點](assets/overlay1.png)

   1. 按一下&#x200B;**「確定」**。資料夾結構會在應用程式資料夾中建立。

   1. 按一下 **全部儲存**.

1. 使用下列步驟，在新建立的設定資料夾中建立tbxeditor-config.xml檔案的副本：

   1. 在libs/fd/cm/config按一下右鍵tbxeditor-config.xml檔案，然後選擇 **複製**.
   1. 按一下右鍵以下資料夾並選擇 **貼上：**

      `apps/fd/cm/config`

   1. 貼上的檔案的名稱預設為 `copy of tbxeditor-config.xml.` 將檔案重新命名為 `tbxeditor-config.xml` 按一下 **全部儲存**.

1. 在apps/fd/cm/config處開啟tbxeditor-config.xml檔案，然後進行所需的變更。

   1. 在apps/fd/cm/config處按兩下tbxeditor-config.xml檔案。 檔案隨即開啟。

      ```xml
      <editorConfig>
         <bulletIndent>0.25in</bulletIndent>
      
         <defaultDateFormat>DD-MM-YYYY</defaultDateFormat>
      
         <fonts>
            <default>Times New Roman</default>
            <font>_sans</font>
            <font>_serif</font>
            <font>_typewriter</font>
            <font>Arial</font>
            <font>Courier</font>
            <font>Courier New</font>
            <font>Geneva</font>
            <font>Georgia</font>
            <font>Helvetica</font>
            <font>Tahoma</font>
            <font>Times New Roman</font>
            <font>Times</font>
            <font>Verdana</font>
         </fonts>
      
         <fontSizes>
            <default>12</default>
            <fontSize>8</fontSize>
            <fontSize>9</fontSize>
            <fontSize>10</fontSize>
            <fontSize>11</fontSize>
            <fontSize>12</fontSize>
            <fontSize>14</fontSize>
            <fontSize>16</fontSize>
            <fontSize>18</fontSize>
            <fontSize>20</fontSize>
            <fontSize>22</fontSize>
            <fontSize>24</fontSize>
            <fontSize>26</fontSize>
            <fontSize>28</fontSize>
            <fontSize>36</fontSize>
            <fontSize>48</fontSize>
            <fontSize>72</fontSize>
         </fontSizes>
      
         <lineHeights>
            <default>2</default>     
            <lineHeight>2</lineHeight>
            <lineHeight>3</lineHeight>
            <lineHeight>4</lineHeight>
            <lineHeight>5</lineHeight>
            <lineHeight>6</lineHeight>
            <lineHeight>7</lineHeight>
            <lineHeight>8</lineHeight>
            <lineHeight>9</lineHeight>
            <lineHeight>10</lineHeight>
            <lineHeight>11</lineHeight>
            <lineHeight>12</lineHeight>
            <lineHeight>13</lineHeight>
            <lineHeight>14</lineHeight>
            <lineHeight>15</lineHeight>
            <lineHeight>16</lineHeight>
         </lineHeights>
      
         <letterSpacings>
            <default>0</default>
            <letterSpacing>0</letterSpacing>
            <letterSpacing>1</letterSpacing>
            <letterSpacing>2</letterSpacing>
            <letterSpacing>3</letterSpacing>
            <letterSpacing>4</letterSpacing>
            <letterSpacing>5</letterSpacing>
            <letterSpacing>6</letterSpacing>
            <letterSpacing>7</letterSpacing>
            <letterSpacing>8</letterSpacing>
            <letterSpacing>9</letterSpacing>
            <letterSpacing>10</letterSpacing>
            <letterSpacing>11</letterSpacing>
            <letterSpacing>12</letterSpacing>
            <letterSpacing>13</letterSpacing>
            <letterSpacing>14</letterSpacing>
            <letterSpacing>15</letterSpacing>
            <letterSpacing>16</letterSpacing>
         </letterSpacings>
      </editorConfig>
      ```

   1. 在檔案中進行所需的變更，以變更字型設定中的下列項目：

      * 添加或刪除字型系列和大小
      * 高度和字母間距等屬性
      * 字型系列和大小、高度、字母間距和日期格式的預設值
      * 項目符號縮進

      例如，要添加名為Sazanami Mincho Medium的日文字型，需要在XML檔案中進行以下條目： `<font>Sazanami Mincho Medium</font>`. 您還需要在用於存取和使用字型自訂的用戶端電腦上安裝此字型。 如需詳細資訊，請參閱 [將自定義字型添加到客戶端電腦](#addcustomfonts).

      您也可以更改文本各個方面的預設值，並通過刪除條目，從文本編輯器中刪除字型。

   1. 按一下 **全部儲存**.


## 將自定義字型添加到客戶端電腦 {#addcustomfonts}

當您在互動式通訊文字編輯器中存取字型時，字型必須存在於您用來存取互動式通訊的用戶端電腦中。 若要在文字編輯器中使用自訂字型，您必須先在用戶端電腦上安裝相同的字型。

有關安裝字型的詳細資訊，請參閱以下內容：

* [在Windows上安裝或卸載字型](https://windows.microsoft.com/en-us/windows-vista/install-or-uninstall-fonts)
* [Mac基本介紹：字型簿](https://support.apple.com/en-us/HT201749)

## 訪問字型自定義 {#access-font-customizations}

在CRX中對tbxeditor-config.xml檔案中的字型進行變更，並在用於存取AEM Forms的用戶端電腦上安裝所需字型後，變更會顯示在文字編輯器中。

例如，在 [在CRX中編輯tbxeditor-config.xml檔案以自訂字型](#customizefonts) 過程在文本編輯器UI中顯示如下：

![sanamiminchointext](assets/sazanamiminchointext.png)

>[!NOTE]
>
>若要查看日文文字，您必須先輸入帶有日文字元的文字。 自訂日文字型的應用程式只會以特定方式格式化文字。 應用自定義日文字型不會將英文或其他字元更改為日文字元。