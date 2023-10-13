---
title: 在表格提交時顯示感謝訊息
description: 使用onSubmitSuccess處理常式，在react應用程式中顯示已設定的感謝訊息
feature: Adaptive Forms
version: 6.5
kt: 13490
topic: Development
role: User
level: Intermediate
exl-id: 489970a6-1b05-4616-84e8-52b8c87edcda
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 0%

---

# 顯示已設定的感謝訊息

在提交表單時顯示感謝訊息，是以貼心的方式認可並感謝使用者填寫和提交表單。 這可確認其提交內容已收到並受到讚賞。 感謝訊息是使用最適化表單指南容器的提交索引標籤來設定

![感謝訊息](assets/thank-you-message.png)

您可以在AdaptiveForm超級元件的onSuccess事件處理常式，存取已設定的感謝訊息。
關聯onSuccess事件的程式碼和onSuccess事件處理常式的程式碼如下所示

```javascript
<AdaptiveForm mappings={extendMappings} onSubmitSuccess={onSuccess} formJson={selectedForm}/>
```

```javascript
const onSuccess=(action) =>{
        let body = action.payload?.body;
        debugger;
        setThankYouMessage(body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));
        console.log("Thank you message "+body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));

      }
```

Contact函式元件的完整程式碼如下

```javascript
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import { useState,useEffect } from "react";
import { AdaptiveForm } from "@aemforms/af-react-renderer";
export default function Contact(){
  
    const [selectedForm, setForm] = useState("");
    const [thankYouMessage, setThankYouMessage] = useState("");
    const [formSubmitted, setFormSubmitted] = useState(false);
  
    const extendMappings = {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
      };
     const onSuccess=(action) =>{
        let body = action.payload?.body;
        debugger;
        setFormSubmitted(true);
        setThankYouMessage(body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));
        // Remove any html tags in the thank you message
        console.log("Thank you message "+body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));

      }
      
      const getForm = async () => {
        
        const resp = await fetch('/adobe/forms/af/L2NvbnRlbnQvZm9ybXMvYWYvY29udGFjdHVz');
        // Get the form id manually using the listform api
        let formJSON = await resp.json();
        setForm(formJSON.afModelDefinition)
      }
      useEffect( ()=>{
        getForm()
        

    },[]);
    
    return(
        
        <div>
           {!formSubmitted ?
            (
                <div>
                    <h1>Get in touch with us!!!!</h1>
                    <AdaptiveForm mappings={extendMappings} onSubmitSuccess={onSuccess} formJson={selectedForm}/>
                </div>
            ) :
            (
                <div>
                    <div>{thankYouMessage}</div>
                </div>
            )}
        </div>
      
          
        
    )
}
```

上述程式碼使用原生html元件，這些元件對應至最適化表單中使用的元件。 例如，我們將文字輸入最適化表單元件對應至TextField元件。 文章中使用的原生元件 [可從此處下載](./assets/native-components.zip)
