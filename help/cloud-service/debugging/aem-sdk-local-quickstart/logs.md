---
title: 使用記錄檔對AEM SDK除錯
description: 記錄檔是AEM應用程式偵錯作業的最前線，但部署的AEM應用程式必須具備足夠的登入能力。
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

# 使用記錄檔對AEM SDK除錯

存取AEM SDK的記錄檔時，AEM SDK本機Quickstart Jar或Dispatcher工具可提供AEM應用程式除錯的重要分析。

## AEM記錄檔

>[!VIDEO](https://video.tv.adobe.com/v/34334?quality=12&learn=on)

記錄檔是AEM應用程式偵錯作業的最前線，但部署的AEM應用程式必須具備足夠的登入能力。 Adobe建議盡可能保留本機開發和AEMas a Cloud Service開發記錄設定，因為它可標準化AEM SDK本機Quickstart和AEM as a Cloud Service開發環境的記錄可見性，減少設定的扭曲和重新部署。

此 [AEM專案原型](https://github.com/adobe/aem-project-archetype) 會透過位於的Sling Logger OSGi設定，針對AEM應用程式的Java套件設定在DEBUG層級記錄以進行本機開發

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

會登入 `error.log`.

如果預設記錄不足於本機開發，則可透過AEM SDK的本機Quickstart的「記錄支援」Web主控台來設定臨機記錄，位於([/system/console/slinglog](http://localhost:4502/system/console/slinglog))，不過不建議將臨機變更持續保存至Git，除非AEMas a Cloud Service開發環境也需要相同的記錄設定。 請記得，透過記錄支援主控台所做的變更會直接保存至AEM SDK的本機Quickstart存放庫。

可在 `error.log` 檔案：

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

通常，「追蹤」 `error.log` 將其輸出流到終端。

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows需要 [第三方尾部應用程式](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) 或 [Powershell的Get-Content命令](https://stackoverflow.com/a/46444596/133936).

## Dispatcher記錄檔

當 `bin/docker_run` 叫用時，但可以直接透過存取Docker包含中的記錄。

### 存取Docker容器中的記錄{#dispatcher-tools-access-logs}

Dispatcher記錄檔可直接存取Docker容器中的 `/etc/httpd/logs`.

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

_此 `<CONTAINER ID>` in `docker exec -it <CONTAINER ID> /bin/sh` 必須替換為 `docker ps` 命令。_


### 將Docker日誌複製到本地檔案系統{#dispatcher-tools-copy-logs}

Dispatcher記錄檔可從Docker容器中複製，位於 `/etc/httpd/logs` 使用您最喜愛的日誌分析工具檢查本地檔案系統。 請注意，這是時間點副本，不會提供記錄檔的即時更新。

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

_此 `<CONTAINER_ID>` in `docker cp <CONTAINER_ID>:/var/log/apache2 ./` 必須替換為 `docker ps` 命令。_
