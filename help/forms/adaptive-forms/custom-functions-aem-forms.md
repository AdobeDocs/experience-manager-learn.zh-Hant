---
title: AEM Forms中的自訂函式
description: 在最適化表單中建立和使用自訂函式
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
jira: KT-9685
exl-id: 07fed661-0995-41ab-90c4-abde35a14a4c
last-substantial-update: 2021-06-09T00:00:00Z
duration: 286
source-git-commit: a72f533b36940ce735d5c01d1625c6f477ef4850
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 0%

---

# 自訂函式

AEM Forms 6.5匯入定義JavaScript函式的功能，以在使用規則編輯器定義複雜商業規則時使用這些函式。
AEM Forms提供許多立即可用的自訂函式，但您需要定義自己的自訂函式，並在多個表單中使用這些函式。

若要定義您的第一個自訂函式，請遵循下列步驟：
* [登入crx](http://localhost:4502/crx/de/index.jsp#/apps/experience-league/clientlibs)
* 在應用程式下建立名為experience-league的新資料夾（此資料夾名稱可以是您選擇的名稱）
* 儲存您的變更。
* 在experience-league資料夾底下建立名為clientlibs的cq：ClientLibraryFolder型別的新節點。
* 選取新建立的clientlibs資料夾，然後新增allowProxy和類別屬性（如熒幕擷取畫面中所示），並儲存變更。

![client-lib](assets/custom-functions.png)
* 在&#x200B;**clientlibs**&#x200B;資料夾下建立名為&#x200B;**js**&#x200B;的資料夾
* 在&#x200B;**js**&#x200B;資料夾下建立名為&#x200B;**functions.js**&#x200B;的檔案
* 在&#x200B;**clientlibs**&#x200B;資料夾下建立名為&#x200B;**js.txt**&#x200B;的檔案。 儲存您的變更。
* 您的資料夾結構看起來應該像下面的熒幕擷取畫面。

![規則編輯器](assets/folder-structure.png)

* 按兩下functions.js以開啟編輯器。
將下列程式碼複製到functions.js中並儲存變更。

```javascript
/**
* Get List of County names
* @name getCountyNamesList Get list of county names
* @returns {string[]} An array of county names
 */
function getCountyNamesList()
{
    return ["Santa Clara", "Alameda", "Buxor", "Contra Costa", "Merced"];

}
/**
* Covert UTC to Local Time
* @name convertUTC Convert UTC Time to Local Time
* @param {string} strUTCString in Stringformat
* @return {string}
*/
function convertUTC(strUTCString)
{
    var dt = new Date(strUTCString);
    console.log(dt.toLocaleString());
    return dt.toLocaleString();
}
```

請[參閱jsdoc](https://jsdoc.app/index.html)以取得有關註解javascript函式的詳細資訊。
上述程式碼有兩個功能：
**getCountyNamesList** — 傳回字串陣列
**convertUTC** — 將UTC時間戳記轉換為當地時區

開啟js.txt並貼上下列程式碼並儲存變更。

```javascript
#base=js
functions.js
```

#base=js行會指定JavaScript檔案所在的目錄。
以下各行表示JavaScript檔案相對於基礎位置的位置。

如果您在建立自訂函式時遇到問題，請隨時[下載此套件](assets/custom-functions.zip)並安裝在您的AEM執行個體中。

## 使用自訂函式

以下影片會逐步帶您瞭解在適用性表單的規則編輯器中使用自訂函式的相關步驟
>[!VIDEO](https://video.tv.adobe.com/v/340305?quality=12&learn=on)
