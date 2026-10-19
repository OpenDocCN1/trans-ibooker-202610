
API gateways serve as key components in serverless applications, handling requests from external sources and routing them to internal services. Cloud providers offer fully managed API gateway services that make it easy to set up, configure, and manage these resources for serverless web applications and APIs. While you might think that using a fully managed API gateway eliminates all security risks, failing to configure and secure it properly can leave your serverless application open to attacks. When setting up API gateways for serverless applications, developers may sometimes forget to enforce strict access controls and permissions. As a result, they may end up leaving APIs misconfigured and allow attackers to access sensitive data or perform unauthorized actions. In some cases, they might incorrectly assume that the default configuration is sufficient and leave it unchanged.

Let’s say you leveraged the `functionless integration pattern` (see [“Functionless Integration Pattern”](#ch02.html#ch02-functionless-integration-pattern)) and configured an API gateway resource to map incoming read requests directly to a database. Assuming the API is meant to support only read operations, an attacker may be able to exploit a misconfiguration and modify or delete records in the database. You might incorrectly assume that it’s impossible to guess your API’s endpoint URL as well as the API’s subpaths or routes. However, attackers often use various tools and techniques to discover, explore, and attack these endpoints.

In addition to improperly configured access controls, other API gateway security misconfigurations can put your serverless applications at risk. Here is a non-exhaustive list of misconfigurations and issues to be aware of:

### No authentication and authorization mechanisms

Managed API gateway services often support fine-grained access controls as well as integration with IdPs out of the box, but they must be explicitly configured by the engineer setting up the cloud resource. Leaving API endpoints exposed without authentication and authorization mechanisms would allow anyone, especially attackers, to call the API.

### Exposed sensitive data in query parameters or headers

Passing sensitive information such as API keys, access tokens, or personal user data through query parameters or HTTP headers without encryption can lead to unintended exposure that attackers may take advantage of to gain unauthorized access, impersonate users, or escalate privileges within the system.

### Unrestricted CORS configuration

Overly permissive cross-origin resource sharing (CORS) settings allow any application to interact with your API. For example, using `Access-Control-Allow-Origin: *` may enable malicious websites to make unauthorized requests on behalf of authenticated users.

### No logging or insufficient logging

Insufficient or misconfigured request and error logging at the API gateway level limits your ability to audit, investigate, or respond to attacks effectively.

### Lack of rate limiting and throttling

Managed API gateway services typically provide built-in support for rate limiting and throttling to protect backend resources and manage costs. Not configuring rate limiting and throttling exposes API endpoints to brute-force, DoS, and DoW attacks.

### Exposed private APIs

Misconfiguring the network setup could leave a private API gateway resource inadvertently exposed and publicly accessible.

### Publicly exposed debug information

Leaving debugging or verbose error messages enabled in production APIs can unintentionally reveal stack traces and implementation details that attackers can use to exploit your application. Managed API gateway resources typically allow engineers to configure custom error responses, ensuring that sensitive internal details like stack traces and service implementation details are not exposed to unauthorized users.

#### Note

You may also want to check the [OWASP Top 10 API Security Risks (2023)](https://oreil.ly/DHxIK) to gain a deeper understanding of common API vulnerabilities. While the security risks and issues discussed generally apply to various types of APIs, they should be considered when building and securing serverless APIs as well.

Improperly configured API gateway resources can leave the serverless application exposed to various security risks. Having a good idea of what common misconfigurations look like and how they can be exploited is key to preventing unauthorized access and securing your serverless application.

# Supply Chain Attacks

Developers often rely on third-party libraries, SDKs, and prebuilt IaC templates to accelerate the development of serverless applications. While these help you build and deploy applications faster, using them without checking for tampering or malicious updates can expose your application to supply chain attacks. Instead of directly attacking your application through an API gateway, an attacker can publish a malicious package or inject malicious code in an existing package that your application is using. While you might assume that using well-known or vendor-managed libraries reduces the risk, the reality is that even widely used packages can be compromised through a variety of means.

In 2022, the `ctx` Python library was updated with malicious code designed to steal environment variables that potentially included credentials and other sensitive configuration values. The library, which allowed developers to use dot notation when working with dictionaries, was modified to send the environment variables to a remote server controlled by the attacker ([Figure 3-16](#ch03.html#ch03-supply-chain-attacks_fig)).

![](assets/lsse_0316.png)

If an application using this library was deployed in a serverless function, the attacker could gain access to cloud platform credentials and other secrets stored in the environment variables. These credentials would then allow the attacker to make unauthorized API calls, escalate privileges, and gain unauthorized access to other services or resources. While all malicious versions were immediately removed from the Python Package Index (PyPI), those who installed, updated, and used the package from May 14, 2022, to May 24, 2022, were advised to perform an audit of all potentially affected credentials and rotate these as needed.

#### Note

If you’re wondering how a malicious version of the package ended up on PyPI, [a domain takeover allowed the attacker to compromise the original maintainer’s user account](https://oreil.ly/E9vlA), which was then used to publish a malicious version of the `ctx` package. The attacker reregistered the expired domain associated with the original maintainer’s email address, re-created the email account used on PyPI, and initiated a password reset to gain access to the original maintainer’s PyPI account. From there, the attacker uploaded malicious versions of the `ctx` package, which when used would send captured environment variables to a server the attacker controlled.

In 2023, previous versions of the `bignum` Node Package Manager (NPM) package were reported to download a malicious binary file that exfiltrated environment variables and credentials, and sent them to the attacker. The `bignum` package used `node-pre-gyp` to download a binary file from a hardcoded cloud storage bucket during installation. When this cloud storage bucket was deleted, the attacker created a new bucket with the same name and uploaded a malicious version of the binary file downloaded by the package. Users installing the affected `bignum` package versions downloaded the malicious binary file instead of the legitimate one ([Figure 3-17](#ch03.html#ch03-cloud-subdomain-takeover)).

![](assets/lsse_0317.png)
