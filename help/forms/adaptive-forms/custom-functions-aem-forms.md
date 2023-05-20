---
title: AEM Forms自定義函式
description: 在自適應表單中建立和使用自定義函式
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
kt: 9685
exl-id: 07fed661-0995-41ab-90c4-abde35a14a4c
last-substantial-update: 2021-06-09T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 0%

---

# 自定義函式

AEM Forms6.5引入了定義JavaScript函式的功能，這些函式可用於使用規則編輯器定義複雜業務規則。
AEM Forms提供了許多此類自定義函式，但您需要定義自己的自定義函式並跨多個表單使用它們。

要定義第一個自定義函式，請執行以下步驟：
* [登錄到crx](http://localhost:4502/crx/de/index.jsp#/apps/experience-league/clientlibs)
* 在名為「體驗聯盟」的應用下建立新資料夾（此資料夾名稱可以是您選擇的名稱）
* 保存更改。
* 在「體驗聯盟」資料夾下，建立一個名為clientlibs的cq:ClientLibraryFolder類型的新節點。
* 選擇新建立的客戶端資料夾，並添加allowProxy和類別屬性，如螢幕抓圖所示，並保存您所做的更改。

![客戶端庫](assets/custom-functions.png)
* 建立名為 **j** 下 **客戶端** 資料夾
* 建立名為 **函式.js** 下 **j** 資料夾
* 建立名為 **js.txt** 下 **客戶端** 的子菜單。 保存更改。
* 資料夾結構應與下面的螢幕抓圖類似。

![規則編輯器](assets/folder-structure.png)

* 按兩下functions.js以開啟編輯器。
將以下代碼複製到functions.js中並保存您所做的更改。

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

請 [參考jsdoc ](https://jsdoc.app/index.html)的子菜單。
上述代碼具有兩個功能：
**getCountyNamesList**  — 返回字串陣列
**convertUTC**  — 將UTC時間戳轉換為本地時區

開啟js.txt並貼上以下代碼並保存更改。

```javascript
#base=js
functions.js
```

行#base=js指定JavaScript檔案所在的目錄。
下面的行指示JavaScript檔案相對於基位置的位置。

如果您在建立自定義函式時遇到問題，請隨時 [下載並安裝此包](assets/custom-functions.zip) 你AEM的。

## 使用自定義函式

以下視頻將引導您完成在自適應表單的規則編輯器中使用自定義函式涉及的步驟
>[!VIDEO](https://video.tv.adobe.com/v/340305?quality=12&learn=on)
