---
title: Webhook和AEM活動
description: 瞭解如何透過webhook接收AEM事件，並檢閱事件詳細資訊，例如裝載、標題和中繼資料。
version: Experience Manager as a Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
duration: 358
last-substantial-update: 2023-01-29T00:00:00Z
jira: KT-14732
thumbnail: KT-14732.jpeg
exl-id: 00954d74-c4c7-4dac-8d23-7140c49ae31f
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '523'
ht-degree: 1%

---

# Webhook和AEM活動

瞭解如何透過webhook接收AEM事件，並檢閱事件詳細資訊，例如裝載、標題和中繼資料。


>[!VIDEO](https://video.tv.adobe.com/v/3449759?captions=chi_hant&quality=12&learn=on)


>[!IMPORTANT]
>
>影片參考了Glitch託管的webhook端點。 由於Glitch已終止其託管服務，因此webhook已移轉至Azure應用程式服務。
>
>功能保持不變 — 只有託管平台已變更。


您也可以使用自己的webhook端點來接收AEM事件，而不使用Adobe提供的範例webhook。

## 先決條件

若要完成本教學課程，您需要：

- 已啟用[AEM事件的AEM as a Cloud Service環境](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment)。

- 已針對Adobe Developer Console事件設定[AEM專案](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console)。


## 存取webhook

若要存取Adobe提供的webhook範例，請遵循下列步驟：

- 確認您可以在新的瀏覽器分頁中存取[Adobe提供的webhook](https://aemeventing-webhook.azurewebsites.net/)範例。

  ![Adobe提供的範例webhook](../assets/examples/webhook/adobe-provided-webhook.png)

- 輸入您webhook的唯一名稱，例如`<YOUR_PETS_NAME>-aem-eventing`，然後按一下&#x200B;**連線**。 您應該會看到`Connected to: ${YOUR-WEBHOOK-URL}`訊息出現在畫面上。

  ![建立您的webhook端點](../assets/examples/webhook/create-webhook-endpoint.png)

- 記下&#x200B;**Webhook URL**。 在本教學課程的後半部分，您會用到它。

## 在Adobe Developer Console專案中設定webhook

若要在上述webhook URL上接收AEM活動，請按照以下步驟操作：

- 在[Adobe Developer Console](https://developer.adobe.com)中，導覽至您的專案並按一下以開啟專案。

- 在&#x200B;**產品與服務**&#x200B;區段下，按一下應將AEM活動傳送至webhook之所需活動卡片旁的省略符號`...`，然後選取&#x200B;**編輯**。

  ![Adobe Developer Console專案編輯](../assets/examples/webhook/adobe-developer-console-project-edit.png)

- 在新開啟的&#x200B;**設定事件註冊**&#x200B;對話方塊中，按一下&#x200B;**下一步**&#x200B;以繼續進行&#x200B;**如何接收事件**&#x200B;步驟。

  ![Adobe Developer Console專案設定](../assets/examples/webhook/adobe-developer-console-project-configure.png)

- 在&#x200B;**如何接收事件**&#x200B;步驟中，選取&#x200B;**Webhook**&#x200B;選項並貼上您先前從Adobe提供的範例webhook複製的&#x200B;**Webhook URL**，然後按一下&#x200B;**儲存已設定的事件**。

  ![Adobe Developer Console專案Webhook](../assets/examples/webhook/adobe-developer-console-project-webhook.png)

- 在Adobe提供的webhook範例頁面中，您應該會看到GET請求，這是Adobe I/O Events傳送以驗證webhook URL的質詢請求。

  ![Webhook — 挑戰要求](../assets/examples/webhook/webhook-challenge-request.png)


## 觸發AEM事件

若要從已在上述AEM專案中註冊的AEM as a Cloud Service環境觸發Adobe Developer Console事件，請遵循下列步驟：

- 透過[Cloud Manager](https://my.cloudmanager.adobe.com/)存取並登入您的AEM as a Cloud Service作者環境。

- 根據您的&#x200B;**訂閱事件**，建立、更新、刪除、發佈或取消發佈內容片段。

## 檢閱事件詳細資料

完成上述步驟後，您應該會看到正在傳送至webhook的AEM活動。 在Adobe提供的範例webhook頁面中尋找POST請求。

![Webhook - POST要求](../assets/examples/webhook/webhook-post-request.png)

以下是POST要求的主要詳細資料：

- 路徑： `/webhook/${YOUR-WEBHOOK-URL}`，例如`/webhook/AdobeTM-aem-eventing`

- 標頭： Adobe I/O Events傳送的要求標頭，例如：

```json
{
  "host": "aemeventing-webhook.azurewebsites.net",
  "user-agent": "Adobe/1.0",
  "accept-encoding": "deflate,compress,identity",
  "max-forwards": "10",
  "x-adobe-public-key2-path": "/prod/keys/pub-key-kruhWwu4Or.pem",
  "x-adobe-delivery-id": "25c36f70-9238-4e4c-b1d8-4d9a592fed9d",
  "x-adobe-provider": "aemsites_7ABB3E6A5A7491460A495D61@AdobeOrg_acct-aem-p63947-e1249010@adobe.com",
  "x-adobe-public-key1-path": "/prod/keys/pub-key-lyTiz3gQe4.pem",
  "x-adobe-event-id": "b555a1b1-935b-4541-b410-1915775338b5",
  "x-adobe-event-code": "aem.sites.contentFragment.modified",
  "x-adobe-digital-signature-2": "Lvw8+txbQif/omgOamJXJaJdJMLDH5BmPA+/RRLhKG2LZJYWKiomAE9DqKhM349F8QMdDq6FXJI0vJGdk0FGYQa6JMrU+LK+1fGhBpO98LaJOdvfUQGG/6vq8/uJlcaQ66tuVu1xwH232VwrQOKdcobE9Pztm6UX0J11Uc7vtoojUzsuekclKEDTQx5vwBIYK12bXTI9yLRsv0unBZfNRrV0O4N7KA9SRJFIefn7hZdxyYy7IjMdsoswG36E/sDOgcnW3FVM+rhuyWEizOd2AiqgeZudBKAj8ZPptv+6rZQSABbG4imOa5C3t85N6JOwffAAzP6qs7ghRID89OZwCg==",
  "x-adobe-digital-signature-1": "ZQywLY1Gp/MC/sXzxMvnevhnai3ZG/GaO4ThSGINIpiA/RM47ssAw99KDCy1loxQyovllEmN0ifAwfErQGwDa5cuJYEoreX83+CxqvccSMYUPb5JNDrBkG6W0CmJg6xMeFeo8aoFbePvRkkDOHdz6nT0kgJ70x6mMKgCBM+oUHWG13MVU3YOmU92CJTzn4hiSK8o91/f2aIdfIui/FDp8U20cSKKMWpCu25gMmESorJehe4HVqxLgRwKJHLTqQyw6Ltwy2PdE0guTAYjhDq6AUd/8Fo0ORCY+PsS/lNxim9E9vTRHS7TmRuHf7dpkyFwNZA6Au4GWHHS87mZSHNnow==",
  "x-arr-log-id": "881073f0-7185-4812-9f17-4db69faf2b68",
  "client-ip": "52.37.214.82:46066",
  "disguised-host": "aemeventing-webhook.azurewebsites.net",
  "x-site-deployment-id": "aemeventing-webhook",
  "was-default-hostname": "aemeventing-webhook.azurewebsites.net",
  "x-forwarded-proto": "https",
  "x-appservice-proto": "https",
  "x-arr-ssl": "2048|256|CN=Microsoft Azure RSA TLS Issuing CA 03, O=Microsoft Corporation, C=US|CN=*.azurewebsites.net, O=Microsoft Corporation, L=Redmond, S=WA, C=US",
  "x-forwarded-tlsversion": "1.3",
  "x-forwarded-for": "52.37.214.82:46066",
  "x-original-url": "/webhook/AdobeTechMarketing-aem-eventing",
  "x-waws-unencoded-url": "/webhook/AdobeTechMarketing-aem-eventing",
  "x-client-ip": "52.37.214.82",
  "x-client-port": "46066",
  "content-type": "application/cloudevents+json; charset=UTF-8",
  "content-length": "1178"
}
```

- body/payload： Adobe I/O Events傳送的要求內文，例如：

```json
{
  "specversion": "1.0",
  "id": "83b0eac0-56d6-4499-afa6-4dc58ff6ac7f",
  "source": "acct:aem-p63947-e1249010@adobe.com",
  "type": "aem.sites.contentFragment.modified",
  "datacontenttype": "application/json",
  "dataschema": "https://ns.adobe.com/xdm/aem/sites/events/content-fragment-modified.json",
  "time": "2025-07-24T13:53:23.994109827Z",
  "eventid": "b555a1b1-935b-4541-b410-1915775338b5",
  "event_id": "b555a1b1-935b-4541-b410-1915775338b5",
  "recipient_client_id": "606d4074c7ea4962aaf3bc2a5ac3b7f9",
  "recipientclientid": "606d4074c7ea4962aaf3bc2a5ac3b7f9",
  "data": {
    "user": {
      "imsUserId": "ims-933E1F8A631CAA0F0A495E53@80761f6e631c0c7d495fb3.e",
      "principalId": "xx@adobe.com",
      "displayName": "Sachin Mali"
    },
    "path": "/content/dam/wknd-shared/en/adventures/beervana-portland/beervana-in-portland",
    "sourceUrl": "https://author-p63947-e1249010.adobeaemcloud.com",
    "model": {
      "id": "L2NvbmYvd2tuZC1zaGFyZWQvc2V0dGluZ3MvZGFtL2NmbS9tb2RlbHMvYWR2ZW50dXJl",
      "path": "/conf/wknd-shared/settings/dam/cfm/models/adventure"
    },
    "id": "9e1e9835-64c8-42dc-9d36-fbd59e28f753",
    "tags": [
      "wknd-shared:region/nam/united-states",
      "wknd-shared:activity/social",
      "wknd-shared:season/fall"
    ],
    "properties": [
      {
        "name": "price",
        "changeType": "modified"
      }
    ]
  }
}
```

您可以看到AEM事件詳細資料具有在webhook中處理事件所需的所有必要資訊。 例如，事件型別(`type`)、事件來源(`source`)、事件識別碼(`event_id`)、事件時間(`time`)和事件資料(`data`)。

## 其他資源

- [AEM-Eventing Webhook](../assets/examples/webhook/aemeventing-webhook.tgz)原始程式碼可供您參考。
