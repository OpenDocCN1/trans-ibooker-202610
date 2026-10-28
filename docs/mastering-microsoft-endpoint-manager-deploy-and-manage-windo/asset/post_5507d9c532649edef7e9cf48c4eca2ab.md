
# Section 3: Mastering Microsoft Endpoint Manager

5

## Requirements for Microsoft Endpoint Manager
Self-service capabilities – IT admin 87
Reprovisioning the cloud PC 87
Local administrator 88
VM SKU upgrades (preview feature) 88
Image management – creating a custom image (optional) 89
Supported endpoints 95
Information Worker Portal (IWP) 97
Azure AD – MyApps unified (workspace) portal 101
Multi-factor authentication and conditional access 102
Security baselines for a cloud PC 105
Distributing the Remote Desktop client via Microsoft Endpoint Manager – Intune to your physical endpoints 106
Auto-subscribing users in the Remote Desktop client 117
Autopilot and cloud PCs – lightweight thin client (Kiosk) 120
Monitoring and analytics 122
Shadow users with Quick Assist 123
Windows 11 124
Microsoft Managed Desktop 125
Summary 127
Questions 127
Answers 127
Further reading 127

## Endpoint scenarios
Identity roles and privileges for Microsoft Intune 132
Compliance Administrator 132
Compliance Data Administrator 133
Intune Administrator 133
Message Center Reader 133
Security Administrator 133
Security Operator 133
Security Reader 134
Identity roles and privileges for a Windows 365 cloud PC 134
Azure Subscription Owner 134
Intune Administrator 134
Domain Administrator 135
Identity roles and privileges for Universal Print 135
Printer Administrator 135
Printer Technician 135
Licensing requirements 135
Supported OSes 136
Required web browser versions 137
Windows 11 requirements 137

7

## Manager Windows Autopilot
Technical requirements 212
Windows Autopilot overview 212
Uploading the hardware ID to Windows Autopilot 216
Windows Autopilot for existing devices 226
Windows updates during the Out-of-Box Experience (OOBE) 229
Auto-assigning Windows Autopilot profiles in Intune 230
Signing in to Graph Explorer 232
Enrollment Status Page (ESP) 235
ESP implementation Windows CSP 236
Autopilot reporting and diagnostics 237
Company Portal 240
Configuring automatic BitLocker encryption for Autopilot devices 240
Cloud configuration scenario 246
Deploying essentials that users might need to access work or school resources 253

## Edge kiosk self-deployment scenario 253
Creating a specific ESP for the Edge kiosk 253
Creating a Windows Autopilot profile 254
Self-Deploying (preview) 255
Autopilot Reset 260
Wiping and resetting your devices 262
Fresh start 263
Windows Recovery Environment 264
Summary 264
Questions 265
Answers 265
Further reading 265

8

## Application Management and Delivery
Application delivery via Microsoft Endpoint Manager 268
Different application types you can deploy 268
LOB applications 270
Supersedence mode 288
Community tool – Win32App Migration Tool 292
Deploying Microsoft 365 apps 293
Update channels 295
Office Customization Tool 296
Microsoft 365 Apps admin center 298
Microsoft 365 apps – customization 306
Deploying Microsoft Teams 307
OneDrive 308
Deploying Microsoft Edge 310

11

## Office Policy Management
The Office cloud policy service 400
Creating a policy configuration with the OCP service 402
Configuring policies 405
Tips and tricks in the OCP service 408
How are Office cloud policies applied? 412
Security Policy Advisor 415
Summary 418
Questions 419
Answers 419
Further reading 419

12

## User Profile Management
Windows profiles 422
Modern profile management 423
Enterprise State Roaming 423
Microsoft Office's roaming settings 425
Outlook's signature cloud settings 427
OneDrive for Business Known Folder Move 428
Windows 10 Storage Sense 430
OneDrive and Storage Sense 431
Microsoft Edge 432
ESR + OneDrive + Edge + Office 434
Migrating from legacy to modern profile management 435
Summary 436
Questions 436
Answers 437
Further reading 437

13

## Identity and Security Management
Microsoft Identity 440
AAD 441
AAD users 441
AAD guest users 442
AAD group types 442
AAD membership types 443
Hybrid AAD 446
Conditional Access 450
Users and groups 451
Cloud apps 452
Conditions 454
Questions 396
Answers 397
Further reading 397

14
Monitoring and Endpoint Analytics

## Monitoring and analytics 496
Monitoring your physical and virtual cloud endpoints 496
Endpoint analytics – advanced monitoring 499
Start up performance – logon duration 499
Performance score breakdown 500
Top 10 impacting start up processes 506
OS restart history 507
Resource performance 508
Insights and recommendations – score trends 508
Application reliability 509
Windows 365-specific metrics 510
Insights and recommendations 512
Configuration Manager data collection 513
Customizing your baselines 514
Proactive remediations 515
Azure Monitor integration 517
Productivity Score 518
Service health 519
Summary 521
Questions 521
Answers 521
Further reading 522

15

## Universal Print
- What is Universal Print? — 523
- Universal Print – architecture explained — 524
- The print connector — 526
- Where does my printed data go? — 526
- Printer defaults — 526
- Universal Print – service requirements — 527
- Network requirements — 528
- Learning how to deploy Universal Print — 528
- Delegating printer access – custom roles — 529
- Connecting your existing printer to Universal Print — 530
- Configuring Universal Print — 531
- Enabling Hybrid AD configuration – via the Universal Print connector — 536
- Registering your own custom printers with Universal Print — 538
- Sharing your printers with your users — 539
- Assigning permissions to use a printer(s) — 540
- Testing your Universal Print connected printer — 541
- Assigning and deploying cloud printers with Microsoft Endpoint Manager — 546
- Summary — 556
- Questions — 556
- Answers — 556
- Further reading — 557

## Section 4: Tips and Tricks from the Field

16

## Troubleshooting Microsoft Endpoint Manager
- Troubleshooting MEM — 562
- Service health and message center — 564
- Troubleshoot blade in MEM — 566
- Troubleshooting Windows 10 MEM enrollment — 570
- BitLocker failures — 574
- Windows 10 device diagnostics — 575
- Client requirements — 575
- Troubleshooting application delivery — 579
- Win32 — 579
- LOB — 581
- Microsoft Store apps — 581
- Troubleshooting Autopilot — 582
- Windows 11 Autopilot diagnostics page — 584
- Troubleshooting locating a Windows device — 588

## Table of Contents
- Troubleshooting
- Microsoft Edge — 592
- Summary — 594
- Questions — 594
- Answers — 594
- Further reading — 594

17
## Troubleshooting Windows 365
- Troubleshooting yourself and Microsoft Support — 596
- Windows 365 provisioning errors — 602
- Cloud PC – device-based filtering — 611
- Summary — 616
- Questions — 616
- Further reading — 616

18
## Community Help
- Join the new W365 Community! — 617
- Microsoft Tech Community and MS Learn — 618
- Other community blogs, Microsoft MVPs, and more... — 618
- Summary — 620

## Other Books You May Enjoy

## Index

## Preface
One of the main reasons for the slow adoption of Modern Workplace solutions designed to simplify the management layer of your environment is the lack of understanding and knowledge of the product. With this book, you'll learn everything you need to know to make the shift to the Modern Workplace, running Windows 10, Windows 11, or Windows 365.

*Mastering Microsoft Endpoint Manager* explains various concepts in detail to give you the clarity to plan how to use **Microsoft Endpoint Manager (MEM)** and eliminate potential migration challenges beforehand. You'll get to grips with using new services such as Windows 365 Cloud PC, Windows Autopilot, profile management, monitoring and analytics, Universal Print, and much more. The book will take you through the latest features and new Microsoft cloud services to help you to get to grips with the fundamentals of MEM and understand which services you can manage. Whether you are talking about physical or cloud endpoints it's all covered.

By the end of the book, you'll be able to set up MEM and use it to run Windows 10, Windows 11, and Windows 365 efficiently.

What you will learn:
- Simplify the deployment of Windows in the cloud with Windows 365 Cloud PC.
- Configure advanced policy management within MEM.
- Discover modern profile management and migration options for physical and cloud PCs.
- Harden security with baseline settings and other security best practices.
- Find troubleshooting tips and tricks for MEM, Windows 365 Cloud PC, and more.
- Discover deployment best practices for physical and cloud-managed endpoints.
- Keep up with the Microsoft community and discover a list of MVPs to follow.
