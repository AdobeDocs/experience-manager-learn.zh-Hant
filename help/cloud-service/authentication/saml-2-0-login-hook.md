---
title: SAML 2.0自定義登錄掛接
description: 瞭解如何為開發自定義SAML 2.0登錄掛AEM接。
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
last-substantial-update: 2025-03-11T00:00:00Z
duration: 520
source-git-commit: 34f098de6bd15875e5534250b28c08bdb62e74fa
workflow-type: tm+mt
source-wordcount: '977'
ht-degree: 0%

---


# SAML 2.0登錄掛接

瞭解如何為開發自定義SAML 2.0登錄掛AEM接。 本教程提供了逐步說明，以建立與SAML 2.0身份提供程式整合的自定義登錄掛接，從而允許用戶使用其SAML憑據進行身份驗證。

如果IDP無法在SAML聲明中發送用戶配置檔案資料和用戶組成員身份，或者如果資料需要在同步之前轉換到AEM，則可以實現自定義SAML掛接以擴展SAML驗證過程。 SAML掛接允許在驗證流程期間自定義組成員分配、修改用戶配置檔案屬性以及添加自定義業務邏輯。

>[!NOTE]
>**AEM as a Cloud Service**&#x200B;和&#x200B;**AEM LTS**&#x200B;支援自訂SAML鉤點。 舊版AEM不提供此功能。

## 常見使用案例

當需要執行以下動作時，自訂SAML鉤子會很有用：

+ 超出SAML宣告中所提供的自訂商業邏輯，以動態方式指派群組成員資格
+ 在將用戶配置檔案資料同步到之前轉換或豐富該數AEM據
+ 將複雜的SAML屬性結構映射AEM到用戶屬性
+ 實施自定義授權規則或條件組分配
+ 在SAML驗證期間添加自定義日誌記錄或審核
+ 在驗證過程中與外部系統整合

## `SamlHook` OSGi服務介面

`com.adobe.granite.auth.saml.spi.SamlHook`介面提供在SAML驗證程式的不同階段叫用的兩種掛接方法：

### `postSamlValidationProcess()`方法

此方法在&#x200B;**後調用**,SAML響應已驗證，但在&#x200B;**前**&#x200B;用戶同步進程啟動。 這是修改SAML斷言資料（如添加或轉換屬性）的理想位置。

```java
public void postSamlValidationProcess(
    HttpServletRequest request, 
    Assertion assertion, 
    Message samlResponse)
```

#### 使用案例

+ 向聲明添加其他組成員身份
+ 在同步屬性值之前轉換屬性值
+ 利用來自外部源的資料豐富斷言
+ 驗證自定義業務規則

### `postSyncUserProcess()`方法

此方法在&#x200B;**之後調用**，用戶同步過程已完成。 此掛接可用於在建立或更新用戶AEM後執行其他操作。

```java
public void postSyncUserProcess(
    HttpServletRequest request, 
    HttpServletResponse response, 
    Assertion assertion,
    AuthenticationInfo authenticationInfo, 
    String samlResponse)
```

#### 使用案例

+ 更新標準同步處理未涵蓋的其他使用者設定檔屬性
+ 在AEM中建立或更新自訂使用者相關的資源
+ 使用者驗證後觸發工作流程或通知
+ 記錄自訂驗證事件

**重要資訊：**&#x200B;要修改儲存庫中的用戶屬性，掛接實現需要：

+ 通過`SlingRepository`注入的`@Reference`引用
+ 已配置具有相應權限的[服務用戶](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/cloud-service/developing/advanced/service-users)（在「Apache Sling服務用戶映射器服務修正」中配置）
+ 使用try-catch-finally區塊進行適當的工作階段管理

## 實施自訂SAML鉤點

下列步驟概述如何建立和部署自訂SAML鉤點。

### 建立SAML鉤點實作

在實現`com.adobe.granite.auth.saml.spi.SamlHook`介面AEM的項目中建立新Java類：

```java
package com.mycompany.aem.saml;

import com.adobe.granite.auth.saml.spi.Assertion;
import com.adobe.granite.auth.saml.spi.Attribute;
import com.adobe.granite.auth.saml.spi.Message;
import com.adobe.granite.auth.saml.spi.SamlHook;
import org.apache.jackrabbit.api.JackrabbitSession;
import org.apache.jackrabbit.api.security.user.Authorizable;
import org.apache.jackrabbit.api.security.user.UserManager;
import org.apache.sling.auth.core.spi.AuthenticationInfo;
import org.apache.sling.jcr.api.SlingRepository;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.osgi.service.component.annotations.ReferenceCardinality;
import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.Designate;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.annotation.Nonnull;
import javax.jcr.RepositoryException;
import javax.jcr.Session;
import javax.jcr.ValueFactory;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@Component
@Designate(ocd = SampleImpl.Configuration.class, factory = true)
public class SampleImpl implements SamlHook {
    @ObjectClassDefinition(name = "Saml Sample Authentication Handler Hook Configuration")
    @interface Configuration {
        @AttributeDefinition(
                name = "idpIdentifier",
                description = "Identifier of SAML Idp. Match the idpIdentifier property's value configured in the SAML Authentication Handler OSGi factory configuration (com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>) this SAML hook will hook into"
        )
        String idpIdentifier();

    }

    private static final String SAMPLE_SERVICE_NAME = "sample-saml-service";
    private static final String CUSTOM_LOGIN_COUNT = "customLoginCount";

    private final Logger log = LoggerFactory.getLogger(getClass());

    private SlingRepository repository;

    @SuppressWarnings("UnusedDeclaration")
    @Reference(name = "repository", cardinality = ReferenceCardinality.MANDATORY)
    public void bindRepository(SlingRepository repository) {
        this.repository = repository;
    }

    /**
     * This method is called after the user sync process is completed.
     * At this point, the user has already been synchronized in OAK (created or updated).
     * Example: Track login count by adding custom attributes to the user in the repository
     *
     * @param request
     * @param response
     * @param assertion
     * @param authenticationInfo
     * @param samlResponse
     */
    @Override
    public void postSyncUserProcess(HttpServletRequest request, HttpServletResponse response, Assertion assertion,
                                    AuthenticationInfo authenticationInfo, String samlResponse) {
        log.info("Custom Audit Log: user {} successfully logged in", authenticationInfo.getUser());

        // This code executes AFTER the user has been synchronized in OAK
        // The user object already exists in the repository at this point
        Session serviceSession = null;
        try {
            // Get a service session - requires "sample-saml-service" to be configured as system user
            // Configure in: "Apache Sling Service User Mapper Service Amendment"
            serviceSession = repository.loginService(SAMPLE_SERVICE_NAME, null);

            // Get the UserManager to work with users and groups
            UserManager userManager = ((JackrabbitSession) serviceSession).getUserManager();

            // Get the authorizable (user) that just logged in
            Authorizable user = userManager.getAuthorizable(authenticationInfo.getUser());

            if (user != null && !user.isGroup()) {
                ValueFactory valueFactory = serviceSession.getValueFactory();

                // Increment login count
                long loginCount = 1;
                if (user.hasProperty(CUSTOM_LOGIN_COUNT)) {
                    loginCount = user.getProperty(CUSTOM_LOGIN_COUNT)[0].getLong() + 1;
                }
                user.setProperty(CUSTOM_LOGIN_COUNT, valueFactory.createValue(loginCount));
                log.debug("Set {} property to {} for user {}", CUSTOM_LOGIN_COUNT, loginCount, user.getID());

                // Save all changes to the repository
                if (serviceSession.hasPendingChanges()) {
                    serviceSession.save();
                    log.debug("Successfully saved custom attributes for user {}", user.getID());
                }
            } else {
                log.warn("User {} not found or is a group", authenticationInfo.getUser());
            }

        } catch (RepositoryException e) {
            log.error("Error adding custom attributes to user repository for user: {}",
                     authenticationInfo.getUser(), e);
        } finally {
            if (serviceSession != null) {
                serviceSession.logout();
            }
        }
    }

    /**
     * This method is called after the SAML response is validated but before the user sync process starts.
     * We can modify the assertion here to add custom attributes.
     *
     * @param request
     * @param assertion
     * @param samlResponse
     */
    @Override
    public void postSamlValidationProcess(@Nonnull HttpServletRequest request, @Nonnull Assertion assertion, @Nonnull Message samlResponse) {
        // Add the attribute "memberOf" with value "sample-group" to the assertion
        // In this example "memberOf" is a multi-valued attribute that contains the groups from the Saml Idp
        log.debug("Inside postSamlValidationProcess");
        Attribute groupsAttr = assertion.getAttributes().get("groups");
        if (groupsAttr != null) {
            groupsAttr.addAttributeValue("sample-group-from-hook");
        } else {
            groupsAttr = new Attribute();
            groupsAttr.setName("groups");
            groupsAttr.addAttributeValue("sample-group-from-hook");
            assertion.getAttributes().put("groups", groupsAttr);
        }
    }

}
```

### 配置SAML掛接

SAML掛接使用OSGi配置來指定它應應用到的IDP。 在項目中建立OSGi配置檔案，地址為：

`/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.mycompany.aem.saml.CustomSamlHook~okta.cfg.json`

```json
{
  "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/exk4z55r44Jz9C6am5d7]",
  "service.ranking": 100
}
```

`idpIdentifier`必須與在相應的SAML身份驗證處理程式OSGi工廠配置中配置的`idpIdentifier`值匹配(PID: `com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>.cfg.json`)。 此匹配至關重要：將只為具有相同`idpIdentifier`值的SAML驗證處理程式實例調用SAML掛接。 SAML身份驗證處理程式是工廠配置，這意味著您可以具有多個實例（例如，`com.adobe.granite.auth.saml.SamlAuthenticationHandler~okta.cfg.json`、`com.adobe.granite.auth.saml.SamlAuthenticationHandler~azure.cfg.json`），並且每個掛接都通過`idpIdentifier`綁定到特定處理程式。 `service.ranking`屬性在配置多個掛接時控制執行順序（首先執行較高的值）。

### 新增Maven相依性

將所需的SAML SPI相依性新增到AEM Maven核心專案的`pom.xml`。

**對於AEM as a Cloud Service專案**，請使用包含SAML介面的AEM SDK API相依性：

```xml
<dependency>
    <groupId>com.adobe.aem</groupId>
    <artifactId>aem-sdk-api</artifactId>
    <version>${aem.sdk.api}</version>
    <scope>provided</scope>
</dependency>
```

`aem-sdk-api`成品包含所有必要的Adobe Granite SAML介面，包括`com.adobe.granite.auth.saml.spi.SamlHook`。

### 設定服務使用者（選擇性）

如果SAML掛接需要修改AEMJCR儲存庫中的內容（如`postSyncUserProcess`示例所示），則必須配置[服務用戶](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/cloud-service/developing/advanced/service-users):

1. 在項目`/ui.config/src/main/content/jcr_root/apps/myproject/osgiconfig/config/org.apache.sling.serviceusermapping.impl.ServiceUserMapperImpl.amended~saml.cfg.json`中建立服務用戶映射：

```json
{
  "user.mapping": [
    "com.mycompany.aem.core:sample-saml-service=saml-hook-service"
  ]
}
```

1. 建立重新點擊指令碼以定義`/ui.config/src/main/content/jcr_root/apps/myproject/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~saml.cfg.json`處的服務用戶和權限：

```
create service user saml-hook-service with path system/saml

set ACL for saml-hook-service
    allow jcr:read,rep:write,rep:userManagement on /home/users
end
```

這將授予服務用戶讀取和修改儲存庫中用戶屬性的權限。

### 部署至AEM

將自定義SAML掛接作為雲服務部署到AEM:

1. 生成項AEM目
1. 將代碼提交到Cloud Manager Git儲存庫
1. 使用完整堆棧部署管道部署
1. 當用戶通過SAML驗證時，將自動激活SAML掛接


### 重要注意事項

+ **IDP標識符匹配**：在SAML掛接中配置的`idpIdentifier`必須與SAML身份驗證處理程式工廠配置中的`idpIdentifier`完全匹配(`com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>`)
+ **屬性名稱**：確保掛接中引用的屬性名稱（例如，`groupMembership`）與SAML身份驗證處理程式中配置的屬性匹配
+ **效能**：在每次SAML驗證期間執行掛接實現時，保持輕量
+ **錯誤處理**：當發生嚴重錯誤導致身份驗證失敗時，SAML掛接實現應拋出`com.adobe.granite.auth.saml.spi.SamlHookException`。 SAML身份驗證處理程式將捕獲這些異常並返回`AuthenticationInfo.FAIL_AUTH`。 對於儲存庫操作，始終適當捕獲`RepositoryException`和日誌錯誤。 使用try-catch-finally塊確保正確清理資源
+ **正在測試**：在部署到生產環境之前，請先在較低的環境中徹底測試自訂鉤點
+ **多個掛接**：可以設定多個SAML掛接實作；將執行所有相符的掛接。 使用OSGi元件中的`service.ranking`屬性來控制執行順序（較高的排名值會先執行）。 若要在多個SAML驗證處理常式工廠設定(`com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>`)中重複使用SAML掛接，請建立多個掛接設定（OSGi工廠設定），每個設定具有符合個別SAML驗證處理常式的不同`idpIdentifier`
+ **安全性**：在業務邏輯中使用SAML斷言之前，先驗證和清理所有資料
+ **儲存庫訪問**：在`postSyncUserProcess`中修改用戶屬性時，始終使用具有適當權限的[服務用戶](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/cloud-service/developing/advanced/service-users)，而不是管理會話
+ **服務用戶權限**：向[服務用戶](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/cloud-service/developing/advanced/service-users)授予最低所需權限（例如，在`jcr:read`上僅`rep:write`和`/home/users`，未授予完全的管理權限）
+ **會話管理**：始終使用try-catch-finally塊確保儲存庫會話正確關閉，即使出現異常
+ **用戶同步計時**：在用戶已同步到OAK後執行`postSyncUserProcess`掛接，因此用戶對象在該位置保證存在於儲存庫中
