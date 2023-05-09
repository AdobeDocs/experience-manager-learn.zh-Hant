---
title: 安裝並配置Tomcat
seo-title: Install and Configure Tomcat
description: 這是建立第一個互動式通信文檔的多步驟教程的第1部分。在本部分，我們將安裝TOMCAT，並在TOMCAT中部署sampleRest.war檔案。
uuid: c6d4c74c-ea16-4c63-92c9-182d087fd88c
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 4f400c22-6c96-4018-851c-70d988ce7c6c
topic: Development
role: Developer
level: Beginner
exl-id: f0f19838-1ade-4eda-b736-a9703a3916c2
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 0%

---

# 安裝並配置Tomcat {#install-and-configure-tomcat}

在本部分，我們安裝了TOMCAT，並在TOMCAT中部署了sampleRest.war檔案。 此WAR檔案公開的REST端點是資料源和表單資料模型的基礎。

要設定tomcat，請按照以下說明操作：

1. 下載並安裝JDK1.8。
2. 將JAVA_HOME設定為指向JDK1.8。
3. 下載 [tomcat](https://tomcat.apache.org/). 此war檔案已使用Tomcat 8.5.x版和9.0.x版進行測試。
4. 下載您的首選項的tomcat版本。 您可以在核心區段下載64位元的windows zip。
5. 將內容解壓縮至c:\tomcat。
6. 你應該在c驅動器中看到類似的東西 **c:\tomcat\apache-tomcat-8.5.27** 取決於您的tomcat版本
7. 建立名為&quot;CATALINA_HOME&quot;的環境變數，並將其值設定為tomcat安裝資料夾示例c:\tomcat\apache- tomcat-8.5.27
8. 將SampleRest.war檔案複製到Tomcat安裝的Webapps資料夾中
9. 啟動新的命令提示窗口。
10. 導覽至 &lt;tomcat install=&quot;&quot; folder=&quot;&quot;>\bin並運行startup.bat
11. 啟動tomcat後，通過測試WAR檔案公開的端點 [按一下這裡](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. 您應該會收到此呼叫的結果範例資料。

恭喜!!!。 您已設定tomcat並部署了SampleRest.war檔案。

## 後續步驟

[配置RESTful資料源](./parttwo.md)
