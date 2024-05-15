---
title: 新增自訂網域名稱
description: 瞭解如何將自訂網域名稱新增到AEM as a Cloud Service託管的網站。
version: Cloud Service
feature: Cloud Manager, Operations
topic: Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
duration: 1042
last-substantial-update: 2024-03-12T00:00:00Z
jira: KT-15121
thumbnail: KT-15121.jpeg
exl-id: 8936c3ae-2daf-4d0f-b260-28376ae28087
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '701'
ht-degree: 0%

---

# 新增自訂網域名稱

瞭解如何將自訂網域名稱新增到AEMas a Cloud Service網站。

在本教學課程中，範例的品牌 [AEM WKND](https://github.com/adobe/aem-guides-wknd) 新增HTTPS可定址的自訂網域名稱來增強網站 `wknd.enablementadobe.com` 具有傳輸層安全性(TLS)。

>[!VIDEO](https://video.tv.adobe.com/v/3427903?quality=12&learn=on)

高層級步驟為：

![高自訂網域名稱](./assets/add-custom-domain-name-steps.png){width="800" zoomable="yes"}

## 先決條件

>[!VIDEO](https://video.tv.adobe.com/v/3427909?quality=12&learn=on)

- [OpenSSL](https://www.openssl.org/) 和 [dig](https://www.isc.org/blogs/dns-checker/) 已安裝在您的本機電腦上。
- 存取協力廠商服務：
   - 憑證授權單位(CA) — 為您的網站網域要求已簽署的憑證，例如 [數字憑證](https://www.digicert.com/)
   - 網域名稱系統(DNS)託管服務 — 為您的自訂網域新增DNS記錄，例如Azure DNS或AWS Route 53。
- 存取目標 [AdobeCloud Manager](https://my.cloudmanager.adobe.com/) 作為企業所有者或部署管理員角色。
- 範例 [AEM WKND](https://github.com/adobe/aem-guides-wknd) 將站點部署到的AEMCS環境 [生產計畫](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs) 型別。

如果您無法存取協力廠商服務， _與您的安全性或託管團隊共同作業以完成這些步驟_.

## 產生SSL憑證

>[!VIDEO](https://video.tv.adobe.com/v/3427908?quality=12&learn=on)

您有兩個選項：

- 使用 `openssl` 命令列工具 — 您可以為網站網域產生私密金鑰和憑證申請檔(CSR)。 若要要求簽署的憑證，請將CSR提交給憑證授權單位(CA)。

- 您的託管團隊會提供您網站所需的私密金鑰和已簽署的憑證。

讓我們來回顧第一個選項的步驟。

若要產生私密金鑰和CSR，請執行以下命令，並在出現提示時提供所需資訊：

```bash
# Generate a private key and a CSR
$ openssl req -newkey rsa:2048 -keyout <YOUR-SITE-NAME>.key -out <YOUR-SITE-NAME>.csr -nodes
```

若要請求已簽署的憑證，請依照相關檔案的說明，將產生的CSR提供給CA。 CA簽署CSR後，您會收到簽署的憑證檔案。

### 檢閱簽署的憑證

在將已簽署的憑證新增到Cloud Manager之前檢視它是好的做法。 您可以使用以下命令來檢閱憑證詳細資訊：

```bash
# Review the certificate details
$ openssl crl2pkcs7 -nocrl -certfile <YOUR-SIGNED-CERT>.crt | openssl pkcs7 -print_certs -noout
```

簽署的憑證可能包含憑證鏈，其中包括根和中間憑證以及終端實體憑證。

AdobeCloud Manager接受終端實體憑證和憑證鏈 _在個別的表單欄位中_，因此您必須從已簽署的憑證中擷取終端實體憑證和憑證鏈結。

在本教學課程中， [數字憑證](https://www.digicert.com/) 簽章憑證，簽發對象： `*.enablementadobe.com` 網域為例。 透過在文字編輯器中開啟已簽署的憑證並複製以下內容來擷取終端實體和憑證鏈： `-----BEGIN CERTIFICATE-----` 和 `-----END CERTIFICATE-----` 標籤。

## 在Cloud Manager中新增SSL憑證

>[!VIDEO](https://video.tv.adobe.com/v/3427906?quality=12&learn=on)

若要在Cloud Manager中新增SSL憑證，請遵循 [新增SSL憑證](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-ssl-certificates/add-ssl-certificate) 檔案。

## 網域名稱驗證

>[!VIDEO](https://video.tv.adobe.com/v/3427905?quality=12&learn=on)

若要驗證網域名稱，請執行下列步驟：

- 按照以下步驟在Cloud Manager中新增網域名稱 [新增自訂網域名稱](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/add-custom-domain-name) 檔案。
- 新增AEM特定 [TXT記錄](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/add-text-record) 在您的DNS託管服務中。
- 透過使用查詢DNS伺服器來驗證上述步驟 `dig` 命令。

```bash
# General syntax, the `_aemverification` is prefix provided by Adobe
$ dig _aemverification.[YOUR-DOMAIN-NAME] -t txt

# This tutorial specific example, as the subdomain `wknd.enablementadobe.com` is used
$ dig _aemverification.wknd.enablementadobe.com -t txt
```

成功的回應範例看起來像這樣：

```bash
; <<>> DiG 9.10.6 <<>> _aemverification.wknd.enablementadobe.com -t txt
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 8636
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1220
;; QUESTION SECTION:
;_aemverification.wknd.enablementadobe.com. IN TXT

;; ANSWER SECTION:
_aemverification.wknd.enablementadobe.com. 3600    IN TXT "adobe-aem-verification=wknd.enablementadobe.com/105881/991000/bef0e843-9280-4385-9984-357ed9a4217b"

;; Query time: 81 msec
;; SERVER: 153.32.14.247#53(153.32.14.247)
;; WHEN: Tue Mar 12 15:54:25 EDT 2024
;; MSG SIZE  rcvd: 181
```

在本教學課程中，以Azure DNS為例。 若要新增TXT記錄，您必須遵循DNS託管服務的檔案。

檢閱 [檢查網域名稱狀態](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/check-domain-name-status) 說明檔案（如果有問題）。

## 設定DNS記錄

>[!VIDEO](https://video.tv.adobe.com/v/3427907?quality=12&learn=on)

若要設定自訂網域的DNS記錄，請按照以下步驟操作，

- 根據網域型別(如根網域(APEX)或子網域(CNAME))判斷DNS記錄型別（CNAME或APEX），並遵循 [正在設定DNS設定](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/configure-dns-settings) 檔案。
- 在您的DNS託管服務中新增DNS記錄。
- 遵循以下步驟觸發DNS記錄驗證 [檢查DNS記錄狀態](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/check-dns-record-status) 檔案。

在本教學課程中，作為 **子網域** `wknd.enablementadobe.com` 使用，指向的CNAME記錄型別 `cdn.adobeaemcloud.com` 「 」已新增。

不過，如果您使用 **根網域**，您必須新增指向Adobe提供之特定IP位址的APEX記錄型別（亦稱為A、ALIAS或ANAME）。

## 網站驗證

>[!VIDEO](https://video.tv.adobe.com/v/3427904?quality=12&learn=on)

若要確認可使用自訂網域名稱存取網站，請開啟網頁瀏覽器並導覽至自訂網域URL。 請確定網站可以存取，且瀏覽器會以掛鎖圖示顯示安全連線。

## 端對端視訊

您也可以觀看端對端影片，其中示範將自訂網域名稱新增到AEMas a Cloud Service託管網站的概述、先決條件和上述步驟。

>[!VIDEO](https://video.tv.adobe.com/v/3427817?quality=12&learn=on)
