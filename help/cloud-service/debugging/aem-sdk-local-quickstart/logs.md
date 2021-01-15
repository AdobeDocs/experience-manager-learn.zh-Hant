---
title: 使用記錄檔除錯AEM SDK
description: 記錄檔是除錯AEM應用程式的前線，但需視部署的AEM應用程式中的適當登入而定。
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5252
translation-type: tm+mt
source-git-commit: 178ba3dbcb6f2050a9c56303bbabbcfcbead3e79
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 0%

---


# 使用記錄檔除錯AEM SDK

存取AEM SDK的記錄檔時，AEM SDK本機快速入門Jar或Dispatcher Tools&#39;都能提供除錯AEM應用程式的重要深入資訊。

## AEM記錄檔

>[!VIDEO](https://video.tv.adobe.com/v/34334/?quality=12&learn=on)

記錄檔是除錯AEM應用程式的前線，但需視部署的AEM應用程式中的適當登入而定。 Adobe建議將本機開發和AEM當做雲端服務開發人員記錄設定盡可能保持類似，因為它可將AEM SDK本機快速入門和AEM當作雲端服務開發人員環境的記錄可見度標準化，以減少設定繁瑣和重新部署。

[AEM Project Archetype](https://github.com/adobe/aem-project-archetype)會針對您的AEM應用程式的Java封裝，設定在DEBUG層級登入，以透過Sling Logger OSGi組態進行本機開發，請參閱

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

登錄到`error.log`。

如果預設記錄不足以進行本端開發，則可透過AEM SDK的本端快速入門的「記錄支援」網頁主控台([/system/console/slinglog](http://localhost:4502/system/console/slinglog))來設定臨機記錄，但不建議將臨機變更持續存留至Git，除非AEM也需要這些相同的記錄設定作為雲端服務開發環境。 請記住，透過「記錄支援控制台」所做的變更會直接保存至AEM SDK的本機快速入門資料庫。

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
