---
title: 調試Dispatcher工具
description: Dispatcher Tools提供容器化的Apache Web Server環境，可用來模擬AEM為Cloud Services的AEM Publish服務的本機Dispatcher。 除錯Dispatcher工具的日誌和快取內容對於確保端到端應用程式和支援快取和安全配置AEM正確至關重要。
feature: Dispatcher
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5918
topic: 開發
role: 開發人員
level: 初學者，中級
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 1%

---


# 調試Dispatcher工具

Dispatcher Tools提供容器化的Apache Web Server環境，可用來模擬AEM為Cloud Services的AEM Publish服務的本機Dispatcher。
除錯Dispatcher工具的日誌和快取內容對於確保端到端應用程式和支援快取和安全配置AEM正確至關重要。

>[!NOTE]
>
>由於Dispatcher Tools基於容器，因此每次重新啟動時，先前的日誌和快取內容都會被銷毀。

## Dispatcher Tools日誌

Dispatcher Tools日誌可通過`stdout`或`bin/docker_run`命令獲得，或通過`/etc/https/logs`的Docker容器獲得更詳細的資訊。

有關如何直接訪問Dispatcher Tools&#39; Docker容器日誌的說明，請參見[ Dispatcher logs](./logs.md#dispatcher-logs)。

## Dispatcher Tools快取

### 訪問Docker容器中的日誌

Dispatcher快取可以直接訪問` /mnt/var/www/html`的Docker容器中。

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

Dispatcher日誌可以從`/mnt/var/www/html`的Docker容器複製到本地檔案系統，以便使用您喜愛的工具進行檢查。 請注意，這是時間點副本，不會提供快取的即時更新。

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```

