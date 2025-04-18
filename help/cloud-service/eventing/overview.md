---
title: AEM事件
description: 瞭解AEM事件、內容、使用原因及時機，並舉例說明。
version: Experience Manager as a Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 540
last-substantial-update: 2023-12-07T00:00:00Z
jira: KT-14649
thumbnail: KT-14649.jpeg
exl-id: 142ed6ae-1659-4849-80a3-50132b2f1a86
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '860'
ht-degree: 0%

---

# AEM事件

瞭解AEM事件、內容、使用原因及時機，並舉例說明。

>[!VIDEO](https://video.tv.adobe.com/v/3426686?quality=12&learn=on)

## 內容

AEM事件是雲端原生事件系統，可訂閱AEM事件以便在外部系統中處理。 AEM事件是每當特定動作發生時，AEM所傳送的狀態變更通知。 例如，這可以包括建立、更新或刪除內容片段時的事件。

![AEM事件](./assets/aem-eventing.png)

上圖以視覺效果呈現AEM as a Cloud Service如何產生事件，並傳送至Adobe I/O Events，後者接著向事件訂閱者顯示。

總而言之，有三個主要元件：

1. **事件提供者：** AEM as a Cloud Service。
1. **Adobe I/O Events：**&#x200B;根據Adobe的產品和技術整合、擴充及建置應用程式和體驗的開發人員平台。
1. **事件消費者：**&#x200B;由訂閱AEM事件的客戶所擁有的系統。 例如，CRM （客戶關係管理）、PIM （產品資訊管理）、OMS (Order Management系統)或自訂應用程式。

### 有何不同

[Apache Sling事件](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)、OSGi事件和[JCR觀察](https://jackrabbit.apache.org/oak/docs/features/observation.html)所有提供者機制都會訂閱及處理事件。 不過，這些內容與本檔案中討論的AEM事件不同。

AEM活動的主要區別包括：

- 事件取用者程式碼會在AEM外部執行，而不是在與AEM相同的JVM中執行。
- AEM產品程式碼負責定義事件並將其傳送到Adobe I/O Events。
- 事件資訊會標準化，並以JSON格式傳送。 如需詳細資訊，請參閱[cloudevents](https://cloudevents.io/)。
- 為了傳回AEM，事件消費者會使用AEM as a Cloud Service API。


## 使用它的原因與時機

AEM Eventing在系統架構和營運效率方面具備眾多優勢。 使用AEM事件的主要原因包括：

- **建置事件導向架構**：協助建立鬆散耦合的系統，可獨立擴充且可復原故障。
- **低程式碼並降低營運成本**：避免在AEM中進行自訂，讓系統更容易維護和擴充，進而降低營運費用。
- **簡化AEM與外部系統之間的通訊**：藉由讓Adobe I/O Events管理通訊(例如決定哪些AEM事件應傳送至特定系統或服務)，消除點對點連線。
- **事件更耐用**： Adobe I/O Events是高可用性、可擴充的系統，專為處理大量事件而設計，並能可靠地提供給訂閱者。
- **並行處理事件**：啟用同時傳送事件給多個訂閱者，允許跨不同系統分散處理事件。
- **無伺服器應用程式開發**：支援將事件使用者程式碼部署為無伺服器應用程式，進一步增強系統彈性和擴充性。

### 限制

AEM事件雖然功能強大，但有一些限制需要考量：

- **僅限AEM as a Cloud Service使用的可用性**：目前，AEM Eventing僅適用於AEM as a Cloud Service。

- **可用的事件型別**：檢閱目前可用的事件型別清單[這裡](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#available-event-types)。

## 如何啟用

如需後續步驟，請參閱[在您的AEM Cloud Service環境中啟用AEM事件](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment)。

## 如何訂閱

若要訂閱AEM事件，您不必在AEM中撰寫任何程式碼，而是必須設定[Adobe Developer Console](https://developer.adobe.com/)專案。 Adobe Developer Console是Adobe API、SDK、事件、執行階段和App Builder的閘道。

在這種情況下，Adobe Developer Console中的&#x200B;_專案_&#x200B;可讓您訂閱從AEM as a Cloud Service環境發出的事件，並設定事件傳送至外部系統。

如需詳細資訊，請參閱[如何在Adobe Developer Console中訂閱AEM事件](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console)。

## 使用方式

有兩種主要方法可使用AEM事件： _推播_&#x200B;方法和&#x200B;_提取_&#x200B;方法。

- **推播方法**：在此方法中，當有事件可用時，Adobe I/O Events會主動通知事件消費者。 整合選項包括Webhooks、Adobe I/O Runtime和Amazon EventBridge。
- **提取方法**：在此處，事件消費者主動輪詢Adobe I/O Events以檢查新事件。 此方法的主要整合選項為Adobe Developer Journaling API。

如需詳細資訊，請參閱[透過Adobe I/O Events處理的AEM事件](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#aem-events-processing-via-adobe-io)。

## 範例

<table>
  <tr>
    <td>
        <a  href="./examples/webhook.md"><img alt="在webhook上接收AEM活動" src="./assets/examples/webhook/webhook-example.png"/></a>
        <div><strong><a href="./examples/webhook.md">在webhook上接收AEM活動</a></strong></div>
        <p>
          使用Adobe提供的webhook接收AEM活動並檢閱活動詳細資料。
        </p>
      </td>
      <td>
        <a  href="./examples/journaling.md"><img alt="載入AEM事件日誌" src="./assets/examples/journaling/eventing-journal.png"/></a>
        <div><strong><a href="./examples/journaling.md">載入AEM事件日誌</a></strong></div>
        <p>
          使用Adobe提供的網頁應用程式，從日誌載入AEM事件並檢閱事件詳細資訊。
        </p>
      </td>
    </tr>
  <tr>
    <td>
        <a  href="./examples/runtime-action.md"><img alt="接收Adobe I/O Runtime動作的AEM事件" src="./assets/examples/runtime-action/eventing-runtime.png"/></a>
        <div><strong><a href="./examples/runtime-action.md">接收Adobe I/O Runtime動作的AEM事件</a></strong></div>
        <p>
          接收AEM事件並檢閱事件詳細資料。
        </p>
      </td>
      <td>
        <a  href="./examples/event-processing-using-runtime-action.md"><img alt="使用Adobe I/O Runtime動作處理的AEM事件" src="./assets/examples/event-processing-using-runtime-action/event-processing.png"/></a>
        <div>使用AEM動作處理的<strong><a href="./examples/event-processing-using-runtime-action.md">Adobe I/O Runtime事件</a></strong></div>
        <p>
          瞭解如何使用Adobe I/O Runtime動作處理收到的AEM事件。 事件處理包括AEM回呼、事件資料持續性，以及在SPA中顯示它們。
        </p>
      </td>
  </tr>
  <tr>
    <td>
        <a  href="./examples/assets-pim-integration.md"><img alt="PIM整合的AEM Assets事件" src="./assets/examples/assets-pim-integration/PIM-integration-tile.png"/></a>
        <div>用於PIM整合的<strong><a href="./examples/assets-pim-integration.md">AEM Assets事件</a></strong></div>
        <p>
          瞭解如何整合AEM Assets和產品資訊管理(PIM)系統，以進行中繼資料更新。
        </p>
      </td>
  </tr> 
</table>
