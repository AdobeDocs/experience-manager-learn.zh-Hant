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
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---


# 使用記錄檔除錯AEM SDK

存取AEM SDK的記錄檔時，AEM SDK本機快速入門Jar或Dispatcher Tools&#39;都能提供除錯AEM應用程式的重要深入資訊。

## AEM記錄檔

>[!VIDEO](https://video.tv.adobe.com/v/34334/?quality=12&learn=on)

記錄檔是除錯AEM應用程式的前線，但需視部署的AEM應用程式中的適當登入而定。 Adobe建議將本機開發和AEM當做雲端服務開發人員記錄設定盡可能保持類似，因為它可將AEM SDK本機快速入門和AEM當作雲端服務開發人員環境的記錄可見度標準化，以減少設定繁瑣和重新部署。

The [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) configures logging at the DEBUG level for your AEM application&#39;s Java packages for local development via the Sling Logger OSGi configuration found at

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

登錄到 `error.log`。

如果預設記錄不足以進行本端開發，則可透過AEM SDK的本端快速入門的「記錄支援」網頁主控台([/system/console/slinglog](http://localhost:4502/system/console/slinglog))來設定臨機記錄，但不建議將臨機變更持續存留至Git，除非AEM也需要這些相同的記錄設定作為雲端服務開發環境。 請記住，透過「記錄支援控制台」所做的變更會直接保存至AEM SDK的本機快速入門資料庫。

Java日誌語句可在檔案中 `error.log` 查看：

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

通常，將輸出串流到終端的「尾 `error.log` 部」會很有用。

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows需 [要第三方尾翼應用程式](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) ，或 [使用Powershell的Get-Content命令](https://stackoverflow.com/a/46444596/133936)。

## Dispatcher logs

調用調度程式日誌時將輸出 `bin/docker_run` 到stdout，但日誌可以直接訪問Docker包含中的日誌。

### 訪問Docker容器中的日誌

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

### 將Docker日誌複製到本地檔案系統

Dispatcher logs can be copied out of the Docker container at `/etc/httpd/logs` to to the local file system for inspection using your favorite log analysis tool. 請注意，這是時間點副本，不會即時更新記錄檔。

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

