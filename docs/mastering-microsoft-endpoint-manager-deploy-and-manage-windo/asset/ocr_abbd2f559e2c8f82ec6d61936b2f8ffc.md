# AVD and Windows 365 Cloud PC – shared responsibility model 1

As with many cloud services, there is a shared set of security responsibilities. You have control and flexibility, and with that comes responsibility. If you are adopting Windows 365 Cloud PC, it's important to understand that while some components come already secured for your environment, there are other areas where you will need to configure things to fit your organization's security needs:

| Responsibility | Traditional VDI | Azure Virtual Desktop | Windows 365 |
|---|---|---|---|
| Virtualization Management Plane | Customer/Partner | Microsoft | Microsoft |
| Identity Platform (Azure AD) | Customer/Partner | Microsoft | Microsoft |
| Platform Security | Customer/Partner | Microsoft | Microsoft |
| Networking | Customer/Partner | Microsoft | Microsoft |
| Data Center | Customer/Partner | Microsoft | Microsoft |
| Image Management Platform (via MEM) | Customer/Partner | Customer/Partner | Microsoft |
| Operating System Updates | Customer/Partner | Customer/Partner | Customer/Partner |
| End User Devices (Physical PC/Mobile) | Customer/Partner | Customer/Partner | Customer/Partner |

Table 1.1 – Shared responsibility model 1