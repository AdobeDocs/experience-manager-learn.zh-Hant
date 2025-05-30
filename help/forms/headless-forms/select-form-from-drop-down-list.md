---
title: 從可用表單清單中選取表單
description: 使用listforms API填入下拉式清單
feature: Adaptive Forms
version: Experience Manager 6.5
jira: KT-13346
topic: Development
role: User
level: Intermediate
exl-id: 49b6a172-8c96-4fc6-8d31-c2109f65faac
duration: 88
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 1%

---

# 從下拉式清單中選取要填寫的表單

下拉式清單提供精簡且有條理的方式，向使用者呈現選項清單。 下拉式清單中的專案將會填入[listforms API](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms)的結果

![卡片檢視](./assets/forms-drop-down.png)

## 下拉式清單

下列程式碼可用來將listforms API呼叫的結果填入下拉式清單中。 根據使用者選擇，將顯示最適化表單以供使用者填寫和提交。 建立此介面時已使用[原物料UI元件](https://mui.com/)

```javascript
import * as React from 'react';
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import Box from '@mui/material/Box';
import InputLabel from '@mui/material/InputLabel';
import MenuItem from '@mui/material/MenuItem';
import FormControl from '@mui/material/FormControl';
import Select, { SelectChangeEvent } from '@mui/material/Select';
import { AdaptiveForm } from "@aemforms/af-react-renderer";

import { useState,useEffect } from "react";
export default function SelectFormFromDropDownList()
 {
    const extendMappings =
    {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
    };

const[formID, setFormID] = useState('');
const[afForms,SetOptions] = useState([]);
const [selectedForm, setForm] = useState('');
const HandleChange = (event) =>
     {
        console.log("The form id is "+event.target.value) 
    
        setFormID(event.target.value)
        
     };
const getForm = async () =>
     {
        
        console.log("The formID in getForm"+ formID);
        const resp = await fetch(`/adobe/forms/af/${formID}`);
        let formJSON = await resp.json();
        console.log(formJSON.afModelDefinition);
        setForm(formJSON.afModelDefinition);
     }
const getAFForms =async()=>
     {
        const response = await fetch("/adobe/forms/af/listforms")
        //let myresp = await response.status;
        let myForms = await response.json();
        console.log("Got response"+myForms.items[0].title);
        console.log(myForms.items)
        
        //setFormID('test');
        SetOptions(myForms.items)

        
     }
     useEffect( ()=>{
        getAFForms()
        

    },[]);
    useEffect( ()=>{
        getForm()
        

    },[formID]);

  return (
    <Box sx={{ minWidth: 120 }}>
      <FormControl fullWidth>
        <InputLabel id="demo-simple-select-label">Please select the form</InputLabel>
        <Select
          labelId="demo-simple-select-label"
          id="demo-simple-select"
          value={formID}
          label="Please select a form"
          onChange={HandleChange}
          
        >
       {afForms.map((afForm,index) => (
    
        
          <MenuItem  key={index} value={afForm.id}>{afForm.title}</MenuItem>
        ))}
        
       
        </Select>
      </FormControl>
      <div><AdaptiveForm mappings={extendMappings} formJson={selectedForm}/></div>
    </Box>
    

  );
  

}
```

建立此使用者介面時，使用了下列兩個API呼叫

* [清單表單](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms)。 擷取表單的呼叫只會在元件轉譯時執行一次。 API呼叫的結果儲存在afForms變數中。
在上述程式碼中，我們會使用map函式來反複執行afForms，針對afForms陣列中的每個專案，會建立MenuItem元件並新增至Select元件。

* 擷取表單 — 會對[getForm](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/Get-Form-Definition)進行get呼叫，其中識別碼是使用者在下拉式清單中選取的最適化表單的識別碼。 此GET呼叫的結果會儲存在selectedForm中。

```
const resp = await fetch(`/adobe/forms/af/${formID}`);
let formJSON = await resp.json();
console.log(formJSON.afModelDefinition);
setForm(formJSON.afModelDefinition);
```

* 顯示選取的表單。 下列程式碼已用於顯示選取的表單。 AdaptiveForm元素在aemforms/af-react-renderer npm套件中提供，且預期對應和formJson為其屬性

```
<div><AdaptiveForm mappings={extendMappings} formJson={selectedForm}/></div>
```

## 後續步驟

[以卡片版面配置顯示表單](./display-forms-card-view.md)
