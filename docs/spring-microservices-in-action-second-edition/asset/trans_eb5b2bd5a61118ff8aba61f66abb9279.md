
```java
@Service
public class LicenseService {

    public License getLicense(String licenseId, String organizationId){
        License license = new License();
        license.setId(new Random().nextInt(1000));
        license.setLicenseId(licenseId);
        license.setOrganizationId(organizationId);
        license.setDescription("Software product");
        license.setProductName("Ostock");
        license.setLicenseType("full");

        return license;
    }

    public String createLicense(License license, String organizationId){
        String responseMessage = null;
        if(license != null) {
            license.setOrganizationId(organizationId);
            responseMessage = String.format("This is the post and the object is: %s", license.toString());
        }

        return responseMessage;
    }

    public String updateLicense(License license, String organizationId){
        String responseMessage = null;
        if (license != null) {
            license.setOrganizationId(organizationId);
            responseMessage = String.format("This is the put and the object is: %s", license.toString());
        }

        return responseMessage;
    }

    public String deleteLicense(String licenseId, String organizationId){
        String responseMessage = null;
        responseMessage = String.format("Deleting license with id %s for the organization %s", licenseId, organizationId);
        return responseMessage;
    }
}
```

这个服务类包含一组示例服务，返回硬编码的数据，让你了解微服务的骨架应该是什么样子。继续阅读时，你将继续完善这个服务，我们也会深入探讨如何构建它。现在，让我们先为控制器添加第一个方法。该方法实现 REST 调用中使用的 GET 动词，并返回单个 License 类实例，如下一个代码清单所示。

在方法的参数上使用语法。对于清单 3.4 中的 GET 服务，我们将 URL 中的两个参数（`organizationId` 和 `licenseId`）映射到方法中的两个参数级变量，如下所示：

```
@PathVariable("organizationId") String organizationId
@PathVariable("licenseId") String licenseId
```

最后，让我们检查一下 `ResponseEntity` 返回对象。`ResponseEntity` 表示整个 HTTP 响应，包括状态码、响应头和响应体。在前面的清单中，它允许我们将 `License` 对象作为响应体返回，并将 200（OK）状态码作为服务的 HTTP 响应。

现在你已了解如何使用 HTTP GET 动词创建端点，接下来我们继续添加 POST、PUT 和 DELETE 方法，以创建、更新和删除 `License` 类实例。下面的清单展示了如何实现。

## 清单 3.5 暴露单个 HTTP 端点

```java
package com.optimagrowth.license.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

import com.optimagrowth.license.model.License;
import com.optimagrowth.license.service.LicenseService;

@RestController
@RequestMapping(value="v1/organization/{organizationId}/license")
public class LicenseController {

    @Autowired
    private LicenseService licenseService;

    @RequestMapping(value="/{licenseId}",method = RequestMethod.GET)
    public ResponseEntity<License> getLicense(
        @PathVariable("organizationId") String organizationId,
        @PathVariable("licenseId") String licenseId) {

        License license = licenseService.getLicense(licenseId, organizationId);
        return ResponseEntity.ok(license);
    }

    @PutMapping
    public ResponseEntity<String> updateLicense(
        @PathVariable("organizationId")
        String organizationId,
        @RequestBody License request) {
        return ResponseEntity.ok(licenseService.updateLicense(request, organizationId));
    }

    @PostMapping
    public ResponseEntity<String> createLicense(
        @PathVariable("organizationId") String organizationId,
        @RequestBody License request) {
        return ResponseEntity.ok(licenseService.createLicense(request, organizationId));
    }

    @DeleteMapping(value="/{licenseId}")
    public ResponseEntity<String> deleteLicense(
        @PathVariable("organizationId") String organizationId,
        @PathVariable("licenseId") String licenseId) {
        return ResponseEntity.ok(licenseService.deleteLicense(licenseId, organizationId));
    }
}
```

- PUT 方法用于更新许可证
- 将 HTTP 请求体映射到 License 对象
- POST 方法用于插入许可证
- DELETE 方法用于删除许可证

在清单 3.5 中，我们首先使用方法级的 `@PutMapping` 注解来标注 `updateLicense()` 方法。这个注解是 `@RequestMapping(method = RequestMethod.PUT)` 的快捷方式，而我们还没有使用过后者。

接下来需要注意，我们在 `updateLicense()` 方法的参数中使用了 `@PathVariable` 和 `@RequestBody` 注解。`@RequestBody` 将 HTTP 请求体映射到一个传输对象（在本例中是 `License` 对象）。在 `updateLicense()` 方法中，我们将来自两个参数（一个来自 URL，另一个来自 HTTP 请求体）的值映射到 `createLicense` 方法中的两个参数级变量：

```
@PathVariable("organizationId") String organizationId
@RequestBody License request
```

最后，在清单 3.5 中注意我们使用了 `@PostMapping` 和 `@DeleteMapping` 注解。`@PostMapping` 注解是方法级注解，是以下写法的快捷方式：

```
@RequestMapping(method = RequestMethod.POST)
```

`@DeleteMapping(value="/{licenseId}")` 也是方法级注解，是以下写法的快捷方式：

```
@RequestMapping(value="/{licenseId}",method = RequestMethod.DELETE)
```

## 端点命名很重要

在深入编写微服务之前，请确保你（以及组织中可能涉及的其他团队）为要通过服务暴露的端点建立标准。微服务的 URL（统一资源定位符）应清晰地传达服务的意图、服务管理的资源，以及服务内所管理资源之间的关系。我们发现以下指南对于命名服务端点非常有用：

- *使用清晰的 URL 名称*，明确服务所代表的资源。为定义 URL 建立规范格式，有助于让 API 更直观、更易用，并保持命名约定的一致性。
- *使用 URL 建立资源之间的关系。* 很多时候，微服务中的资源之间存在父子关系，子资源在父资源上下文之外是不存在的。因此，你可能不必为子资源单独创建微服务。使用 URL 来表达这些关系。如果你发现 URL 趋于冗长和嵌套，那可能意味着微服务承担的职责过多。
- *尽早为 URL 建立版本化方案。* URL 及其对应端点代表了服务提供方与服务消费方之间的契约。一种常见模式是为所有端点加上版本号前缀。尽早确定版本化方案并坚持执行。当已有多个消费者在使用 URL 时，再想为 URL 添加版本化（例如在 URL 映射中使用 `/v1/`）将极其困难。

此时，你已经有了几个服务。在命令行窗口中，进入项目根目录（其中可以找到 `pom.xml`），然后执行以下 Maven 命令。（图 3.4 展示了该命令的预期输出。）

```
mvn spring-boot:run
```

![](img/3c6dc613426280ca3eafe43dea24391e_101_0.png)

许可证服务器在端口 8080 上启动

图 3.4  显示许可服务成功启动的输出

