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
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 2%

---

# 使用記錄檔對AEM SDK除錯

存取AEM SDK的記錄檔時，AEM SDK本機Quickstart Jar或Dispatcher工具可提供AEM應用程式除錯的重要分析。

## AEM記錄檔

>[!VIDEO](https://video.tv.adobe.com/v/34334/?quality=12&learn=on)

記錄檔是AEM應用程式偵錯作業的最前線，但部署的AEM應用程式必須具備足夠的登入能力。 Adobe建議盡可能將本機開發和AEM作為Cloud Service開發記錄設定，因為這可標準化AEM SDK本機Quickstart和AEM作為Cloud Service開發環境的記錄可見性，減少設定扭曲和重新部署。

[AEM專案原型](https://github.com/adobe/aem-project-archetype)會透過位於的Sling Logger OSGi設定，針對AEM應用程式的Java套件，在DEBUG層級設定記錄，以進行本機開發

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

會記錄到`error.log`。

如果預設記錄不足於本機開發，則可透過AEM SDK的本機Quickstart「記錄支援」Web主控台(位於([/system/console/slinglog](http://localhost:4502/system/console/slinglog))設定臨機記錄，但不建議將隨選變更保留至Git，除非AEM上也需要這些相同的記錄設定作為Cloud Service開發環境。 請記得，透過記錄支援主控台所做的變更會直接保存至AEM SDK的本機Quickstart存放庫。

可在`error.log`檔案中查看Java日誌陳述式：

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

通常，將`error.log`串流輸出到終端會很有用。

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows需要[第三方尾部應用程式](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command)或使用[Powershell的Get-Content命令](https://stackoverflow.com/a/46444596/133936)。

## Dispatcher記錄檔

叫用`bin/docker_run`時，Dispatcher記錄檔會輸出至stdout，但Docker包含的中可以直接透過存取記錄檔。

### 存取Docker容器中的記錄{#dispatcher-tools-access-logs}

Dispatcher記錄檔可直接存取位於`/etc/httpd/logs`的Docker容器中。

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

_中 `<CONTAINER ID>` 的必 `docker exec -it <CONTAINER ID> /bin/sh` 須替換為命令中列出的目標Docker容器 `docker ps` ID。_


### 將Docker日誌複製到本地檔案系統{#dispatcher-tools-copy-logs}

Dispatcher記錄檔可從位於`/etc/httpd/logs`的Docker容器複製到本機檔案系統，以使用您最喜愛的記錄檔分析工具進行檢查。 請注意，這是時間點副本，不會提供記錄檔的即時更新。

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

_中 `<CONTAINER_ID>` 的必 `docker cp <CONTAINER_ID>:/var/log/apache2 ./` 須替換為命令中列出的目標Docker容器 `docker ps` ID。_
