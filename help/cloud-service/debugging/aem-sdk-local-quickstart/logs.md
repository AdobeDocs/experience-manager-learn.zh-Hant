---
title: 使用記錄檔除錯AEM SDK
description: 記錄檔是AEM應用程式偵錯作業的最前線，但部署的AEM應用程式必須有充足的登入次數。
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5252
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 91aa4a10-47fe-4313-acd2-ca753e5484d9
duration: 430
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 0%

---

# 使用記錄檔除錯AEM SDK

存取AEM SDK的記錄檔(AEM SDK本機快速入門Jar或Dispatcher工具)可提供AEM應用程式偵錯的重要深入分析。

## AEM記錄

>[!VIDEO](https://video.tv.adobe.com/v/34334?quality=12&learn=on)

記錄檔是AEM應用程式偵錯作業的最前線，但部署的AEM應用程式必須有充足的登入次數。 Adobe建議儘可能保持本機開發和AEMas a Cloud Service開發記錄設定，因為它標準化AEM SDK本機Quickstart和AEMas a Cloud Service開發環境的記錄可見度，減少設定迂迴和重新部署。

此 [AEM專案原型](https://github.com/adobe/aem-project-archetype) 透過Sling Logger OSGi設定（可在以下位置找到），將AEM應用程式的Java套件的DEBUG層級記錄設定為本機開發

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

會記錄到 `error.log`.

如果預設記錄不足以用於本機開發，則可透過AEM SDK的本機Quickstart的「記錄支援」Web主控台([/system/console/slinglog](http://localhost:4502/system/console/slinglog))，但除非也在AEMas a Cloud Service開發環境中需要這些相同的記錄設定，否則不建議將臨機變更持續儲存到Git。 請記住，透過記錄支援主控台進行的變更會直接儲存至AEM SDK的本機Quickstart存放庫。

Java記錄陳述式可以在 `error.log` 檔案：

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

通常「追蹤」以下專案會很實用： `error.log` 將其輸出串流到終端機。

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows需要 [第三方尾部應用程式](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) 或使用 [Powershell的Get-Content命令](https://stackoverflow.com/a/46444596/133936).

## Dispatcher記錄

Dispatcher記錄檔會輸出到stdout，當 `bin/docker_run` 會叫用，但記錄檔可直接透過在Docker包含中存取。

### 存取Docker容器中的日誌{#dispatcher-tools-access-logs}

Dispatcher記錄檔可直接存取Docker容器，位於 `/etc/httpd/logs`.

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

_此 `<CONTAINER ID>` 在 `docker exec -it <CONTAINER ID> /bin/sh` 必須使用以下清單中的目標Docker容器ID替換： `docker ps` 命令。_


### 將Docker日誌複製到本機檔案系統{#dispatcher-tools-copy-logs}

可以從Docker容器複製Dispatcher日誌，位於 `/etc/httpd/logs` 到本機檔案系統以使用您最喜愛的記錄分析工具進行檢查。 請注意，這是時間點副本，不會提供記錄的即時更新。

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

_此 `<CONTAINER_ID>` 在 `docker cp <CONTAINER_ID>:/var/log/apache2 ./` 必須使用以下清單中的目標Docker容器ID替換： `docker ps` 命令。_
