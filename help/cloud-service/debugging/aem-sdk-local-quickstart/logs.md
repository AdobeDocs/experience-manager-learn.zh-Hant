---
title: 使用日AEM志調試SDK
description: 日誌充當調試應用程式的前AEM沿，但取決於在已部署的應用程式中進行充分的AEM日誌記錄。
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5252
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 91aa4a10-47fe-4313-acd2-ca753e5484d9
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 2%

---

# 使用日AEM志調試SDK

訪問AEMSDK的日誌AEM（SDK本地快速啟動Jar或Dispatcher Tools）可提供調試應用程式的關鍵AEM見解。

## 日AEM志

>[!VIDEO](https://video.tv.adobe.com/v/34334?quality=12&learn=on)

日誌充當調試應用程式的前AEM沿，但取決於在已部署的應用程式中進行充分的AEM日誌記錄。 Adobe建議盡可能保持AEM本地開發和as a Cloud Service開發日誌配置的相似性，因為它使AEMSDK的本地快速啟動和as a Cloud Service的開發環境的日誌可見性正常化AEM，從而減少配置的扭曲和重新部署。

的 [項AEM目原型](https://github.com/adobe/aem-project-archetype) 配置在DEBUG級別為應用程式AEM的Java包進行日誌記錄，以便通過位於以下位置的Sling Logger OSGi配置進行本地開發

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

登錄到 `error.log`。

如果預設日誌記錄不足以進行本地開發，則可以通過AEMSDK的本地快速啟動日誌支援Web控制台配置即席日誌記錄，位於([/system/console/slinglog](http://localhost:4502/system/console/slinglog))，但建議不要將臨時更改保留到Git，除非在as a Cloud Service的開發環境中也需要這些AEM相同的日誌配置。 請記住，通過日誌支援控制台所做的更改會直接保留AEM到SDK的本地快速啟動儲存庫。

Java日誌語句可在 `error.log` 檔案：

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

通常，&quot;跟蹤&quot; `error.log` 將輸出流傳輸到終端。

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows需要 [第三方尾部應用](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) 或 [Powershell的Get-Content命令](https://stackoverflow.com/a/46444596/133936)。

## 調度程式日誌

將調度程式日誌輸出到stdout `bin/docker_run` 調用，但日誌可以直接訪問Docker包含中的日誌。

### 訪問Docker容器中的日誌{#dispatcher-tools-access-logs}

Dispatcher日誌可以直接訪問Docker容器中的 `/etc/httpd/logs`。

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker exec -it <CONTAINER ID> /bin/sh

/ # 
/ # cd /etc/httpd/logs
/ # ls
    dispatcher.log          healthcheck_access_log  httpd_access.log        httpd_error.log

# When finished viewing the logs files, exit the Docker container's shell
/# exit
```

_的 `<CONTAINER ID>` 在 `docker exec -it <CONTAINER ID> /bin/sh` 必須替換為從 `docker ps` 的子菜單。_


### 將Docker日誌複製到本地檔案系統{#dispatcher-tools-copy-logs}

可以從Docker容器中複製調度程式日誌，位於 `/etc/httpd/logs` 到本地檔案系統，以便使用您喜愛的日誌分析工具進行檢查。 請注意，這是一個時間點拷貝，不提供對日誌的即時更新。

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker cp -L <CONTAINER ID>:/etc/httpd/logs logs 
$ cd logs
$ ls
    dispatcher.log          healthcheck_access_log  httpd_access.log        httpd_error.log
```

_的 `<CONTAINER_ID>` 在 `docker cp <CONTAINER_ID>:/var/log/apache2 ./` 必須替換為從 `docker ps` 的子菜單。_
