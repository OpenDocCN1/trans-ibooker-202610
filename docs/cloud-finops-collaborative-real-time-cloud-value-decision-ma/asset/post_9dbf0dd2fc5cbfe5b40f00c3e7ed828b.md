
benefits and value, and it will have a team of people whose main focus is maintaining and improving the tool—so your teams don’t have to.

## Other Considerations

Cost isn’t, and shouldn’t be, the whole story when it comes to buy versus build. There are security implications. Also, you might find that the features a tool offers aren’t compatible with your environment or can’t be integrated with something you already use like chat and ticketing applications for alerts and notifications. Some industries even have specific compliance requirements like FedRAMP, HIPAA, SOC 2, or PCI, and the third-party tooling providers may not currently hold the right certifications. Finding a vendor with the right set of certifications can make your automation process easier from a compliance perspective.

Being able to audit automated actions is key, especially when the automation interacts with production environments. This knowledge builds transparency.

Third-party automation tools are usually well documented and provide training materials for staff to learn from, so when you’re hiring new staff it’s more likely that they are already familiar with the tooling. If you build automation tooling yourself, you need to be willing and able to provide ongoing training for existing and new team members, and you run the risk that those responsible for building and maintaining the tooling may leave the organization. Unless there are more than a few employees familiar with the tooling and its implementation, you encounter “key-person risk.”

When you decide to build automation tools instead of buying off-the-shelf offerings, you should do so in an informed way. You must always compare the effort to build all the required features—including ways to measure the effectiveness of your tooling, ongoing maintenance, and updates you will need to implement—with a solution you purchase. And you should be careful to identify the missing features or compliance requirements of any third-party vendor tooling.

## Tooling Deployment Options

As with most software, there are a few considerations around how automation tools are deployed that will have an impact on the selection process.

### Cloud native

With automation that runs within a cloud account, scripts can be provided by the cloud service provider or the broader community. These are executed inside your cloud account in Function-as-a-Service platforms (like AWS Lambda).

### Self-built

This is software that your own staff members build and run. Feature development is run by internal teams. If you have many different DevOps teams around your organization, you might find they each solve the same problem using their own version of automation. But it's best if a centralized team runs an automation tool that services your whole cloud environment. Centralized management of a tool will reduce duplicated effort around the organization, but it also means you need a one-size-fits-all solution.

### Third-party self-hosted

When it comes to prebuilt automation solutions, there are free and open source tools available, as well as paid software licensing options. With this deployment model, teams use third-party software but run it within their own environment, maintaining and managing the service themselves. In the open source space, a commonly recommended tool is the Cloud Custodian application created by the team at Capital One. You'll want to consider the cost of operating and potentially customizing third-party self-hosted solutions.

### Third-party SaaS

This option removes the need to run and manage software altogether. A third-party hosted software solution gives you the easiest setup and potentially the fastest time to automation. A vendor will often provide simple tools that will handle the setup required in your cloud account for the vendor's platform to integrate with your environment. Complex metrics and configurations have already been developed and often come attached to easy-to-use dashboards and setup screens. You will often pay a monthly fee, which is easy to measure against the benefits of the tool itself. This model does come with a security impact, which we'll cover in a moment.

![](img/4d4736bc293fd892d1da81802d0f9520_364_0.png)

It's not always simply build versus buy; some companies take a hybrid approach of buy and augment, where they combine an off-the-shelf tool's API data or recommendations with their own in-house automation so they can avoid building the data ingestion/management capabilities of FinOps but can own the code that actually touches their infrastructure.

Jason Fuller from HERE Technologies shared that while he uses a vendor FinOps platform to drive his reporting and recommendations, he relies on heavily customized internal automation code (based on an in-house fork of the open source tool Cloud Custodian) to actually effect changes in his environment. This hybrid implementation of build versus buy is a three-pronged (vendor + open source + custom code) approach that he has honed after 10 years of doing FinOps at four large-scale cloud spenders.

## Automation Working Together

Automation tools rarely operate in isolation. It's likely that multiple tools will be operating alongside each other, especially over time as your cloud footprint grows.

### Integration

Integration between tools can be very powerful, and it's something you should be looking for and taking advantage of. Avoid having many separate tools that do their own version of the same thing. For example, if you have an existing ticketing system at your organization, you would ideally implement FinOps tooling that will generate tickets for action items in that existing system instead of building in a separate task-tracking system.

If the automation tooling you have selected doesn't have direct integration with your other software tools, it's still possible that the automation tooling supports extensible event processing. Then you can receive notifications about events generated inside the automation tooling and can build an extension that will connect the event to some of your other software packages. It's a great way to extend functionality—for instance, sending notifications to your chat application that particular tasks have been done, generating tickets in your ticketing systems for necessary follow-up tasks, or implementing your own automated remediation tasks from the detections.

### Automation Conflict

When you deploy multiple automation tools, you need to make sure that they don't conflict with one another. If one tool is trying to start a server while another is trying to shut it down, you can end up with the automations fighting each other's actions. If you are deploying an automation tool for a particular feature, and that same tooling has another feature that you don't intend to use, consider whether you can remove the service's permissions for that disabled feature to perform changes on your cloud accounts.

We've seen companies deploy different automation tools for different features they offer, only to find that both tools are trying to control the same resources. Or one team will implement their own automation that conflicts with existing automation.

This is a harder problem to solve. To prevent it, you need to educate the whole organization about what automation is in place, and have teams work together to ensure they understand the possible impacts of any planned automation within their cloud accounts.

Ideally, automation should notify you when it's fighting with something external. For example, if automation stops a server instance more than a set number of times in a short period of time, it should send an alert and prevent any further changes.

Sometimes the conflict an automation service has isn’t with other tooling, but with people. You may have automation that stops a server instance while one of your team members requires access to that server. This can lead to the team member starting the instance, only to see the automation stop it. Once again, you should be sure that your team has been well educated, including on how to exclude a resource from the automation.

![](img/4d4736bc293fd892d1da81802d0f9520_366_0.png)

Joe Daly, formerly Director of Cloud Optimization at Cardinal Health and Nationwide, tells the story of how he accidentally took down a production environment by manually removing a nonproduction resource tied via automation to the production one. As stated earlier, education about automation being used is critical in the organization. To hear the full story, check out Episode 13 of the FinOpsPod podcast, titled “FinOops—Lessons Learned the Hard Way.”

## Safety and Security

Security is a primary concern with any FinOps automation tooling. Often, FinOps automation requires very high levels of access to your cloud environments, whether it’s just the ability to describe how everything in your account has been configured, or the ability to change (start/stop/delete) the resources themselves. Such access could lead directly to a denial of service attack, data loss, corruption, or a confidentiality breach.

Security is often cited as a major reason third-party tooling isn’t used inside organizations. It’s understandably difficult to get a security team to sign off on the use of a third-party tool that needs broad access to cloud accounts. However, just because your own teams write the software doesn’t necessarily mean it will be perfectly secure. You must ensure that all automation tooling you deploy has the least amount of permissions possible to perform the tasks required, while protecting the software from external threats.

It is also highly likely that a successful third-party vendor has spent a lot of time and effort on their security. Reviewing their documentation and any previous security announcements from them will help you gauge how seriously that vendor takes security as part of their offering.

If you look for inspiration from the FinOps maturity model, you might choose to initially integrate with a third-party SaaS tool in read-only mode. You might then take the notifications from that tool and automate the remediation actions using your own tools. When you have more confidence in the vendor, you can start allowing them to perform more actions on your behalf until you have enabled all the features you require.
