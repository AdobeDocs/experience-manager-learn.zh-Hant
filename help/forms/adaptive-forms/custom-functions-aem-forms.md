---
title: AEM Forms中的自訂函式
description: 在適用性表單中建立和使用自訂函式
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
kt: 9685
exl-id: 07fed661-0995-41ab-90c4-abde35a14a4c
last-substantial-update: 2021-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 0%

---

# 自訂函式

AEM Forms 6.5導入了定義JavaScript函式的功能，這些函式可用於使用規則編輯器定義複雜業務規則。
AEM Forms提供許多此類自訂函式，但您需要定義自己的自訂函式，並在多個表單中使用。

若要定義您的第一個自訂函式，請依照下列步驟操作：
* [登入crx](http://localhost:4502/crx/de/index.jsp#/apps/experience-league/clientlibs)
* 在名為experience-league的應用程式下建立新資料夾（此資料夾名稱可以是您所選擇的名稱）
* 儲存您的變更。
* 在experience-league資料夾下，建立名為clientlibs的cq:ClientLibraryFolder類型的新節點。
* 選取新建立的clientlibs資料夾，並新增allowProxy和類別屬性（如螢幕擷取畫面所示），然後儲存您的變更。

![client-lib](assets/custom-functions.png)
* 建立名為 **js** 在 **clientlibs** 資料夾
* 建立檔案，稱為 **函式.js** 在 **js** 資料夾
* 建立檔案，稱為 **js.txt** 在 **clientlibs** 檔案夾。 儲存您的變更。
* 您的資料夾結構應如下方的螢幕擷取畫面。

![規則編輯器](assets/folder-structure.png)

* 連按兩下functions.js以開啟編輯器。
將下列程式碼複製至functions.js並儲存您的變更。

```javascript
/**
* Get List of County names
* @name getCountyNamesList Get list of county names
* @return {OPTIONS} drop down options 
 */
function getCountyNamesList()
{
    var countyNames= [];
    countyNames[0] = "Santa Clara";
    countyNames[1] = "Alameda";
    countyNames[2] = "Buxor";
    countyNames[3] = "Contra Costa";
    countyNames[4] = "Merced";

    return countyNames;

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

請 [請參閱jsdoc ](https://jsdoc.app/index.html)以取得關於註解javascript函式的詳細資訊。
上述程式碼有兩個功能：
**getCountyNamesList**  — 傳回字串陣列
**convertUTC**  — 將UTC時間戳轉換為本地時區

開啟js.txt並貼上下列程式碼並儲存您的變更。

```javascript
#base=js
functions.js
```

#base=js行會指定JavaScript檔案的目錄。
以下幾行表示JavaScript檔案相對於基本位置的位置。

如果您在建立自訂函式時遇到問題，歡迎 [下載並安裝此包](assets/custom-functions.zip) 在AEM例項中。

## 使用自訂函式

以下影片會逐步帶您了解在適用性表單的規則編輯器中使用自訂函式的相關步驟
>[!VIDEO](https://video.tv.adobe.com/v/340305?quality=9&learn=on)
