---
title: 如何在AEM as a Cloud Service中的領導者執行個體上執行工作
description: 瞭解如何在AEM as a Cloud Service中的領導者執行個體上執行工作。
version: Cloud Service
topic: Development
feature: OSGI, Cloud Manager
role: Architect, Developer
level: Intermediate, Experienced
doc-type: Article
duration: 0
last-substantial-update: 2024-10-23T00:00:00Z
jira: KT-16399
thumbnail: KT-16399.jpeg
source-git-commit: 7dca86137d476418c39af62c3c7fa612635c0583
workflow-type: tm+mt
source-wordcount: '557'
ht-degree: 0%

---


# 如何在AEM as a Cloud Service中的領導者執行個體上執行工作

瞭解如何在AEM Author服務中的領導執行個體上執行作為AEM as a Cloud Service一部分的工作，並瞭解如何將其設定為只執行一次。

Sling作業是指在背景執行的非同步工作，旨在處理系統或使用者觸發的事件。 依照預設，這些工作會平均分配到叢集中的所有執行處理(pod)。

如需詳細資訊，請參閱[Apache Sling事件和作業處理](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)。

## 建立及處理工作

為了示範，讓我們建立簡單的&#x200B;_工作，指示工作處理器記錄訊息_。

### 建立工作

使用以下程式碼來&#x200B;_建立_ Apache Sling工作：

```java
package com.adobe.aem.guides.wknd.core.sling.jobs.impl;

import java.util.HashMap;
import java.util.Map;

import org.apache.sling.event.jobs.JobManager;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(immediate = true)
public class SimpleJobCreaterImpl {

    private static final Logger log = LoggerFactory.getLogger(SimpleJobCreaterImpl.class);

    // Define the topic on which the job will be created
    protected static final String TOPIC = "wknd/simple/job/topic";

    // Inject a JobManager
    @Reference
    private JobManager jobManager;

    @Activate
    protected final void activate() throws Exception {
        log.info("SimpleJobCreater activated successfully");
        createJob();
        log.info("SimpleJobCreater created a job");
    }

    private void createJob() {
        // Create a job and add it on the above defined topic
        Map<String, Object> jobProperties = new HashMap<>();
        jobProperties.put("action", "log");
        jobProperties.put("message", "Job metadata is: Created in activate method");
        jobManager.addJob(TOPIC, jobProperties);
    }
}
```

上述程式碼要注意的重點為：

- 工作承載有兩個屬性： `action`和`message`。
- 使用[JobManager](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/apache/sling/event/jobs/JobManager.html)的`addJob(...)`方法，工作已新增至主題`wknd/simple/job/topic`。

### 處理工作

使用以下程式碼來&#x200B;_處理_&#x200B;上述Apache Sling工作：

```java
package com.adobe.aem.guides.wknd.core.sling.jobs.impl;

import org.apache.sling.event.jobs.Job;
import org.apache.sling.event.jobs.consumer.JobConsumer;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(service = JobConsumer.class, property = {
        JobConsumer.PROPERTY_TOPICS + "=" + SimpleJobCreaterImpl.TOPIC
}, immediate = true)
public class SimpleJobConsumerImpl implements JobConsumer {

    private static final Logger log = LoggerFactory.getLogger(SimpleJobConsumerImpl.class);

    @Override
    public JobResult process(Job job) {
        // Get the action and message properties
        String action = job.getProperty("action", String.class);
        String message = job.getProperty("message", String.class);

        // Log the message
        if ("log".equals(action)) {
            log.info("Processing WKND Job, and {}", message);
        }

        // Return a successful result
        return JobResult.OK;
    }

}
```

上述程式碼要注意的重點為：

- `SimpleJobConsumerImpl`類別實作`JobConsumer`介面。
- 這是已登入以使用主題`wknd/simple/job/topic`中的工作的服務。
- `process(...)`方法透過記錄工作承載的`message`屬性來處理工作。

### 預設工作處理

當您將上述程式碼部署至AEM as a Cloud Service環境，並在AEM Author服務(以具有多個AEM Author JVM的叢集方式運作)上執行時，工作將在每個AEM Author例項(pod)上執行一次，這表示建立的工作數量將與pod的數量相符。 Pod的數量一律會超過一個（適用於非RDE環境），但會隨著AEM as a Cloud Service的內部資源管理而波動。

工作會在每個AEM Author執行個體(pod)上執行，因為`wknd/simple/job/topic`與AEM的主佇列相關聯，這會將工作分散到所有可用的執行個體。

如果工作負責變更狀態（例如建立或更新資源或外部服務），這通常會有問題。

如果您希望工作在AEM Author服務上只執行一次，請新增如下所述的[工作佇列設定](#how-to-run-a-job-on-the-leader-instance)。

您可以在[Cloud Manager](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/logs#cloud-manager)中檢閱AEM Author服務的記錄檔，以驗證它。

![由所有執行個體處理的工作](./assets/run-job-once/job-processed-by-all-instances.png)


您應會看到：

```
<DD.MM.YYYY HH:mm:ss.SSS> [cm-pxxxx-exxxx-aem-author-68775db964-nxxcx] *INFO* [sling-oak-observation-15] org.apache.sling.event.impl.jobs.queues.JobQueueImpl.<main queue> Starting job queue <main queue>
<DD.MM.YYYY HH:mm:ss.SSS> INFO [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method

<DD.MM.YYYY HH:mm:ss.SSS> [cm-pxxxx-exxxx-aem-author-68775db964-r4zk7] *INFO* [sling-oak-observation-11] org.apache.sling.event.impl.jobs.queues.JobQueueImpl.<main queue> Starting job queue <main queue>
<DD.MM.YYYY HH:mm:ss.SSS> INFO [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method
```

有兩個記錄專案，每個AEM Author執行個體（`68775db964-nxxcx`和`68775db964-r4zk7`）各一個，表示每個執行個體(pod)已處理工作。

## 如何在領導者執行個體上執行工作

若要在AEM Author服務上執行工作&#x200B;_一次_，請建立型別為&#x200B;**Ordered**&#x200B;的新Sling工作佇列，並將您的工作主題(`wknd/simple/job/topic`)與此佇列建立關聯。 使用此設定時，將只允許前置AEM Author例項(pod)處理工作。

在您的AEM專案的`ui.config`模組中，建立OSGi設定檔(`org.apache.sling.event.jobs.QueueConfiguration~wknd.cfg.json`)並將其儲存在`ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author`資料夾。

```json
{
    "queue.name":"WKND Queue - ORDERED",
    "queue.topics":[
      "wknd/simple/job/topic"
    ],
    "queue.type":"ORDERED",
    "queue.retries":1,
    "queue.maxparallel":1.0
  }
```

上述組態中需注意的重點為：

- 佇列主題已設定為`wknd/simple/job/topic`。
- 佇列型別已設定為`ORDERED`。
- 平行作業的最大數量設為`1`。

部署上述設定後，工作將僅由領導者執行個體處理，確保其僅在整個AEM Author服務中執行一次。

```
<DD.MM.YYYY HH:mm:ss.SSS> [cm-pxxxx-exxxx-aem-author-7475cf85df-qdbq5] *INFO* [FelixLogListener] Events.Service.org.apache.sling.event Service [QueueMBean for queue WKND Queue - ORDERED,7755, [org.apache.sling.event.jobs.jmx.StatisticsMBean]] ServiceEvent REGISTERED
<DD.MM.YYYY HH:mm:ss.SSS> INFO [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method
<DD.MM.YYYY HH:mm:ss.SSS> [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method
```
