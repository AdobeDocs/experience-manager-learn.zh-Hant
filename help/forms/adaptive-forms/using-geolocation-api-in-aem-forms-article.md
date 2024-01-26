---
title: 在最適化Forms中使用地理位置API
description: 使用地理位置API填入表單上的位址列位
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 50db6155-ee83-4ddb-9e3a-56e8709222db
last-substantial-update: 2020-03-20T00:00:00Z
duration: 106
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 0%

---

# 在最適化Forms中使用地理位置API{#using-geolocation-api-s-in-adaptive-forms}

在本文中，我們將瞭解如何使用Google的地理位置API填入調適型表單的欄位。 當您想要填入表單上的目前位址列位時，通常使用此使用案例。

在適用性Forms中使用地理位置API時，需遵循下列步驟。

1. [取得API金鑰](https://developers.google.com/maps/documentation/javascript/get-api-key) 從Google使用Google地圖平台。 您可以取得試用金鑰，有效期為1年。

1. 最適化表單片段是使用儲存目前位址的欄位建立的

1. Geolocation API是在最適化表單影像物件的點選事件上叫用的

1. 已剖析API呼叫傳回的JSON資料，並據此設定調適型表單欄位值。

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

在第1行，我們使用HTML地理位置API來取得目前位置。 取得目前位置後，我們會將目前位置傳遞給showPosition函式。

在showPosition函式中，我們使用Google API來擷取指定經緯度的位址詳細資料。

API傳回的JSON接著會經過剖析，以設定調適型表單欄位。

>[!NOTE]
>
>為了測試目的，您可以將HTTP通訊協定與URL中的localhost搭配使用。
>
>對於生產伺服器，您需要為AEM伺服器啟用SSL才能取得此功能。
>
>與本文相關的範例已透過美國地址進行測試。 如果您想在其他地理位置使用此功能，則可能必須調整JSON剖析。

若要讓伺服器具備此功能，請遵循下列步驟

* 安裝並啟動AEM Forms伺服器。
> 此功能已在AEM Forms 6.3及更高版本上測試
* [取得Google API金鑰](https://developers.google.com/maps/documentation/javascript/get-api-key).
* [將與本文相關的資產匯入AEM。](assets/geolocationapi.zip)
* [在編輯模式中開啟最適化表單片段。](http://localhost:4502/editor.html/content/forms/af/currentaddressfragment.html)
* 開啟影像選擇元件的規則編輯器。
* 取代 &lt;your_api_key> 使用Google API金鑰。
* 儲存您的變更。
* [預覽表單](http://localhost:4502/content/dam/formsanddocuments/currentaddressfragment/jcr:content?wcmmode=disabled).
* 按一下「地理位置」圖示。
* 您的表單應填入您的目前位置。
