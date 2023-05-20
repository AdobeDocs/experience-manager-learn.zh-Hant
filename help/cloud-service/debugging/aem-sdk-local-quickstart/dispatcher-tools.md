---
title: 調試Dispatcher工具
description: Dispatcher Tools提供了集裝箱化的Apache Web Server環境，可用AEM於本地模擬Cloud Services的AEM發佈服務的Dispatcher。 調試Dispatcher Tools的日誌和快取內容對於確保端到端應用程式以及支援快取和安全配置AEM是正確的至關重要。
feature: Dispatcher
kt: 5918
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: f0adf7a6-c7c2-449a-9fa5-402c54b812e5
source-git-commit: 4b47daf82e27f6bea4be30e3cdd132f497f4c609
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# 調試Dispatcher工具

Dispatcher Tools提供了集裝箱化的Apache Web Server環境，可用AEM於本地模擬Cloud Services的AEM發佈服務的Dispatcher。

調試Dispatcher Tools的日誌和快取內容對於確保端到端應用程式以及支援快取和安全配置AEM是正確的至關重要。

>[!NOTE]
>
>由於Dispatcher Tools基於容器，因此每次重新啟動它時，先前的日誌和快取內容都會被銷毀。

## Dispatcher Tools日誌

Dispatcher Tools日誌可通過 `stdout` 或 `bin/docker_run` 命令，或提供更多詳細資訊，可在Docker容器 `/etc/https/logs`。

請參閱 [調度程式日誌](./logs.md#dispatcher-logs) 有關如何直接訪問Dispatcher Tools&#39; Docker容器日誌的說明。

## Dispatcher Tools快取

### 訪問Docker容器中的日誌

Dispatcher快取可以直接訪問Docker容器中的 ` /mnt/var/www/html`。

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

可以從Docker容器中複製調度程式日誌，位於 `/mnt/var/www/html` 到本地檔案系統，以便使用您喜愛的工具進行檢查。 請注意，這是一個時間點拷貝，不會向快取提供即時更新。

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_ira

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```
