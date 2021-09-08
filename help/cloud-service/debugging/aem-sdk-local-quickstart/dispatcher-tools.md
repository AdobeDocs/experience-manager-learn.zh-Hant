---
title: 偵錯Dispatcher工具
description: Dispatcher工具提供容器化的Apache Web Server環境，可用來在本機將AEM模擬為Cloud Services的AEM Publish服務的Dispatcher。 對於確保端對端AEM應用程式以及支援的快取和安全設定正確，除錯Dispatcher工具的記錄檔和快取內容可能至關重要。
feature: Dispatcher
kt: 5918
topic: Development
role: Developer
level: Beginner, Intermediate
source-git-commit: 0737cd2410b48dbaa9b6dfaaa27b854d44536f15
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---


# 偵錯Dispatcher工具

Dispatcher工具提供容器化的Apache Web Server環境，可用來在本機將AEM模擬為Cloud Services的AEM Publish服務的Dispatcher。

對於確保端對端AEM應用程式以及支援的快取和安全設定正確，除錯Dispatcher工具的記錄檔和快取內容可能至關重要。

>[!NOTE]
>
>由於Dispatcher工具是以容器為基礎，因此每次重新啟動時，先前的記錄和快取內容都會遭到破壞。

## Dispatcher工具記錄檔

Dispatcher工具記錄檔可透過`stdout`或`bin/docker_run`命令使用，或透過更詳細的資訊，可在位於`/etc/https/logs`的Docker容器中使用。

如需如何直接存取Dispatcher工具的Docker容器記錄檔的指示，請參閱[Dispatcher記錄檔](./logs.md#dispatcher-logs)。

## Dispatcher工具快取

### 存取Docker容器中的記錄

Dispatcher快取可直接存取位於` /mnt/var/www/html`的Docker容器中。

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

Dispatcher記錄檔可從`/mnt/var/www/html`的Docker容器複製到本機檔案系統，以使用您最喜愛的工具進行檢查。 請注意，這是時間點副本，不提供快取的即時更新。

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_ira

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```

