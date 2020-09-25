---
title: 在最適化表單中使用Geolocation API
seo-title: 在最適化表單中使用Geolocation API
description: 使用地理位置api的
seo-description: 使用地理位置api的
uuid: 5a461659-6873-4ea1-9f37-8296e5a9d895
feature: adaptive-forms,
topics: integrations
audience: developer
doc-type: article
activity: develop
version: 6.3,6.4,6.5
discoiquuid: 3400251b-aee0-4d69-994b-e1643fabc868
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '429'
ht-degree: 0%

---


# 在最適化表單中使用Geolocation API{#using-geolocation-api-s-in-adaptive-forms}

請造訪 [AEM Forms範例頁面](https://forms.enablementadobe.com/content/samples/samples.html?query=0) ，以取得此功能的即時示範連結。

在本文中，我們將檢視如何使用Google的Geolocation API填入最適化表單的欄位。 當您要在表單中填入目前的位址欄位時，通常會使用此使用案例。

在Adaptive Forms中使用Geolocation API時，會遵循下列步驟。

1. [從Google取得API金鑰](https://developers.google.com/maps/documentation/javascript/get-api-key) ，以使用Google地圖平台。 您可以取得有效期為1年的試用金鑰。

1. 最適化表單片段已建立，其中包含欄位以保存目前位址

1. Geolocation API是在Adaptive Form影像物件的click事件上呼叫

1. API呼叫傳回的JSON資料已加以剖析，並已據以設定「最適化表單」欄位值。

```javascript
navigator.geolocation.getCurrentPosition(showPosition);
function showPosition(position) 
{
console.log(" I am inside the showPosition in fragment");
console.log("Latitude: " + position.coords.latitude + "Longitude " + position.coords.longitude);
var url = "https://maps.googleapis.com/maps/api/geocode/json?latlng="+position.coords.latitude+","+position.coords.longitude+"&key=<your_api_key>";
  console.log(url);
  
  $.getJSON(url,function (data, textStatus){
    
    var location=data.results[0].formatted_address;
    console.log(location);
    
    for(i=0;i<data.results[0].address_components.length;i++)
        {
          if(data.results[0].address_components[i].types[0] == "street_number")
            {
              streetNumber.value = data.results[0].address_components[i].long_name;
            }
          if(data.results[0].address_components[i].types[0] == "route")
            {
              streetName.value = data.results[0].address_components[i].long_name;
            }
            if(data.results[0].address_components[i].types[0] == "postal_code")
            {
              
              zipCode.value = data.results[0].address_components[i].long_name;
            }
            if(data.results[0].address_components[i].types[0] == "locality")
            {
              
              city.value = data.results[0].address_components[i].long_name;
            }
          if(data.results[0].address_components[i].types[0] == "administrative_area_level_1")
            {
              
              state.value = data.results[0].address_components[i].long_name;
            }
        }
    
  });
}
```

![填入地理位置api的欄位](assets/capture-4.gif)

在第1行中，我們使用HTML Geolocation API來取得目前位置。 一旦獲得當前位置，我們會將當前位置傳遞至showPosition函式。

在showPosition函式中，我們使用Google API擷取指定經緯度的位址詳細資訊。

接著會剖析API傳回的JSON，以設定「最適化表單」欄位。

>[!NOTE]
>
>為了進行測試，您可以在URL中搭配使用HTTP通訊協定與localhost。
>
>對於生產伺服器，您需要為AEM伺服器啟用SSL才能取得此功能。
>
>與本文相關的範例已使用美國地址進行測試。 如果您想要在其他地理位置使用此功能，可能必須調整JSON剖析。

若要將此功能連接至您的伺服器，請依照下列步驟進行

* 安裝並啟動AEM Forms伺服器。

>!![NOTE] 這項功能已在AEM Forms 6.3和更新版本上測試
* [取得Google API金鑰](https://developers.google.com/maps/documentation/javascript/get-api-key)。
* [將與此文章相關的資產匯入AEM。](assets/geolocationapi.zip)
* [在編輯模式中開啟最適化表單片段。](http://localhost:4502/editor.html/content/forms/af/currentaddressfragment.html)
* 開啟「影像選擇」元件的規則編輯器。
* 將&lt;your_api_key>取代為Google API金鑰。
* 儲存您的變更。
* [預覽表格](http://localhost:4502/content/dam/formsanddocuments/currentaddressfragment/jcr:content?wcmmode=disabled)。
* 按一下「地理位置」圖示。
* 您的表格應填入您目前的位置。
