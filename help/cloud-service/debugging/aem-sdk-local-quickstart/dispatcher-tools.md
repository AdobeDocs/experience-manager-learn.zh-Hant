---
title: 偵錯Dispatcher工具
description: Dispatcher工具提供容器化Apache Web Server環境，可用於在本機模擬AEM as a Cloud Service的AEM Publish服務的Dispatcher。 偵錯Dispatcher工具的記錄和快取內容對於確保端對端AEM應用程式以及支援的快取和安全設定正確性至關重要。
feature: Dispatcher
jira: KT-5918
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: f0adf7a6-c7c2-449a-9fa5-402c54b812e5
duration: 56
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# 偵錯Dispatcher工具

Dispatcher工具提供容器化Apache Web Server環境，可用於在本機模擬AEM as a Cloud Service的AEM Publish服務的Dispatcher。

偵錯Dispatcher工具的記錄和快取內容對於確保端對端AEM應用程式以及支援的快取和安全設定正確性至關重要。

>[!NOTE]
>
>由於Dispatcher Tools是以容器為基礎，因此每次重新啟動時，都會摧毀先前的記錄和快取內容。

## Dispatcher工具記錄檔

Dispatcher工具記錄可透過`stdout`或`bin/docker_run`命令取得，或是在`/etc/https/logs`的Docker容器中取得更詳細的資料。

如需如何直接存取Dispatcher工具的Docker容器記錄的指示，請參閱[Dispatcher記錄](./logs.md#dispatcher-logs)。

## Dispatcher工具快取

### 存取Docker容器中的日誌

Dispatcher快取可以直接存取` /mnt/var/www/html`的Docker容器。

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

### 將Docker日誌複製到本機檔案系統

Dispatcher記錄檔可以從位於`/mnt/var/www/html`的Docker容器複製到本機檔案系統，以使用您最喜愛的工具進行檢查。 請注意，這是時間點副本，不會提供快取的即時更新。

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_ira

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```
