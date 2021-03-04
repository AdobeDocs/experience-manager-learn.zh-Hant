---
title: 使用記AEM錄除錯SDK
description: 記錄檔是除錯應用程式的前AEM沿，但需視部署應用程式的登入程AEM式而定。
feature: 開發人員工具
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5252
topic: 開發
role: 開發人員
level: 初學者，中級
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 3%

---


# 使用記AEM錄除錯SDK

存取AEMSDK記錄檔(AEMSDK本機快速入門Jar或Dispatcher Tools)可提供除錯應用程式的重要深入資訊AEM。

## 記AEM錄

>[!VIDEO](https://video.tv.adobe.com/v/34334/?quality=12&learn=on)

記錄檔是除錯應用程式的前AEM沿，但需視部署應用程式的登入程AEM式而定。 Adobe建議盡可能保持本AEM機開發和Cloud Service開發記錄設定的類似性，因為它可標準化AEM SDK本機快速入門和Cloud Service開發AEM環境的記錄可見度，減少設定的繁瑣和重新部署。

[AEM Project Archetype](https://github.com/adobe/aem-project-archetype)會在DEBUG層級設定登入您應用程式的Java封裝，以透過Sling Logger OSGi組態，位於

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

登錄到`error.log`。

如果預設記錄不足以進行本機開發，則可透過AEMSDK本機快速入門的「記錄支援」網頁主控台([/system/console/slinglog](http://localhost:4502/system/console/slinglog))來設定臨機記錄，但不建議將臨機變更持續存留至Git，除非Cloud Service開發環境也需要這些相同的記錄設定AEM。 請記住，透過「記錄支援控制台」所做的變更會直接保存至AEMSDK的本機快速入門資料庫。

Java日誌語句可以在`error.log`檔案中查看：

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

通常，將輸出串流至終端的`error.log`「尾隨」會很有用。

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows需要[第三方尾部應用程式](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command)或使用[Powershell的Get-Content命令](https://stackoverflow.com/a/46444596/133936)。

## Dispatcher logs

調用`bin/docker_run`時，調度程式日誌將輸出到stdout，但Docker包含的日誌可以直接訪問。

### 訪問Docker容器中的日誌{#dispatcher-tools-access-logs}

Dispatcher日誌可以直接訪問`/etc/httpd/logs`的Docker容器中。

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

_必 `<CONTAINER ID>` 須 `docker exec -it <CONTAINER ID> /bin/sh` 以命令中列出的目標Docker CONTAINER ID替換 `docker ps` 中。_


### 將Docker日誌複製到本地檔案系統{#dispatcher-tools-copy-logs}

Dispatcher logs can be copied out of the Docker container at `/etc/httpd/logs` to the local file system for inspection using your favorite log analysis tool. 請注意，這是時間點副本，不會即時更新記錄檔。

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

_必 `<CONTAINER_ID>` 須 `docker cp <CONTAINER_ID>:/var/log/apache2 ./` 以命令中列出的目標Docker CONTAINER ID替換 `docker ps` 中。_
