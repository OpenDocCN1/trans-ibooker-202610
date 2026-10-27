

**清单 9.5 仅限制经过身份验证的用户访问**

```java
package com.optimagrowth.organization.security;
import org.springframework.context.annotation.Configuration;
import org.springframework.http.HttpMethod;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;

@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(jsr250Enabled = true)
public class SecurityConfig extends KeycloakWebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http)
            throws Exception {
        super.configure(http);
        http.authorizeRequests()
            .anyRequest().authenticated();
    }
}
```

传递给方法的 HttpSecurity 对象配置了所有访问规则。

所有访问规则都在 `configure()` 方法内部定义。我们将使用 Spring 传入的 `HttpSecurity` 类来定义我们的规则。在此示例中，我们将限制对组织服务中任何 URL 的访问，仅允许经过身份验证的用户。

假设我们在 HTTP 头中没有提供访问令牌的情况下访问组织服务。在这种情况下，我们会收到 401 HTTP 响应代码，并显示一条消息，指出需要对服务进行完整身份验证。图 9.24 显示了在没有身份验证 HTTP 头的情况下调用组织服务的输出。

![](img/3c6dc613426280ca3eafe43dea24391e_291_0.png)

返回 HTTP 状态码 401。

**图 9.24 在没有访问令牌的情况下调用组织服务会导致调用失败。**

接下来，我们将使用访问令牌调用组织服务。 （要生成访问令牌，请参阅第 9.2.5 节。）我们希望从对 `/openid-connect/token` 端点的返回 JSON 调用中复制并粘贴 `access_token` 值，并在调用组织服务时使用它。记住，当我们调用组织服务时，我们需要将授权类型设置为 Bearer Token，并使用 `access_token` 值。图 9.25 显示了对组织服务的调用，但这次传递了访问令牌。

![](img/3c6dc613426280ca3eafe43dea24391e_292_0.png)

**图 9.25 在对组织服务的调用中传入访问令牌**

这可能是使用 JWT 保护端点最直接的用例之一。接下来，我们将在此示例的基础上进行扩展，并限制特定角色对特定端点的访问。

## 通过特定角色保护服务

在下一个示例中，我们将锁定组织服务上的 DELETE 调用，仅允许具有 ADMIN 访问权限的用户访问。正如你在第 9.2.4 节中记得的，我们创建了两个可以访问 O-stock 服务的用户账户：`illary.huaylupo` 和 `john.carnell`。`john.carnell` 账户分配了 USER 角色。`illary.huaylupo` 账户具有 USER 角色和 ADMIN 角色。我们可以通过在控制器中使用 `@RolesAllowed` 来允许特定角色执行某些方法。以下清单展示了如何操作。

**清单 9.6 在 OrganizationController.java 中使用 @RolesAllowed 注解**

```java
package com.optimagrowth.organization.security;
//Imports removed for conciseness

@RestController
@RequestMapping(value = "/v1/organization")
public class OrganizationController {

    @Autowired
    private OrganizationService service;

    @RolesAllowed({ "ADMIN", "USER" })
    @RequestMapping(value = "/{organizationId}", method = RequestMethod.GET)
    public ResponseEntity<Organization> getOrganization(
        @PathVariable("organizationId") String organizationId) {
        return ResponseEntity.ok(service.findById(organizationId));
    }

    @RolesAllowed({ "ADMIN", "USER" })
    @RequestMapping(value = "/{organizationId}", method = RequestMethod.PUT)
    public void updateOrganization(@PathVariable("organizationId")
        String id, @RequestBody Organization organization) {
        service.update(organization);
    }

    @RolesAllowed({ "ADMIN", "USER" })
    @PostMapping
    public ResponseEntity<Organization> saveOrganization(
        @RequestBody Organization organization) {
        return ResponseEntity.ok(service.create(organization));
    }

    @RolesAllowed("ADMIN")
    @DeleteMapping(value = "/{organizationId}")
    @ResponseStatus(HttpStatus.NO_CONTENT)
    public void deleteLicense(@PathVariable("organizationId")
        String organizationId) {
        service.delete(organizationId);
    }
}
```

- 表示只有具有 USER 和 ADMIN 角色的用户才能执行此操作
- 表示只有具有 ADMIN 角色的用户才能执行此操作

现在，为了获取 `john.carnell`（密码：`password1`）的令牌，我们需要再次执行 `/openid-connect/token` POST 请求。一旦我们获得了新的访问令牌，我们就需要调用组织服务的 DELETE 端点：`http://localhost:8072/organization/v1/organization/dfd13002-57c5-47ce-a4c2-a1fda2f51513`。我们将在调用中收到 403 Forbidden HTTP 状态码，并收到一条错误消息，指出对该服务的访问被拒绝。我们调用返回的 JSON 文本如下所示：

```json
{
  "timestamp": "2020-10-19T01:19:56.534+0000",
  "status": 403,
  "error": "Forbidden",
  "message": "Forbidden",
  "path": "/v1/organization/4d10ec24-141a-4980-be34-2ddb5e0458c7"
}
```

**注意：** 我们使用 8072 端口，因为这是我们在上一章中为 Spring Cloud Gateway 定义的端口（具体在 Spring Cloud Configuration 服务仓库中位于 `gateway-server.yml` 配置文件中）。

如果我们使用 `illary.huaylupo` 用户账户（密码：`password1`）及其访问令牌尝试相同的调用，我们会看到一次成功的调用，返回无内容和 HTTP 状态码 204，No Content。此时，我们已经了解了如何使用 Keycloak 调用和保护单个服务（组织服务）。然而，在微服务环境中，我们通常需要进行多次服务调用来完成一次事务。在这些情况下，我们需要确保访问令牌从一次服务调用传播到另一次服务调用。

## 9.4.4 传播访问令牌

为了演示在服务之间传播令牌，我们还将使用 Keycloak 保护我们的许可服务。记住，许可服务调用组织服务来查找信息。问题在于，我们如何将令牌从一个服务传播到另一个服务？

我们将设置一个简单的示例，在其中许可服务调用组织服务。如果你按照我们在前几章中构建的示例进行操作，你会看到两个服务都在网关后面运行。图 9.26 显示了经过身份验证用户的令牌如何通过网关、许可服务，然后传递到组织服务。

![](img/3c6dc613426280ca3eafe43dea24391e_294_0.png)

图 9.26 中发生的事情如下。图 9.26 中对应的数字在以下内容中用括号标出。

1. 用户已经使用 Keycloak 服务器进行身份验证，并对 O-stock Web 应用程序发出调用。用户的访问令牌存储在用户的会话中。O-stock Web 应用程序需要检索一些许可数据并调用许可服务 REST 端点（1）。作为对许可 REST 端点的调用的一部分，O-stock Web 应用程序通过 HTTP Authorization 头添加访问令牌。许可服务只能通过 Spring Cloud Gateway 访问。
2. 网关查找许可服务端点，然后将调用转发到许可服务的一个服务器（2）。服务网关复制传入调用中的授权 HTTP 头，并确保该 HTTP 头被转发到新的端点。
3. 许可服务接收传入调用。由于许可服务是一个受保护的资源，许可服务将使用 Keycloak 服务器验证令牌（3），然后检查用户的角色以获得适当的权限。作为其工作的一部分，许可服务调用组织服务。在执行此操作时，许可服务需要将用户的访问令牌传播到组织服务。
4. 当组织服务接收到调用时，它会取出 HTTP Authorization 头并使用 Keycloak 服务器验证令牌（4）。

要实现这些步骤，我们需要对代码进行多项更改。如果我们不这样做，在从许可服务检索组织信息时，我们将收到以下错误：

```text
"message": "401 : [{status:401,error: Unauthorized,message: Unauthorized,path: /v1/organization/d898a142-de44-466c-8c88-9ceb2c2429d3}]
```

第一步是修改网关，使其将访问令牌传播到许可服务。默认情况下，网关不会将诸如 `cookie`、`set-cookie` 和 `authorization` 之类的敏感 HTTP 头转发到下游服务。为了允许传播授权 HTTP 头，我们需要在 Spring Cloud Config 仓库中位于 `gateway-server.yml` 配置文件中的每个路由中添加以下过滤器：

```yaml
- RemoveRequestHeader=Cookie,Set-Cookie
```

此配置是网关将阻止传播到下游服务的敏感头黑名单。`RemoveRequestHeader` 列表中没有 Authorization 值意味着网关将允许该头通过。如果我们不设置此配置属性，网关会自动阻止所有三个值的传播（Cookie、Set-Cookie 和 Authorization）。

接下来，我们需要配置许可服务以包含 Keycloak 和 Spring Security 依赖项，并为服务设置我们想要的任何授权规则。最后，我们需要将 Keycloak 属性添加到配置服务器中的应用程序属性文件中。

### 配置许可服务

在传播访问令牌时，我们的第一步是向许可服务 `pom.xml` 文件添加 Maven 依赖项。以下清单显示了依赖项。

