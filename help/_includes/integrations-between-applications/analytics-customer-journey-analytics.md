---
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 1%

---


# 將Adobe Analytics與Customer Journey Analytics整合

{{analytics-description}}

{{customer-journey-analytics-description}}

將Adobe Analytics與Customer Journey Analytics整合可提供下列主要優點：

+ **全面深入分析** 至客戶行為和偏好設定。
+ **順暢的跨頻道追蹤** 以取得整體檢視。
+ **統一的資料和報告** 以進行精確分析。
+ **增強的個人化** 以及提升客戶參與度。
+ **即時資料深入分析** 用於敏捷決策。

## 常見整合

<table>
    <thead>
        <tr>
            <td>Experience Cloud應用程式</td>
            <td>整合，使用</td>
            <td>使用時機</td>
            <td>常見使用案例</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><a href="../../integrations/tutorials/analytics-customer-journey-analytics/experience-platform-source-connector.md" target="_blank" rel="noreferrer">Customer Journey Analytics分析</a></td>
            <td>Experience Platform來源聯結器</td>
            <td>
                <ul>
                    <li>TODO：使用此整合將Analytics資料從報表套裝擷取至Experience Platform。</li>
                    <li>當客戶設定檔的資料可用性可能在收集資料的2-30分鐘之間，而資料湖的可用性則高達90分鐘。</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>由使用者介面直接啟動的工作流程。</li>
                    <li>對應使用者介面以將Analytics prop和eVar複製到新的XDM欄位。</li>
                    <li>從Real-Time Customer Profile和Customer Journey Analytics中獲得價值的最快方式。</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td><a href="https://www.adobe.com/" target="_blank" rel="noreferrer">TODO連結：具有Customer Journey Analytics的Analytics</a></td>
            <td>Experience Platform邊緣</td>
            <td>
                <ul>
                    <li>當您想要實作長期策略時。 這會使用AEP Web SDK、AEP Mobile SDK或Edge Network Server API，將資料直接從裝置傳送至Experience Platform。</li>
                    <li>當您是新客戶或現有客戶時，他們需要客戶設定檔的Analytics資料可用性，以支援相同和下一頁個人化使用案例。</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>針對收集用於支援使用案例的資料提供最高級別的控制。</li>
                    <li>使用者端資料可輕鬆對應至XDM欄位。</li>
                    <li>即時客戶個人檔案資料可用速度最快。</li>
                </ul>
            </td>
        </tr>  
    </tbody>          
</table>
