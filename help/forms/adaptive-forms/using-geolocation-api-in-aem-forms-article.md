---
title: Geolocation API在自適應Forms中的應用
description: 使用地理位置API填充窗體上的地址欄位
feature: Adaptive Forms
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 50db6155-ee83-4ddb-9e3a-56e8709222db
source-git-commit: 631fef25620c84e04c012c8337c9b76613e3ad46
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 0%

---

# Geolocation API在自適應Forms中的應用{#using-geolocation-api-s-in-adaptive-forms}

在本文中，我們將看一下使用Google的Geolocation API填充自適應表單的欄位。 在要填充窗體上的當前地址欄位時，通常使用此用例。

接下來是在Adaptive Forms中使用Geolocation API。

1. [獲取API密鑰](https://developers.google.com/maps/documentation/javascript/get-api-key) 從Google到Google地圖平台。 您可以獲得一個有效期為1年的試用密鑰。

1. 建立了自適應表單片段，其中包含欄位以保存當前地址

1. Geolocation API被調用於Adaptive Form影像對象的click事件

1. 分析了API調用返回的JSON資料，並相應地設定了「自適應表單」欄位值。

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

![使用geoloaction api填充的欄位](assets/capture-4.gif)

在第1行中，我們使用HTML地理位置API來獲取當前位置。 獲得當前位置後，我們將當前位置傳遞給showPosition函式。

在showPosition函式中，我們使用GoogleAPI獲取給定經緯度的地址詳細資訊。

然後分析API返回的JSON以設定「自適應表單」欄位。

>[!NOTE]
>
>為了測試目的，可以將HTTP協定與URL中的localhost一起使用。
>
>對於生產伺服器，您需要為伺服器啟用SSLAEM才能獲得此功能。
>
>與本文相關的示例已用美國地址進行測試。 如果要在其他地理位置使用此功能，則可能必須調整JSON分析。

要將此功能安裝到您的伺服器上，請執行以下步驟

* 安裝並啟動AEM Forms伺服器。

>!![NOTE] 這一能力已在AEM Forms6.3及以上測試
* [獲取GoogleAPI密鑰](https://developers.google.com/maps/documentation/javascript/get-api-key)。
* [將與本條相關的資產導入AEM。](assets/geolocationapi.zip)
* [在編輯模式下開啟「自適應表單」片段。](http://localhost:4502/editor.html/content/forms/af/currentaddressfragment.html)
* 開啟「影像選擇」元件的規則編輯器。
* 替換 &lt;your_api_key> GoogleAPI密鑰。
* 保存更改。
* [預覽窗體](http://localhost:4502/content/dam/formsanddocuments/currentaddressfragment/jcr:content?wcmmode=disabled)。
* 按一下「地理位置」表徵圖。
* 您的表單應填入當前位置。
