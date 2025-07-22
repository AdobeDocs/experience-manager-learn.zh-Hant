---
title: Webhook和AEM活動
description: 瞭解如何透過webhook接收AEM事件，並檢閱事件詳細資訊，例如裝載、標題和中繼資料。
version: Experience Manager as a Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 358
last-substantial-update: 2023-01-29T00:00:00Z
jira: KT-14732
thumbnail: KT-14732.jpeg
exl-id: 00954d74-c4c7-4dac-8d23-7140c49ae31f
source-git-commit: e01eb7ff050321a70d84f8a613705799017dbf5d
workflow-type: tm+mt
source-wordcount: '533'
ht-degree: 1%

---

# Webhook和AEM活動

瞭解如何透過webhook接收AEM事件，並檢閱事件詳細資訊，例如裝載、標題和中繼資料。


>[!VIDEO](https://video.tv.adobe.com/v/3427051?quality=12&learn=on)


>[!IMPORTANT]
>
>此教學課程中的即時示範端點先前託管於[問題](https://glitch.com/)。 自2025年7月起，Glitch已終止託管服務，且無法再存取端點。
>&#x200B;>我們正在積極努力將示範移轉至其他平台。 教學課程內容會維持正確狀態，而且我們很快會提供更新的連結。
>&#x200B;>感謝您的理解和耐心。

使用您自己的webhook，直到即時示範端點再次可用為止。

## 先決條件

若要完成本教學課程，您需要：

- 已啟用[AEM事件的AEM as a Cloud Service環境](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment)。

- 已針對Adobe Developer Console事件設定[AEM專案](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console)。


## 存取webhook

若要存取Adobe提供的webhook，請遵循下列步驟：

- 確認您可以在新的瀏覽器分頁中存取[問題 — 託管的webhook](https://lovely-ancient-coaster.glitch.me/)。

  ![問題 — 託管的webhook](../assets/examples/webhook/glitch-hosted-webhook.png)

- 輸入您webhook的唯一名稱，例如`<YOUR_PETS_NAME>-aem-eventing`，然後按一下&#x200B;**連線**。 您應該會看到`Connected to: ${YOUR-WEBHOOK-URL}`訊息出現在畫面上。

  ![問題 — 建立webhook](../assets/examples/webhook/glitch-create-webhook.png)

- 記下&#x200B;**Webhook URL**。 在本教學課程的後半部分，您會用到它。

## 在Adobe Developer Console專案中設定webhook

若要在上述webhook URL上接收AEM活動，請按照以下步驟操作：

- 在[Adobe Developer Console](https://developer.adobe.com)中，導覽至您的專案並按一下以開啟專案。

- 在&#x200B;**產品與服務**&#x200B;區段下，按一下應將AEM活動傳送至webhook之所需活動卡片旁的省略符號`...`，然後選取&#x200B;**編輯**。

  ![Adobe Developer Console專案編輯](../assets/examples/webhook/adobe-developer-console-project-edit.png)

- 在新開啟的&#x200B;**設定事件註冊**&#x200B;對話方塊中，按一下&#x200B;**下一步**&#x200B;以繼續進行&#x200B;**如何接收事件**&#x200B;步驟。

  ![Adobe Developer Console專案設定](../assets/examples/webhook/adobe-developer-console-project-configure.png)

- 在&#x200B;**如何接收事件**&#x200B;步驟中，選取&#x200B;**Webhook**&#x200B;選項並貼上您先前從Glitch託管的Webhook複製的&#x200B;**Webhook URL**，然後按一下&#x200B;**儲存已設定的事件**。

  ![Adobe Developer Console專案Webhook](../assets/examples/webhook/adobe-developer-console-project-webhook.png)

- 在Glitch Webook頁面中，您應該會看到GET請求，這是Adobe I/O Events傳送以驗證webhook URL的挑戰請求。

  ![問題 — 挑戰要求](../assets/examples/webhook/glitch-challenge-request.png)


## 觸發AEM事件

若要從已在上述AEM專案中註冊的AEM as a Cloud Service環境觸發Adobe Developer Console事件，請遵循下列步驟：

- 透過[Cloud Manager](https://my.cloudmanager.adobe.com/)存取並登入您的AEM as a Cloud Service作者環境。

- 根據您的&#x200B;**訂閱事件**，建立、更新、刪除、發佈或取消發佈內容片段。

## 檢閱事件詳細資料

完成上述步驟後，您應該會看到正在傳送至webhook的AEM活動。 在Glitch webhook頁面中尋找POST要求。

![問題 — POST要求](../assets/examples/webhook/glitch-post-request.png)

以下是POST要求的主要詳細資料：

- 路徑： `/webhook/${YOUR-WEBHOOK-URL}`，例如`/webhook/AdobeTM-aem-eventing`

- 標頭： Adobe I/O Events傳送的要求標頭，例如：

```json
{
"connection": "close",
"x-forwarded-for": "34.205.178.127,::ffff:10.10.10.136,::ffff:10.10.84.114",
"x-forwarded-proto": "https,http,http",
"x-forwarded-port": "443,80,80",
"host": "lovely-ancient-coaster.glitch.me",
"content-length": "826",
"x-adobe-public-key2-path": "/prod/keys/pub-key-IkpzhSpTw0.pem",
"x-adobe-delivery-id": "18abfb47-d24a-4684-ade8-f442a3444033",
"x-adobe-provider": "aemsites_7ABB3E6A5A7491460A495D61@AdobeOrg_acct-aem-p46652-e1074060@adobe.com",
"x-adobe-public-key1-path": "/prod/keys/pub-key-Ptc2pD9vT9.pem",
"x-adobe-event-id": "a0f3fb7d-b02c-4612-aac6-e472b80af793",
"x-adobe-event-code": "aem.sites.contentFragment.modified",
"user-agent": "Adobe/1.0",
"x-adobe-digital-signature-2": "zGLso15+6PV6X6763/x6WqgxDlEXpkv5ty8q4njaq3aUngAI9VCcYonbScEjljRluzjZ05uMJmRfNxwjj60syxEJPuc0dpmMU635gfna7I4T7IaHs496wx4m2E5mvCM+aKbNQ+NPOutyTqI8Ovq29P2P87GIgMlGhAtOaxRVGNc6ksBxc2tCWbrKUhW8hPJ0sHphU499dN4TT32xrZaiRw4akT3M/hYydsA8dcWpJ7S4dpuDS21YyDHAB8s9Dawtr3fyPEyLgZzpwZDfCqQ8gdSCGqKscE4pScwqPkKOYCHDnBvDZVe583jhcZbHGjk7Ncp/FrgQk7avWsk5XlzcuA==",
"x-adobe-digital-signature-1": "QD7THFJ1vmJqD/BatIpzO6+ACQ9cSKPR7XVaW0LI7cN/xs7ucyri6dmkerOPe9EJpjGoqCg8rxWedrIRQB3lgVskChbHH3Ujx5YG0aTQLSd1Lsn5CFbW1U0l0GqId9Cnd6MccrqSznZXcdW1rMFuRk8+gqwabBifSaLbu3r30G5hmqQd72VtiYTE4m23O3jYIMiv62pRP+a+p4NjNj1XG320uRSry+BPniTjDJ6oN/Ng7aUEKML8idZ/ZTqeh/rJSrVO95UryUolFDRwDkRn5zKonbvhSLAeXzaPhvimWUHtldq9M1WTyRMpsBk8BRzaklxlq+woJ2UjYPUIEzjotw==",
"accept-encoding": "deflate,compress,identity",
"content-type": "application/cloudevents+json; charset=UTF-8",
"x-forwarded-host": "lovely-ancient-coaster.glitch.me",
"traceparent": "00-c27558588d994f169186ca6a3c6607d4-a7e7ee36625488d4-01"
}
```

- body/payload： Adobe I/O Events傳送的要求內文，例如：

```json
{
  "specversion": "1.0",
  "type": "aem.sites.contentFragment.modified",
  "source": "acct:aem-p46652-e1074060@adobe.com",
  "id": "bf922a49-9db4-4377-baf4-70e96e15c45f",
  "time": "2023-12-12T20:36:43.583228Z",
  "dataschema": "https://ns.adobe.com/xdm/aem/sites/events/content-fragment-modified.json",
  "datacontenttype": "application/json",
  "data": {
    "user": {
      "imsUserId": "933E1F8A631CAA0F0A495E53@80761f6e631c0c7d495fb3.e",
      "principalId": "xxx@adobe.com",
      "displayName": "First LastName",
    },
    "path": "/content/dam/wknd-shared/en/adventures/beervana-portland/beervana-in-portland",
    "model": {
      "id": "/conf/wknd-shared/settings/dam/cfm/models/adventure"
    },
    "id": "9a2d3e6a-efda-4079-a86e-0ef2ede692da",
    "properties": [
      {
        "name": "groupSize",
        "changeType": "modified"
      }
    ]
  },
  "event_id": "a0f3fb7d-b02c-4612-aac6-e472b80af793",
  "recipient_client_id": "f51ea733ba404db299fefbf285dc1c42"
}
```

您可以看到AEM事件詳細資料具有在webhook中處理事件所需的所有必要資訊。 例如，事件型別(`type`)、事件來源(`source`)、事件識別碼(`event_id`)、事件時間(`time`)和事件資料(`data`)。

## 其他資源

- [AEM-Eventing Webhook](../assets/examples/webhook/aemeventing-webhook.tgz)原始程式碼可供您參考。
