---
title: 在卡片檢視中顯示擷取的表單
description: 使用listforms API顯示表單
feature: Adaptive Forms
version: 6.5
kt: 13311
topic: Development
role: User
level: Intermediate
exl-id: 7316ca02-be57-4ecf-b162-43a736b992b3
source-git-commit: 529e98269a08431152686202a8a2890712b9c835
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 0%

---

# 以卡片格式擷取及顯示表單

卡片檢視格式是以卡片形式呈現資訊或資料的設計模式。 每個卡片都代表個別的內容或資料專案，通常由視覺上不同的容器組成，其中包含排列的特定元素。
React中的可點按卡片是類似卡片或圖磚的互動元件，使用者可點按或點選。 當使用者點按或點選可點按的卡片時，會觸發指定的動作或行為，例如導覽至其他頁面、開啟強制回應視窗或更新UI。

在本文中，我們將使用 [listforms API](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms) 以擷取表單並以卡片格式顯示表單，然後在點選事件時開啟最適化表單。

![卡片檢視](./assets/card-view-forms.png)

## 卡片範本

下列程式碼已用於設計卡片範本。 卡片範本顯示最適化表單的標題和說明以及Adobe標誌。 [原物料UI元件](https://mui.com/) 已用於建立此版面。



```javascript
import Container from "@mui/material/Container";
import Form from './Form';
import PlainText from './plainText'
import TextField from './TextField'
import Button from './Button';
import { AdaptiveForm } from "@aemforms/af-react-renderer";

import { CardActionArea, Typography } from "@mui/material";
import { Box } from "@mui/system";
import { useState,useEffect } from "react";
import DisplayForm from "../DisplayForm";
import { Link } from "react-router-dom";
export default function FormCard({headlessForm}) {
const extendMappings =
    {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
    };
   
    return (
        
            <Grid item xs={3}>
                <Paper elevation={3}>
                    <img src="/content/dam/formsanddocuments/registrationform/jcr:content/renditions/cq5dam.thumbnail.48.48.png" className="img"/>
                    <Box padding={3}>
                        <Link style={{ textDecoration: 'none' }} to={`/displayForm${headlessForm.id}`}>
                            <Typography variant="subtititle2" component="h2">
                                {headlessForm.title}
                            </Typography>
                            <Typography variant="subtititle3" component="h4">
                                {headlessForm.description}
                            </Typography>
                        </Link>
                
                    </Box>
                </Paper>
            </Grid>
    );
    

};
```

下列路由是在Main.js中定義以導覽至DisplayForm.js

```javascript
    <Route path="/displayForm/:formID" element={<DisplayForm/>} exact/>
```

## 擷取表單

listforms API是用來從AEM伺服器擷取表單。 API會傳回JSON物件陣列，每個JSON物件都代表一個表單。

```javascript
import { useState,useEffect } from "react";
import React, { Component } from "react";
import FormCard from "./components/FormCard";
import Grid from "@mui/material/Grid";
import Paper from "@mui/material/Paper";
import Container from "@mui/material/Container";
 
export default function ListForm(){
    const [fetchedForms,SetHeadlessForms] = useState([])
    const getForms=async()=>{
        const response = fetch("/adobe/forms/af/listforms")
        let headlessForms = await (await response).json();
        console.log(headlessForms.items);
        SetHeadlessForms(headlessForms.items);
    }
    useEffect( ()=>{
        getForms()
        

    },[]);
    return(
        <div>
             <div>
                <Container>
                   <Grid container spacing={3}>
                       {
                            fetchedForms.map( (afForm,index) =>
                                <FormCard headlessForm={afForm} key={index}/>
                         
                            )
                        }
                    </Grid>
                </Container>
             </div>

        </div>
    )
}
```

在上述程式碼中，我們會使用map函式來反複執行fetchedForms，並針對fetchedForms陣列中的每個專案，建立FormCard元件並將其新增至Grid容器。 您現在可以根據需求在React應用程式中使用ListForm元件。

## 後續步驟

[當使用者按一下卡片時顯示最適化表單](./open-form-card-view.md)