
This is where the team has to get creative. There are several ways an attacker can gain access to this information so it’s good to start with higher level themes. Here are a few cases they think about:

- Someone could accidentally, or intentionally, move the data to a developer environment for testing with live data.
- The data could be stolen directly in the database by an attacker who manages to deploy malicious code within the network to gain access to the database.
- The application could be susceptible to attacks that leak data like a SQL Injection, Cross-Site Scripting, or Cross-Frame Scripting.
- The bank account information could get logged to a logging system in plain text.

For the purposes of this exercise, the team decides to first focus on the ability of the attacker to take advantage of a weakness in the application by leveraging Cross-Frame Scripting (XFS). The other attack opportunities can be reviewed in sequence after the first one.

| What | Who | Why | How | Impact | Counter Measure |
|---|---|---|---|---|---|
| Bank Account Numbers | Cyber-criminals | Financial gains with the information. | Cross-Frame Scripting (XFS) |  |  |

## 3.1.5 What to do with the found threats

Dashing Danielle turns to the product owner to understand what the impact would be of a breach of this information with regards to any contractual stipulations that might require the company to pay a fee to clients. The product owner acknowledges that there are clear financial impacts directly linked with data loss, and also raises the concern of brand damage that might be difficult to overcome since there are other vendors and solutions in the market that are direct competitors with Superior Products application. Dashing Danielle also raises the likely support and recover cost associated with the attack as well as the potential for having to purchase credit monitoring for the impacted accounts.

| What | Who | Why | How | Impact | Counter Measure |
|---|---|---|---|---|---|
| Bank Account Numbers | Cyber-criminals | Financial gains with the information. | Cross-Frame Scripting (XFS) | Financial payments to clients, credit monitoring, support and recover costs, brand damage |  |

Since the team is not familiar with the specifics of an XFS attack, Dashing Danielle is able to describe it to the team. In this case, an attacker will use malicious JavaScript in an iframe

that loads a page with the intent of stealing data. There are several mitigation techniques that can be used to protect against XFS. Considering that it is similar to a clickjacking attack, Dashing Danielle suggests the following mitigations:

- Preventing the browser from loading the page in frame using the X-Frame-Options or Content Security Policy (frame-ancestors) HTTP headers.
- Preventing session cookies from being included when the page is loaded in a frame using the SameSite cookie attribute.
- Implementing JavaScript code in the page to attempt to prevent it being loaded in a frame (known as a "frame-buster").

Dashing Danielle puts the mitigations on the board under the "Counter Measures". The team discusses these mitigation techniques and review the current coding and architecture in place.

| What | Who | Why | How | Impact | Counter Measure |
|---|---|---|---|---|---|
| Bank Account Numbers | Cyber-criminals | Financial gains with the information. | Cross-Frame Scripting (XFS) | Financial payments to clients, credit monitoring, support and recover costs, brand damage | X-Frame-Options, SameSite cookie attribute, Frame-busting |

After reviewing the architecture and code, it was recognized that the application already sets its session cookies with the SameSite attribute set to strict:

```
Set-Cookie: CookieName=CookieValue; SameSite=Strict;
```

However, the other mitigations are not in place and require a resolution. The product owner asks whether the proposed additionally countermeasures will be sufficient to resolve the open issue and whether the likelihood of an attack is high enough to warrant the additional effort. Dashing Danielle is able to produce research that shows automated tools that are used to attack their competitors. The product owner agrees to proceed, and Dashing Danielle describes and documents the steps that are needed to set the X-Frame-Options to "Deny" as well as the Content Security Policy setting frame-ancestors 'none'. Dashing Danielle also works with the development team to create proof of concept code that can be implemented in the code to deny the framing of the feature into another site.

The team is satisfied with the results from the threat model for this issue related to the stealing of bank account numbers. However, they don’t stop here and instead move on to the next possible threat. Although the manual method of threat modeling is time consuming, you can see that it can be pretty thorough, especially when compared to the method using a tool which we will talk about next.

## 3.1.6 Threat modeling using a tool

Just like with the manual method of threat modeling, there are several options when it comes to tools that can be used to develop a threat model. Each of these tools has their own benefits and drawbacks. Some are free, some are commercial tools, some can even be as simple as using a graphical tool to just draw the architecture and annotate the potential threats. One of the biggest benefits of using a threat modeling tool that is purpose built to define threats is that they will identify the threats for you making these tools more efficient at identifying issues. The results should still be reviewed with the appropriate stakeholders to ensure that the findings are indeed valid.

**THREAT MODELING TOOLS** Take some time to review the available tools that are out there for threat modeling. There are several commercial ones Like SecuriCad, ThreatModeler, and Irius Risk. However, this corner of application security has fewer tools and less mature ones than in other spaces. Two threat modeling tools that are freely available are the Microsoft Threat Modeling Tool and Threat Dragon by OWASP.

Both Microsoft Threat Modeling Tool (https://docs.microsoft.com/en-us/azure/security/develop/threat-modeling-tool) and Threat Dragon by OWASP (https://owasp.org/www-project-threat-dragon/) offer the ability to drag-and-drop items to a board in order to build the architecture out and show the dataflow. From there, each will generate a list of potential threats to the architecture. A simple diagram from OWASP Threat Dragon is shown below.

### OWASP Threat Dragon – Simple Diagram

![](img/cc84747f93d290964598894b4f5c153b_74_0.png)

Figure 3.3 – Simple diagram from OWASP Threat Dragon

Threat Dragon can place threats in the STRIDE model, and with this simple diagram there are two simple threats identified automatically by the tool. One is related to spoofing and the

other repudiation. Both are on the interaction between the actor and the process. What this means is that there is potential for the actor to impersonate another user and potentially access components of the system that they would not typically be allowed to do. To fix this, the application must put in place a means of authenticating the user and knowing that it is indeed the correct user.

I won’t go through the steps of the threat model using one of these tools since the effort is similar to what I described in the previous section using the manual method. With a tool in hand, the organization can scale the threat modeling process and centralize the review and storage of the threat model. This also allows the organization to threat model reusable components once. For instance, many of the applications in an organization may use the same authentication architecture. In this case the organization can threat model the authentication once and reuse that threat model for each application.

Using the example of Superior products, Dashing Danielle has reviewed several commercial tools for threat modeling, and has decided that the best tool for the job, and budget, is OWASP’s Threat Dragon. She has created a process diagram and documentation that walks the developer through the use of Threat Dragon. She created a repository where all threat models from Threat Dragon will be stored.

### Side notes about using a threat model tool

This approach to decentralizing and democratizing threat modeling with a tool allows for most technical resources in the organization to create a threat model using something like Threat Dragon. If an application security resource creates it, they will review their model with the engineering team. If a resource from the engineering team creates their own threat model, then it will be assigned to the application security team for review in the repository.

Dashing Danielle begins to gather the information on the common architecture that is used in Superior Products so that she can threat model those architectures and determine the open threats in order to get them remediated. Once she completes the threat modeling of the common architectures, she works with some of the resources in the engineering organization to evaluate her findings to make sure she was not missing anything. She also discusses the remediation options for the open threats that are found. Taking the feature that was used in the manual threat modeling session, she works with the appropriate engineering team to define the architecture in the tool. She generates the threats and compares that with what she found in the manual effort.

Chances are, in the story above, that Dashing Danielle will find discrepancies between what Threat Dragon found and what the team found in the manual process. This is to be expected as they are very different processes. Furthermore, the findings from the manual process will tend to be more specific and tailored to the architecture. The findings in Threat Dragon will be more generic. The use of both methods may need to be used to first identify the broad picture using Threat Dragon, and then diving into the details with a manual session using the output from Threat Dragon as an input into the manual effort.

Threat modeling is an early tool that can be used in the secure SDLC, but I’ll cover how to identify security issues while coding next.
