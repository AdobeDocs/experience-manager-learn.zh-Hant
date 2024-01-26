---
title: Webhook和AEM活動
description: 瞭解如何在webhook上接收AEM事件，並檢閱事件詳細資訊，例如裝載、標題和中繼資料。
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 156
last-substantial-update: 2023-12-07T00:00:00Z
jira: KT-14732
thumbnail: KT-14732.jpeg
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 0%

---


# Webhook和AEM活動

瞭解如何在webhook上接收AEM事件，並檢閱事件詳細資訊，例如裝載、標題和中繼資料。

在此範例中，利用Adobe提供的 _託管webhook_ 可讓您接收AEM事件，而不需要設定自己的webhook。 此Adobe提供的webhook託管於 [故障](https://glitch.com/)，此平台以提供Web式環境而聞名，有助於建置和部署Web應用程式。 不過，您也可以選擇使用您自己的webhook選項（若偏好使用）。

## 先決條件

若要完成本教學課程，您需要：

- AEMas a Cloud Service環境搭配 [AEM事件已啟用](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- [為AEM事件設定的Adobe Developer Console專案](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).

## 存取webhook

若要存取Adobe提供的webhook，請按照以下步驟操作：

- 確認您可存取 [問題 — 託管的webhook](https://lovely-ancient-coaster.glitch.me/) 在新的瀏覽器標籤中。

  ![問題 — 託管的webhook](../assets/examples/webhook/glitch-hosted-webhook.png)

- 輸入webhook的唯一名稱，例如 `<YOUR_PETS_NAME>-aem-eventing` 並按一下 **連線**. 您應該會看到 `Connected to: ${YOUR-WEBHOOK-URL}` 訊息出現在畫面上。

  ![問題 — 建立webhook](../assets/examples/webhook/glitch-create-webhook.png)

- 記下 **Webhook URL**. 在本教學課程的後半部分，您會用到它。

## 在Adobe Developer主控台專案中設定webhook

若要在上述webhook URL上接收AEM活動，請按照以下步驟操作：

- 在 [Adobe Developer Console](https://developer.adobe.com)，導覽至您的專案，然後按一下以開啟專案。

- 在 **產品與服務** 部分，按一下省略符號 `...` 位於所需事件卡片旁，該卡片應將AEM事件傳送至webhook並選取 **編輯**.

  ![Adobe Developer Console專案編輯](../assets/examples/webhook/adobe-developer-console-project-edit.png)

- 在新開啟的 **設定事件註冊** 對話方塊，按一下 **下一個** 以繼續前往 **如何接收事件** 步驟。

  ![Adobe Developer主控台專案設定](../assets/examples/webhook/adobe-developer-console-project-configure.png)

- 在 **如何接收事件** 步驟，選取 **Webhook** 並貼上 **Webhook URL** 您先前從Glitch託管的webhook中複製，然後按一下 **儲存已設定的事件**.

  ![Adobe Developer主控台專案Webhook](../assets/examples/webhook/adobe-developer-console-project-webhook.png)

- 在Glitch Webook頁面中，您應該會看到GET請求，這是Adobe I/O事件傳送以驗證webhook URL的挑戰請求。

  ![問題 — 挑戰要求](../assets/examples/webhook/glitch-challenge-request.png)


## 觸發AEM事件

若要從已在上述Adobe Developer Console專案中註冊的AEMas a Cloud Service環境觸發AEM事件，請遵循下列步驟：

- 透過存取和登入您的AEMas a Cloud Service作者環境 [Cloud Manager](https://my.cloudmanager.adobe.com/).

- 根據您的 **訂閱的事件**、建立、更新、刪除、發佈或取消發佈內容片段。

## 檢閱事件詳細資料

完成上述步驟後，您應該會看到AEM活動已傳送至webhook。 在Glitch webhook頁面中尋找POST請求。

![問題 — POST要求](../assets/examples/webhook/glitch-post-request.png)

以下是POST請求的主要詳細資料：

- 路徑： `/webhook/${YOUR-WEBHOOK-URL}`，例如 `/webhook/AdobeTM-aem-eventing`

- 標頭：由Adobe I/O事件傳送的請求標頭，例如：

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

- body/payload：Adobe I/O事件傳送的要求內文，例如：

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

您可以看到AEM事件詳細資料具有在webhook中處理事件所需的所有必要資訊。 例如，事件型別(`type`)，事件來源(`source`)，事件id (`event_id`)，事件時間(`time`)和事件資料(`data`)。

## 其他資源

- [Webhook原始碼問題](https://glitch.com/edit/#!/lovely-ancient-coaster) 可供參考。
