---
title: 獲取要嵌入的自適應表單的JSON
description: 使用API獲取自適應表單的json
feature: Adaptive Forms
version: 6.5
kt: 13285
topic: Development
role: User
level: Intermediate
source-git-commit: c6e83a627743c40355559d9cdbca2b70db7f23ed
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---


# 獲取表單的JSON

登錄到您的AEM Forms作者實例，並使用 **帶核心元件的空白** 的下界。 將表單發佈到發佈實例。

為了嵌入表單，我們首先通過對發佈伺服器進行get調用來獲取自適應表單的json。

以下代碼段提取名為的自適應表單的json **聯繫人**

```javascript
const getForm = async () => {
        const resp = await fetch('/content/forms/af/contactus/jcr:content/guideContainer.model.json');
        let formJSON = await resp.json();
        console.log(formJSON);
        setForm(formJSON);
      }
```

Contact函式元件的完整代碼如下所示

```javascript
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import Panel from './components/Panel';
import { useState,useEffect } from "react";
import { AdaptiveForm } from "@aemforms/af-react-renderer";

export default function Contact(){
   const [selectedForm, setForm] = useState("");
   const extendMappings = {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
      };
    const getForm = async () => {
        
        const resp = await fetch('/content/forms/af/contactus/jcr:content/guideContainer.model.json');
        let formJSON = await resp.json();
        console.log(formJSON);
        setForm(formJSON);
      }
      useEffect( ()=>{
        getForm();
        

    },[]);
    return(
        
        <div>
            <h1>Get in touch with us!!!!</h1>
            <AdaptiveForm mappings={extendMappings} formJson={selectedForm} />
      
          
        </div>
    )
}
```

上述代碼使用映射到自適應表單中使用的元件的本機html元件。 例如，我們將文本輸入自適應表單元件映射到TextField元件。 文章中使用的本機元件 [可從此下載](./assets/native-components.zip)