
In any organization, the most important assets (aside from people) are most likely found in the IT and data inventory. Protection of these assets is incredibly important to security professionals as well as executive leadership, governing boards, and customers of these organizations. Because of the sensitivity and value of these assets, governments and industries across the globe have put legislations and regulations in place to protect them. Along with the loss of revenue or recovery costs if assets are lost or stolen, significant privacy concerns exist when sensitive assets are mismanaged. This chapter covered a great many of the important concepts and guiding principles a security practitioner is expected to know and implement in their daily work. Beginning with constructing an asset management policy, the organizational policy must be informed by prevailing law, directives, and best practices but be customized to each organization's mission and unique risk profile. The process for asset management will include multiple stakeholders within the organization, so roles and responsibilities must be clearly documented, and people should be trained adequately.

At the core of asset management are the standards and frameworks that have been developed by industry experts and cohorts of practitioners that should be used to build the asset management program in an organization. The choice of standards or frameworks and the individual security controls put in place to protect confidentiality, integrity, and availability of assets will also differ from one organization to the next. How a security practitioner will scope and tailor asset management controls depends on measuring and evaluating risk based on variables such as legal jurisdiction, industry, and considerations like compensating and alternative controls. Keep in mind, security controls work best when working together, not managed independently. The proper acquisition, inventorying, monitoring, and security management of assets in organizations around the world is a significant undertaking. Information systems are highly interconnected and dependent on each other, information is valuable and requires protection, and the impact of doing it wrong can be disastrous to an organization and/or its customers. Security professionals have to master the proper security management of information through proper marking, storing, handling, and destruction of assets within their organizations to minimize risk and protect the assets throughout the entire data lifecycle.

# DOMAIN 3

## Security Architecture and Engineering

**SECURITY ARCHITECTURE IS THE** design and organization of the components, processes, services, and controls appropriate to reduce the security risks associated with a system to an acceptable level. Security engineering is the implementation of that design. The goal of both security architecture and security engineering is first and foremost to protect the confidentiality, integrity, and availability of the systems or business in question, in addition to ensuring other important principles such as privacy. This is generally done by following an industry- or government-accepted enterprise or security architecture methodology.

Before designing security architecture, a comprehensive risk assessment must be conducted so that the security architect has an accurate idea of the risks to be addressed. In [Chapter 1](#c01.xhtml), “Security and Risk Management,” risk management, risk assessment, threat modeling, and other approaches are used when determining the risks to the system. Once properly identified and assessed, each risk must eventually be found acceptable by the organization, either as is or after an action is taken. What that action is depends on a number of factors, but it generally occurs in one of four ways:

* Avoid
* Transfer or share (i.e., insurance or contract)
* Mitigate (e.g., through security architecture)
* Accept

Risk assessment is an iterative process. First, the initial risk assessment identifies the risks to be reduced through the design of a security architecture to incorporate appropriate security controls. Then, an assessment must be made to confirm that the resulting system's risks have been reduced to an acceptable level. This is done in line with the principles outlined next.

It may be that during the security architecture process, the costs associated with certain controls are prohibitive relative to the anticipated benefit of the system. As a result, the decision to reduce certain risks may need to be reconsidered, and those risks treated in another manner, avoided through a system redesign, or the project simply abandoned.

Also, security serves to protect the business. The work of the security architect is to ensure the business and its interests at the very least are protected according to applicable standards and laws, as well as meeting any relevant regulatory compliance needs. At times, the organizational leadership's goal to achieve its objectives may appear to be in conflict with compliance. As an essential technical member of the security team, there is a tendency to concentrate on technical security controls and attempt to address all known security issues or requirements. Security for security's sake, while intellectually satisfying, is a disservice to the organization. We must always remember we first serve as subject matter experts, aware of relevant regulations or laws and capable of ensuring our organization's compliance wherever change is required. As for the organization, the same expertise works together with leadership to securely advance the goals of the organization.

It's important to remember that an organization's security strategy must align with its mission, goals, objectives, and compliance environment. Success in security architecture is much more likely when one is aligned with the business and taking a risk management approach to security architecture.

### RESEARCH, IMPLEMENT, AND MANAGE ENGINEERING PROCESSES USING SECURE DESIGN PRINCIPLES

System and application development generally consists of the following stages:

* Design
* Development
* Testing
* Implementation
* Maintenance
* Decommissioning

Secure development principles should be considered at each of the previous stages, but addressing security up front in the Design phase is most critical. Security engineers are often called in to retrofit an existing system with security features designed to protect the confidentiality, integrity, and availability of the system and its data. While this is often necessary, it is best that security be incorporated into the design process (and updated over time, as necessary) for two primary reasons:

* It is less expensive to incorporate security when the overall functional system design is developed rather than trying to add it on later (which will often require redesign, if not reengineering, of already developed components).
* The need for security controls is not just to prevent the user from performing unauthorized actions, but to prevent components of the system itself from violating security requirements when acting on the user's requests. If security is not intrinsic to the overall design, it is not possible to completely mediate all the activities that can compromise security.

Fundamental to any security architecture, regardless of the design principles employed, are the basic requirements outlined in 1972 by James Anderson in *Computer Security Technology Planning Study* (USAF):

* Security functions need to be implemented in a manner that prevents their being bypassed, circumvented, or tampered with.
* Security functions need to be invoked whenever necessary to implement the security control.
* Security functions need to be as small as possible so that defects are more likely to be found.

#### ISO/IEC 19249
