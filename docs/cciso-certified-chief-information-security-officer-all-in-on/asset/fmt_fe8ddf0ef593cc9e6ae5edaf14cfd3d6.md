
# Third-Party Risk Management

## Types of Third-Party Relationships

- **Developed applications** Organizations may acquire from third-party vendors custom-developed applications that contain vulnerabilities and introduce risk into the enterprise.
- **Consultants and service providers** Organizations commonly use third-party vendors to provide services ranging from legal services to IT consulting to HVAC contractors. Any vendor that connects to the organization’s network can introduce risk.
- **Cloud services** X-as-a-service vendors are unique in that they provide assets that connect directly to the organization’s network and/or provide enterprise services turnkey.

## Adapting the NIST Risk Management Framework (RMF)

Although third-party risk management programs can be implemented in many ways, the approach discussed here is to adapt the NIST Risk Management Framework (RMF). RMF is usually thought of as a model used to apply information security controls to systems, but it can be tailored to provide a framework for applying security controls to vendor relationships. [Figure 5-10](#ch5.xhtml#fig5_10) shows the typical RMF phases. The application of each phase to third-party security is discussed next.

### Figure 5-10: NIST Risk Management Framework

![Figure 5-10 NIST Risk Management Framework](Images/F05-10.jpg)

## RMF Phases Applied to Third-Party Security

### Prepare

The Prepare phase involves the creation of the artifacts and tools used to support each of the other phases as well as obtain feedback during each of those phases to facilitate improvements. During this phase the CISO prepares the following:

- **Third-party risk management policy** The policy reflects all requirements that guide the third-party program. Topics within the policy may include the following:
  - Third-party roles and responsibilities and organizational structure
  - Requirements pertaining to contract employees (since they are not employees of the organization and therefore HR policies do not apply to them)
  - Sections covering requirements for physical, asset, communications, and operations security
  - Product development, including security during the SDLC
  - Configuration, patch, and change management
  - Incident management
  - Business continuity
  - Monitoring and auditing
  - Legal and compliance
  - Documentation and recordkeeping

- **Tailoring of other policies** Some organizations prefer to keep the third-party policy lean and write it to contain reference to other policies. In this case those policies may require a review and tailoring to ensure they address third-party requirements correctly.

- **Supporting standards, guidelines, and processes** In addition to policies, the organization may develop standards, guidelines, and processes to be used as guidance for third parties or for individuals within the organization tasked with supervising or working with third parties.

- **Acquire assessment tools** The organization may acquire tools to be used to assess the security performance of third-party vendors. For instance, if a cloud service provider agrees to let the organization conduct vulnerability scans, the organization may identify and acquire scanning tools for this purpose.

- **Prepare questionnaires** Information about the vendor may also be gathered in the form of a vendor security questionnaire that asks specific security program questions. The answers are often used to make procurement decisions. The Vendor Security Alliance (<https://www.vendorsecurityalliance.org/>) provides guidance for preparing vendor questionnaires for this purpose.

- **Line up assessors** The organization may choose to use external assessors to conduct scans or perform other evaluations of a third party’s security capabilities.

- **Contract terms and artifacts** The CISO may prepare in advance contract terms and other language that will be used in third-party agreements.

The remaining phases are applied to each candidate third-party vendor, product, or service. At each phase, feedback is obtained and fed back into the Prepare activities to improve the preparation for the next time the process is used or for the next vendor.

### Categorize

During the Categorize phase, the vendor, product, or service is categorized. Categories should be established in advance, with corresponding controls established for each. To illustrate, an organization may define three categories of third-party relationships as follows:

- Product
- Service provider
- Custom software developer

During the next phase, security controls are selected for each. Some organizations use a highly structured approach whereby they predefine controls for given categories, allowing for faster identification of controls for a new vendor.

### Select

During the Select phase, specific controls are chosen. Using the categories illustrated previously, here is a further illustration showing controls.

- **Global controls** These controls are artifacts that apply to all categories. These artifacts are tools used to establish the security posture of the vendor:
  - **Reputation of the security product or vendor** When establishing trust in a product, sometimes the reputation of the vendor is a valuable consideration. A vendor that has historically failed to deliver patches in a timely manner may not compare well against one that has delivered promptly.
  - **Independent assurance** Evidence that the product or vendor has been independently audited by a credible security assurance entity may be required. For instance, audits and third-party attestations can be performed following standards including SOC 1, SOC 2, ISO/IEC 27001, or FedRAMP.
  - **Legal agreement** A legal agreement may be used to establish a commitment from the vendor to the organization, for instance, to assume liability for a security breach.
  - **Security review** The organization may conduct a security review of the product or service to determine its security posture and suitability for use by the organization.

- **Product controls** Controls that may apply to the acquisition of a hardware or software product or system may include (in addition to the global controls listed previously) verification of the following:
  - The vendor has a program for timely reporting of vulnerabilities.
  - The vendor has a program for the timely release of security patches.

  Product controls may apply to COTS or open source products.

- **Service provider controls** Important controls may include verification that the vendor has and can provide evidence of the following:
  - An established information security program
  - Security policies, standards, and procedures
  - Security procedures for *fourth parties* (third-party vendors of the vendor)
  - Willingness to submit to security testing
  - Validation by an independent testing or auditing entity (such as a SOC 2 audit)

- **Custom software developer controls** In addition to all of the controls listed for the categories, software development organizations should be evaluated to determine the quality of their secure coding program and their SDLC.

### Implement

Once the security controls for the product or service vendor are chosen, they are implemented. Contracts are written, negotiated, and executed. Audits and testing take place. Assurance vendors are engaged. The organization reviews vendor-provided documentation such as security policies and procedures. The Implement phase puts into action all of the controls identified from the Select phase.

### Assess

In the Assess phase, all of the artifacts collected during the Implement phase are collected and the totality of the information is reviewed to make a determination regarding the use of the product or service. If there are control shortcomings, they are assessed for their risk impact to the business. As introduced in [Chapter 1](#ch1.xhtml), there are four choices for how risk can be handled:

- *Mitigate* or reduce the risk by putting an additional control in place
- *Transfer* the risk (usually by using an insurance policy)
- *Accept* the risk when the organization can “live with” the risk
