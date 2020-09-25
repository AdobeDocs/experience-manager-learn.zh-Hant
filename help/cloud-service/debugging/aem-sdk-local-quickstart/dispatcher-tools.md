---
title: 調試Dispatcher工具
description: Dispatcher Tools提供容器化的Apache Web Server環境，可用來將AEM模擬為Cloud Services的AEM Publish服務的Dispatcher本機。 除錯Dispatcher Tools的記錄檔和快取內容對於確保端對端AEM應用程式以及支援快取和安全性組態正確至關重要。
feature: dispatcher, aem-sdk
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5918
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 0%

---


# 調試Dispatcher工具

Dispatcher Tools提供容器化的Apache Web Server環境，可用來將AEM模擬為Cloud Services的AEM Publish服務的Dispatcher本機。
除錯Dispatcher Tools的記錄檔和快取內容對於確保端對端AEM應用程式以及支援快取和安全性組態正確至關重要。

>[!NOTE]
>
>由於Dispatcher Tools基於容器，因此每次重新啟動時，先前的日誌和快取內容都會被銷毀。

## Dispatcher Tools日誌

Dispatcher Tools日誌可通過或命 `stdout` 令獲 `bin/docker_run` 得，或在Docker容器()中提供更多詳細資訊 `/etc/https/logs`。

有關如 [何直接訪問Dispatcher Tools](./logs.md#dispatcher-logs) &#39; Docker容器日誌的說明，請參閱Dispatcher日誌。

## Dispatcher Tools快取

### 訪問Docker容器中的日誌

Dispatcher快取可直接在Docker容器中存取，位址為 ` /mnt/var/www/html`。

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker exec -it <CONTAINER ID> /bin/sh

/ # 
/ # cd /mnt/var/www/html

# When finished viewing the cache, exit the Docker container's shell
/# exit
```

### 將Docker日誌複製到本地檔案系統

Dispatcher日誌可從位於的Docker容器複製到本 `/mnt/var/www/html` 地檔案系統，以便使用您喜愛的工具進行檢查。 請注意，這是時間點副本，不會提供快取的即時更新。

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```

