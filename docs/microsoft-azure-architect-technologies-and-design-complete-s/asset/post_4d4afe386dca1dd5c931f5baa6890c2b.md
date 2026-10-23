
The *application proxy* allows users to access an on-premise application remotely. A proxy, as you know, is a physical or virtual layer that exposes an endpoint, like a URL. That URL then, using the incoming data sent along with the request, can be analyzed so the request can be sent to a different endpoint. This helps from a security perspective because the actual address of the application is not known and also helps from a maintenance perspective because if the hidden endpoint changes, only the proxy needs to be updated, not all the remote clients accessing the endpoint.

Once you have installed the Microsoft Azure Active Directory Application Proxy Connector on a server in an on-premise data center, you need to log in to your Azure account to connect the server hosting the connector and the application proxy. Logging in using a Global Administrator account with the source of an Azure Active Directory endpoint causes an entry to appear on the Application Proxy blade. Then you configure an app, providing the requested information, as shown in Figure 2.18.

## FIGURE 2.18 Adding an application proxy

![](img/95043b3df926bcdadadb082b49262037_96_0.png)

Provide the configuration name, the internal URL to which the request will route, and an external endpoint. Based on the pre-authentication configuration, those attempting access to the external URL will be authenticated against AAD prior to getting access to the internal URL. As shown in Figure 2.19, after a request to the external URL, which is the application proxy service (which is configured similar to that shown previously in Figure 2.18), the credential is validated against your Azure Active Directory; once it’s validated, access to the internal URL is granted.

## FIGURE 2.19 Application proxy flow diagram

![](img/95043b3df926bcdadadb082b49262037_97_0.png)

There are also use cases for reducing latency between different data centers. It is good practice to place the content your employees need, and what your customers or partners want, as close to them as possible. This simply makes the whole experience better because connectivity happens faster. Simply install the Application Proxy Connector onto a server anywhere, as close to the people who need the information as possible. Then configure the external endpoint URL for that specific connector and deliver it to those individuals.

## Service Level Agreement

A service level agreement (SLA) typically describes how available a specific product is guaranteed to be during a certain time frame. Usually, when the SLA is breached, partial refund for the price of the product is provided upon request. The Free edition of Azure Active Directory does not have an SLA, as shown previously in Table 2.1. The Basic and Premium editions have the same SLA terms, as shown in Table 2.3.

## TABLE 2.3 Basic and Premium Azure Active Directory SLAs

| Monthly Uptime Percentage | Service Credit |
|---|---|
| < 99.9% | 25% |
| < 99% | 50% |
| < 95 % | 100% |

The monthly uptime percentage is calculated using the following algorithm:

(User Minutes – Downtime) / User Minutes × 100

where User Minutes is the sum of the amount of Downtime and the number of impacted users. SLAs are important, but that doesn’t mean Microsoft Support will turn their backs on you just because you are running in Free mode. At the same time, if you are running mission-critical or business workloads that have great impact, I recommend running in an edition that has a commitment to uptime. It simply makes good business sense, and any issue can start without needing to have that conversation in the first place.

## Identity Protection

From a security perspective, a company is only as strong as its weakest employee. Getting unauthorized access into a corporate network often uses an approach commonly referred to as *phishing*. The sophistication of these attempted accesses through this attack vector has improved to the point that even veteran IT professionals can sometimes become victims of them. Getting access to a corporate network only requires an unsuspecting victim to double-click an executable, open an attachment, or visit a link that installs malicious software. An interesting approach some bad actors take is that they get administrative access and don’t take any action right away. They let you forget that you made the mistake. But the consequences do not go away. The vulnerability might be employed right away, stay dormant until the attacker needs it, or simply lie in wait until the exploiter gets around to engaging and probing the network.

That reads kind of scary, but there are some ways to protect against this, for example with identity protection. Even if you forget about a detected threat and even if you don’t recognize any odd behaviors on your system, the actions taken on the exploited system expose certain patterns and in many cases act within a definable pattern of activities. These activities can include the following:

- Sign-ins from anonymous IPs (for example, accessing from Tor)
- Sign-ins attempted from multiple locations within a time frame that is suspicious due to impossible travel times
- Sign-ins from unfamiliar locations
- Sign-ins from infected devices

The previous points read like common sense, but the massive amount of authentication-related data being sent back and forth can make attacks difficult to catch. This is what Identity Manager provides; it provides features to monitor the behaviors of users who are authenticated but appear to have been compromised.

## Add Azure AD Identity Protection

In Exercise 2.3, you will add Azure AD Identity Protection to protect an enterprise from exploited accounts and identity spoofing. Identity Protection also provides visibility into existing threats and vulnerabilities.

## EXERCISE 2.3

## Adding Identity Protection

- 1. Log in to the Azure Portal at `portal.azure.com`.
- 2. Click the Marketplace link, search for *Identity Protection*, and click the Create button, as shown in Figure 2.20.

**FIGURE 2.20** Create Azure AD Identity Protection

![](img/95043b3df926bcdadadb082b49262037_99_0.png)

- 3. Select the directory into which to install Identity Protection, for example The C# Guitar Network, as shown in Figure 2.21.

**FIGURE 2.21** Selecting a directory for Azure AD Identity Protection

![](img/95043b3df926bcdadadb082b49262037_99_1.png)

- 4. Click the Create button and open the Azure AD Identity Protection blade by clicking the link, as shown in Figure 2.22.

## FIGURE 2.22 Azure AD Identity Protection

![](img/95043b3df926bcdadadb082b49262037_100_0.png)

Installing this Marketplace feature is required to utilize some of the conditional access policies discussed in the next section. Identity Protection provides the reporting capabilities as well as the interface for creating the remediation policy. For example, consider that I want to create a remediation policy after a sign-in attempt has been flagged as High. The controlled remediation can be to enforce multifactor authentication. Continue reading about conditional access; then perform Exercise 2.4 to create a policy.

## Conditional Access

Security, security, security—it can’t be said enough to a point where it equals the importance of the topic. There have been reports just in the past year that the number of invasive intrusion attempts have increased by almost 300%. So, there are a lot of bad actors attempting to find vulnerabilities in IT solutions and then exploit them. Ransomware, for example, is a lucrative undertaking. It doesn’t specifically focus on private data or corporate intellectual property (IP) like many security administrators might be focusing on. Rather, ransomware attempts to prevent the access to the data and the Internet Protocol (IP) itself. The point is that bad actors are very inventive about discovering new ways to exploit IT vulnerabilities. Therefore, security consultants and those who want to prevent and protect must come up with and implement new ways to protect their assets.

![](img/95043b3df926bcdadadb082b49262037_100_1.png)

Be careful! You can implement so much security that it inhibits employee productivity and access to your products and services by customers. Remember, bring-your-own-device (BYOD) policies, remote workers, and the different types of devices accessing your resources all make the work more efficient. However, consumption of IT resources is much more complex. Each of those scenarios needs specific attention and a specific approach toward the implementation of a security solution.
