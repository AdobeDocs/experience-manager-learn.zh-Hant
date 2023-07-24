---
title: 資料深入分析和啟用
description: 使用整個組織的線上和離線資料分析，在任何管道上推動即時個人化。
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 0%

---


# 資料深入分析和啟用

使用整個組織的線上和離線資料分析，在任何管道上推動即時個人化。

<table>

<!--  ROW 1  -->
<tr>
  <td><b>整合使用案例</b></td>
  <td><b>說明</b></td>
  <td><b>範例</b></td>
  <td><b>應用程式整合</b></td>
 </tr>

<!--  ROW 2  -->
<tr>
   <td rowspan="8"><b>資料分析和報告</b></td>

<!--  ROW 2a  -->
<td>使用Adobe Target並透過Adobe Analytics產生全面的報告，分析最佳化測試結果（包括A/B測試）。</td>
   <td><ul>
        <li>在豐富的Analytics報表中顯示A/B測試結果。</li>
       </ul></td>
   <td><a href="../integrations-between-applications/target/target-analytics.md" target="_blank" rel="noopener noreferrer">Target與Analytics</a></td>
  </tr>

<!--  ROW 2b  -->
<tr>
   <td>將Audience Manager區段資料傳送至Analytics，以進行更深入的對象分析、探索和最佳化。</td>
    <td><ul>
        <li>將協力廠商區段資料傳送至Analytics，以進行更深入的使用者分析。</li>
        <li>傳送CRM資料至Analytics以納入使用者分析。</li>
       </ul></td>
   <td><a href="../integrations-between-applications/aam/aam-analytics.md" target="_blank" rel="noopener noreferrer">Audience Manager和分析</a></td>
 </tr>

<!--  ROW 2c -->
<tr>
   <td>展開個人化和廣告平台的對象。</td>
    <td><ul>
        <li>使用伺服器端轉送功能，將Analytics資料傳送至Audience Manager以建立區段。</li>
       </ul></td>
   <td><a href="../integrations-between-applications/aam/aam-analytics.md" target="_blank" rel="noopener noreferrer">Audience Manager和分析</a></td>
 </tr>

<!--  ROW 2d  -->
<tr>
   <td>使用AEM Forms和Analytics追蹤、分析和報告使用者與數位表單的互動。 </td>
   <td><ul>
        <li>有關表單提交維度和量度的報告，包括完整的表單欄位和有錯誤的表單欄位。</li>
       </ul></td>
   <td><a href="../integrations-between-applications/experience-manager/experience-manager-analytics.md" target="_blank" rel="noopener noreferrer">AEM Forms與Analytics</a></td>
 </tr>

<!--  ROW 2e  -->
<tr>
   <td>使用Adobe Analytics追蹤、分析和報告AEM Sites網站上的使用者活動，以獲得全面的報表和深入分析。</td>
   <td><ul>
        <li>追蹤、分析和報告網站頁面的關鍵量度。</li>
        <li>使用Analytics報表，針對使用者體驗和內容策略進行資料導向式決策。</li>
        <li>分析頂端和底部轉換路徑。</li>
       </ul></td>
   <td><a href="../integrations-between-applications/experience-manager/experience-manager-analytics.md" target="_blank" rel="noopener noreferrer">AEM Sites與Analytics</a></td>
 </tr>

<!--  ROW 2f  -->
<tr>
   <td>使用點按前量度和轉換率，深入瞭解Adobe Campaign電子郵件行銷活動。</td>
   <td><ul>
        <li>追蹤、分析和報告電子郵件促銷活動的點選後轉換量度。</li>
        <li>深入研究行銷活動至Analytics中收集的其他維度。</li>
        <li>在單一報表中檢視點按前和點按後行銷活動量度。</li>
       </ul></td>
   <td><a href="../integrations-between-applications/campaign/campaign-analytics.md" target="_blank" rel="noopener noreferrer">Campaign與Analytics</a></td>
 </tr>

<!--  ROW 2g  -->
<tr>
   <td>使用您選取的關鍵量度和維度，獲得有關Adobe Commerce商店績效的全面深入分析。</td>
   <td><ul>
        <li>商業活動的資料分析和報告。</li>
        <li>使用您選取的關鍵量度和維度，獲得有關Adobe Commerce商店績效的全面深入分析。</li>
       </ul></td>
   <td><a href="../integrations-between-applications/commerce/commerce-analytics.md" target="_blank" rel="noopener noreferrer">Commerce和Analytics</a></td>
 </tr>

<!--  ROW 2h  -->
<tr>
   <td>在Customer Journey Analytics中使用Analysis Workspace中的Adobe Analytics行為資料。</td>
   <td><ul>
        <li>分析管道參與和轉換。</li>
        <li>了解熱門產品類別和產品。</li>
        <li>執行工具使用分析，以最佳化自助服務體驗。</li>
       </ul></td>
   <td><a href="../integrations-between-applications/analytics/analytics-customer-journey-analytics.md" target="_blank" rel="noopener noreferrer">分析和Customer Journey Analytics</a></td>
 </tr>


<!--  Row 3  -->
<tr>
  <td rowspan="5"><b>展開個人化和行銷的對象</b></td>
 </tr>

<!--  ROW 3a  -->
<tr>
  <td>使用多管道資料來源來建立受眾，以用於個人化和再行銷策略。</td>
  <td><ul><li>將受眾區段傳送至Real-Time CDP以傳送至目的地</li>
     </ul></td>
  <td><a href="../integrations-between-applications/rtcdp/rtcdp-cja.md" target="_blank" rel="noopener noreferrer">Customer Journey Analytics和Real-time Customer Data Platform</a></td>
 </tr>

<!--  ROW 3c  -->
<tr>
  <td>使用受眾和設定檔資料增強行銷活動。</td>
  <td><ul>
        <li>使用AEP資料進行受眾細分，強化您的行銷活動。</li>
      </ul></td>
   <td><a href="../integrations-between-applications/campaign/campaign-rtcdp.md">Campaign v8和Real Time Customer Data Platform</a></td>
 </tr>

<!--  ROW 3d  -->
<tr>
  <td>使用Audience Manager區段在Real-Time CDP中建立受眾，以用於個人化和再行銷。</td>
  <td><ul>
        <li>在網站、行動應用程式或支援的廣告頻道上，執行匿名數位對象目標定位和個人化。</li>
        <li>根據已知裝置和行為特性最佳化登入頁面和預先驗證體驗。</li>
        <li>套用Audience Manager第三方資料網路，進一步調整並擴大目標定位對象。</li>
      </ul></td>
  <td><a href="../integrations-between-applications/aam/aam-rtcdp.md" target="_blank" rel="noopener noreferrer">Audience Manager和Real-time Customer Data Platform</a></td>
 </tr>

<!--  ROW 3e  -->
<td>使用Analytics資料來建立對象，以用於個人化或再行銷策略。</td>
   <td><ul><li>在裝置或支援的廣告頻道上執行數位對象目標定位和個人化。</li>
           <li>根據裝置和行為屬性最佳化已知的客戶登陸頁面和匿名體驗。</li>
           <li>對已知管道啟用對象，例如電子郵件和簡訊。</li>
        </ul></td>
   <td><a href="../integrations-between-applications/analytics/analytics-rtcdp.md" target="_blank" rel="noopener noreferrer">Analytics和Real-time Customer Data Platform</a></td>


<!--  ROW 4  -->
<tr>
   <td><b>測量行銷影像使用情況和效能</b></td>
   <td>整合AEM Assets和Adobe Analytics，以追蹤和分析行銷影像的成效。</td>
   <td><ul><li>追蹤和分析資產績效。</li>
           <li>分析使用者參與。</li>
           <li>最佳化內容策略。</li>
        </ul></td>
   <td><a href="../integrations-between-applications/experience-manager/experience-manager-analytics.md" target="_blank" rel="noopener noreferrer">AEM Assets與Analytics</a></td>
 </tr>
 </table>
