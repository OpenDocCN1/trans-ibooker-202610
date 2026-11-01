
- Service Continuity Management: This ensures the required services can be resumed in the case of a failure or disaster. It includes tasks to perform risk assessment and risk management to proactively avoid the risks or disasters from occurring (see Figure 2-7). It aims at reducing risks by developing a recovery plan to restore business activities if they are interrupted.

![](img/f561fe6de202c361a03741b184bee8ea_32_0.png)

Figure 2-7. A risk framework

- Information Security Management: This aligns IT security with business compliance. It enforces security in all aspects in all services and service management activities. It also deals with risk analysis and risk management.
- Design Coordination: This is a single point of coordination among all activities and processes. It also ensures the objectives of the Service Design stage are fulfilled and handed over to the Service Transition stage.

## Service Transition

Service Transition explains how to manage the transition of a new or modified service (see Figure 2-8). It comprises key processes such as the following

- *Transition Planning and Support*: This involves planning and coordination for the services across all relevant stakeholders like suppliers, service teams, etc. It also ensures the requirements from Service Strategy are smoothly transitioned into developed services from the design stage to Service Operation.
- *Service Asset and Configuration Management*: All the components of services and infrastructure have to be managed. Its configuration details such as current and planned state, historical information, etc., need to be maintained. The configuration management database (CMDB) is widely used for maintaining configuration details for infrastructure. It also supports service management processes by providing configuration information for the assets.

![](img/f561fe6de202c361a03741b184bee8ea_33_0.png)

Figure 2-8. Service Transition

- *Change Management*: The objective of the Change Management process is to ensure that changes are recorded or entered in the respective ALM (Application Lifecycle Management) tool and then evaluated; discussed in the change review board or meeting; and then prioritized, planned, and implemented on the planned date and time (see Figure 2-9). Change Management needs standardized methods and procedures to be used for handling the changes effectively and efficiently. All the changes to services and configuration items are updated in the configuration management system.

![](img/f561fe6de202c361a03741b184bee8ea_34_0.png)

Figure 2-9. Change Management process

- *Release and Deployment Management*: The objective is to build, install, test, and deploy applications or services in the target environments. This process ensures that the new or updated service or configurations are delivering the correct requirements. There are various automated tools that can be used to automate the Release and Deployment Management activities.

- *Knowledge Management*: The objective is to harness and harvest knowledge for reuse rather than re-inventing and re-discovering knowledge. This is beneficial when the service knowledge management system is created, maintained, and regularly updated.

## Service Operation

Service Operation is about coordinating and performing activities that are required for the smooth running of services (see Figure 2-10). It ensures that the required services are delivered as per the agreed upon service levels. It includes key processes such as the following:

- *Event Management*: The objective is to detect events at all levels and take appropriate actions to handle the events. The automated tools monitor the events and generate alerts based on the set process for them. It provides a basis for proactive monitoring and service improvement. This has now evolved into a new domain called Observability, and the emphasis is on end-to-end monitoring using logs, metrics, events, and traces. This includes advanced capabilities to provide an end-to-end view and provide correlation across infrastructure and applications to be able to find out the root cause of issues.

- *Incident Management*: The objective is to restore service operations to normal as quickly as possible to avoid an impact on the business. It focuses on delivering quality services and higher availability with minimal downtime. The incidents can be failures that are detected by Event Management or reported by users.

![](img/f561fe6de202c361a03741b184bee8ea_36_0.png)

Figure 2-10. Incident Management process

- **Problem Management:** The objective is to prevent incidents by finding their root cause of occurrence (see Figure 2-11). Eliminate the incidents that occur periodically and focus on minimizing the impact for incidents that cannot be prevented.

![](img/f561fe6de202c361a03741b184bee8ea_37_0.png)

Figure 2-11. Problem Management lifecycle

- *Request Fulfillment*: The objective is to fulfill the service requests raised by users. Service requests fall into different categories such as informational ones on status of a service, access requests, complaints, etc.
- *Access Management*: The objective is to provide authorized users with the required access rights to use a service. Access Management follows the policies that are listed for Security and Availability Management.

## Continual Service Improvement

This stage focuses on how to re-align IT services to match business changes. It matures the IT services by implementing identified improvement areas. It follows the famous Deming cycle, which has four stages: Plan, Do, Check, and Act. It also follows the seven steps in the Service Improvement process (see Figure 2-12). See Figure 2-13 for a tip.

![](img/f561fe6de202c361a03741b184bee8ea_38_0.png)

Figure 2-12. Seven-step improvement process

## DID YOU KNOW?

![](img/f561fe6de202c361a03741b184bee8ea_38_1.png)

## When was ITIL developed?

ITIL – IT Infrastructure Library was developed at the end of 1980s by the Central Computing & Telecommunications Agency (CCTA) – a government agency in Britain. This was designed to standardize and bring in quality in providing IT services.

Figure 2-13. Did you know?

## Drawbacks with Traditional InfraOps Teams

The InfraOps team can continue with the way it has been operating all these years, but times have changed, and there is an urge to move to a new operating model that is more aligned with newer technologies such as cloud, containerization, and infrastructure as code. Let’s take a look at some of the drawbacks of the traditional infrastructure operations model:

- *Structured but rigid process*: There’s no room for flexibility, and repetitive tasks are performed manually. For years, the InfraOps team has trusted manual ways of working and continued with this strategy.
- *Limited collaboration with other teams*: Requests are submitted through tickets and emails; there are dependencies; and delays are expected.
- *Siloed specialization*: This approach has I-shaped expertise, leading to an increase in hand-offs and affecting customer expectations.
- *Highly customized on-prem environment*: Manual efforts are needed to set up new infrastructure or make changes to existing infrastructure. Manual checklists are followed to track the changes. Both integration and architecture are complex.
- *Ticket-based communication*: There are multiple approvals and hand-offs needed when implementing a change, thereby increasing wait times.

## Need for Change

The traditional approach was good until the industry took a shift toward digital. Organizations realized that they could succeed only if they knew their customers well and were able to respond and provide relevant, needed services. This digital age needs new thinking, strategies, and processes, as well as investments in people and new tools and technologies that will help them to optimize. For example, if a tester is empowered to set up a test environment the same day using self-service instead of waiting for two weeks, wouldn’t this help in completing tasks ahead of time?
