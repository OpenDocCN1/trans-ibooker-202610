
This included development of complex system and policy solutions in a variety of critical infrastructures including the nuclear power, electric power, and water sectors. He developed and deployed both strategic and tactical cybersecurity defensive solutions for the electric power and nuclear sectors.

## The Insights of a Practicing Professional

Getting the opportunity to design an overall computing environment may be one of the most challenging, frustrating, and rewarding endeavors in your career. It will challenge both your technical and interpersonal skills, keep you bound up in detail after detail, and most importantly force you to learn an immense amount about the whole process from start to end. Whatever the environment, remember some key points:

- Function not feature
- Security over convenience
- Business before desire

## BUILDING THE CORE SYSTEM

When designing a new environment, always build the core system by defining function, security, and business processes. Don’t fall prey to a long list of desired features, the whining about security slowing down progress, or all of the disparate wishes from organizational desires to do things their way. If you do, you might as well get on your horse, throw down the reins, turn around in your saddle, start whacking its backside, and hold on for your life, because you definitely will not be in control of your destiny.

## Local Network Security Scenario 1

You have been tasked with making recommendations for equipping a new data network for a small, educational-content development company (fewer than 20 employees). The new company has outgrown its old network and computing equipment and wants to start out in the new facility with a network that meets their current needs.

Because their business is based on the creation of IP (intellectual property) in a market that is highly competitive, they have asked for equipment and configuration recommendations to establish the most secure physical networking environment they can afford.

In particular, the customer has asked that you provide comprehensive recommendations for implementing their server-related security policies and standards. Figure 17.1 provides an overview of the company’s electronic workflow structure.

![](img/2459befd842d6fed299fabe6da3512d0_451_0.png)

**FIGURE 17.1** The Company Layout

The company’s major functions can be organized as follows:

- **Executive Staff:** The executive staff conducts the following electronic activities:
    - Conducts electronic banking activities via the Internet
- **Administrative Staff:** The administrative staff is involved in the following electronic functions:
    - Managing a server-based accounting, warehousing, and inventory program
    - Managing electronic employee payroll and timekeeping records
    - Interacting with the content development team to share content problems reported by customers and offering rough revision materials as required
- **Warehouse and Shipping Team:** These workers are involved in the receiving, inventory tracking, storage of, and shipping of products. These employees have the following electronic activities associated with their jobs:
    - Interacting with the warehouse and inventory portion of the company’s accounting software to update and track product receiving and shipping, as well as current inventory levels
    - Interacting via Internet with Federal Express, United Parcel Service, and over-the-road trucking companies to schedule and track shipments

With this scenario in mind, provide suggestions as to how you would implement their server functions and what security measures should be set in place to provide security for their corporate resources. Recommendations should include, but are not limited to, physically securing the server and server room, establishing policies to protect the company’s intellectual property and personal information, and establishing procedures for maintaining the company’s servers.

## FUNCTIONS FIRST

Base your initial network and platform selection on the functionality that everyone may need. Then determine what organizational functionality may be selected by role.

There are six major departments within the organization. Initially mapping them to a basic role profile that can be implemented by a number of domain controllers (Microsoft, Linux, etc.) is important.

- Executive
- Administrative
- Sales/marketing
- Content development
- Customer support
- Warehouse

The existing functions are initially provided by the scenario, so a role-based access program will provide sufficient operational (and security) access controls for them:

- Electronic banking (A)
- Accounting (B)
- Warehouse inventory (C)
- Payroll and timekeeping (D)
- Customer-contact management system (E)
- Company website management (F)
- Content creation/management (G)
- Functional test lab (H)
- Website access (research, email, and so on) (I)

Building role-based access controls is a two-step process. The domain controllers can allow/disallow access to the application, and the application itself can be discriminant of users and privileges. This multi-authentication approach is a good idea. With such a small company, however, there may not be a domain controller, so you’ll have to take that into consideration.

With 20 people or so in the company, a central server (and backup preferably off-site) will provide good functionality. Your initial role-based architecture might look like Figure 17.2 (with the capital letters representing the application function).

Usually, almost everyone in the company will need to access the Internet. In reality, giving access to online email, banking, and a few Internet-surfing liberties won’t be a burden to your network or your computing platforms. What it can do, however, is challenge productivity! To help prevent employee slacking, disallow excessive content streaming (music services, Amazon Prime video, and so on) that can disrupt your business.

Now that you generally have a role-based, electronically enabled user policy, you can determine how the application itself will provide the separation of roles and duties necessary to maintain the operational integrity of the software systems deployed. As an example, everyone in the timekeeping system would need to input their time but not be able to access or report anyone else's. The “timekeeper,” however, would need to have access to those higher-level functions of the payroll software. What you're trying to avoid is the warehouse being able to access the customer contact database.

![](img/2459befd842d6fed299fabe6da3512d0_455_0.png)

**FIGURE 17.2** Role-Based Architecture

Proprietary information and intellectual property are main concerns in this scenario. The role-based access-control methodology will extend into these concerns. The objectives are to limit access to the information, keep it from leaving the company, and ensure that your employees understand that there are serious ramifications if they themselves compromise it. A good document management plan, policy, and procedures will provide a sound basis to protect this information. Some basic principles include keeping the company proprietary and intellectual information inside the system. Don't let employees take it home on a thumb drive, don't email it all over the company, and don't let just anyone access it. Consider the loosely managed environment shown in Figure 17.3.

![](img/2459befd842d6fed299fabe6da3512d0_456_0.png)

**FIGURE 17.3** Loosely Managed Environment

Just a simple “I’ll get this done tonight!” scenario will significantly compromise your files or data. In the worst case, data comes off the work computer, onto a removable device, to a laptop, to a home network (or computer if it has the larger screen you want to use), back onto the laptop, and back to the office computer. From a security perspective, this is a nightmare! From a file or data perspective, the sheep have left the pen! Remember, this is a worst case; it can be argued that the employee has malware/antivirus protection, the file is deleted (even wiped) on all devices, and further it is checked before it is put back on the network. Would everyone really take the time to do all of these tasks? Even with these protections, your files have left the premises. This is not what you want to have happen.

The potential solutions are abundantly more secure. As an example, you could implement the simple, cost-effective solution discussed here:

Assume that the employee needs to be off-site for some reason. You want to accomplish the following:

1. Keep the file(s) on the premises.
2. Keep the development/work environment on the premises.
3. Maintain positive control of both while the work is being completed.

This means you need to extend the desktop in a secure fashion. As shown in Figure 17.4, we usually accomplish this with a simple VPN connection back into the office and extend the remote desktop (RDP) session to the employee’s
