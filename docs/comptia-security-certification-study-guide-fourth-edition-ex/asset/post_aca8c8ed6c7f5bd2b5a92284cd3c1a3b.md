
## Performance-Based Question

15. Using the exhibit, match the term on the left with its corresponding definition on the right.

RPO

MTTF

RTO

MTBF

MTTR

Time between failures

Determines how much data loss is acceptable

Amount of time before a device fails

Amount of time to recover a system

Amount of time allowable for the restoration of a business function

## SELF TEST ANSWERS

## Introduction to Business Continuity and Disaster Recovery

1. ☑ B. It is critical that you obtain management support to create a BCP because it will take up a lot of your time and other company resources. ☒ A, C, and D are incorrect. None of these represents the first step in creating a BCP.

2. ☑ B, D, and E. The business impact assessment is the risk management part of the BCP and involves identifying the critical business functions, identifying the threats associated with the functions, and determining how long the company can go without those functions. ☒ A and C are incorrect. These are not steps performed during the BIA.

3. ☑ D. A structured walkthrough involves the BCP committee getting together and reviewing the BCP. This helps identify oversights in the plan and ensures everyone knows their responsibilities.
☒ A, B, and C are incorrect. A checklist review is when each department is sent a copy of the BCP to review individually. A simulation test is when different parts of the plan are actually tested for completeness, and a parallel test involves testing the alternative site by bringing the systems online at that site.

4. ☑ A. A warm site involves having the site available with the equipment, but you are required to restore the most recent copy of the data. For example, the e-mail server may be present, but you need to restore the most recent backup of the e-mail server data.
☒ B, C, and D are incorrect. A hot site does not require restoration of data, because it is continuously synchronized with the main site. A cold site does not have the servers and other systems—it simply is the location with a little bit of furniture. There is no such thing as a spare site.

5. ☑ A. In order to access data when a drive fails, you need to configure the drive in a RAID solution. RAID is a group of drives that work together to provide the data. When one drive fails in the group, the remaining drives provide the missing data.
☒ B, C, and D are incorrect. Server clustering is used to provide high availability to services such as e-mail and database servers. Load balancing is used to distribute the workload across multiple devices or servers, and virtualization is a method of running multiple systems on a single computer.

6. ☑ C. Tom should use load balancing to split the workload of requests across multiple servers.
☒ A, B, and D are incorrect. RAID is used to create fault tolerance on data, and a proxy server is used to provide a central point through which clients can access the Internet. The proxy server can log their activity and control which sites they can visit. Virtualization is a method of running multiple systems on a single computer.

7. ☑ D. Recovery point objective is determining what is acceptable data loss in your recovery strategy.
☒ A, B, and C are incorrect. Mean time to restore (MTTR) is the amount of time it takes to recover, while mean time to failure (MTTF) and mean time between failures (MTBF) determine the amount of time for a device to fail.

8. ☑ B. Succession planning is determining who the new leaders in an organization will be in case of a disaster.
☒ A, C, and D are incorrect. Business impact analysis is determining the value of each business function and the impact of its loss. Continuity of operations is ensuring that the business operations still continue in the event of a disaster, and disaster recovery is ensuring the recovery of systems.

## Backing Up and Restoring Data: Backup Concepts

9. ☑ C. An incremental backup is responsible for backing up what has changed and clearing the archive bit.
☒ A, B, and D are incorrect. A full backup backs up all selected data and then clears the archive bit, while a differential backup backs up only changes but does not clear the archive bit. Offsite is not a backup type, but is where a copy of backups should be stored in case of disaster.

10. ☑ B. Ensure that you protect your tape backups by encrypting the data on tape or by requiring a password to do a restore operation.
☒ A, C, and D are incorrect. These do not represent secure practices with tapes. The tapes should be stored in a secure location and password protected or encrypted.

## Implementing Fault Tolerance

11. ☑ A. Remember for the exam that RAID allows you to prepare for disasters related to disk failures.
☒ B, C, and D are incorrect. Although backups can help you prepare for disasters with drive failures, RAID is the better answer because it allows continuous access to the data. Load balancing is used for services such as web servers to allow the application to respond more quickly by having multiple servers that answer the request. Clustering is having multiple nodes running a service so that if one service fails, the other node can take over the request.

12. ☑ A. RAID 1 mirrors the data from one drive to another.
☒ B, C, and D are incorrect. RAID 5 is the striping of data across all drives, with parity information being written so that any missing data can be regenerated. Although not mentioned in this chapter, RAID 3 works in a similar fashion to RAID 5, but the parity information is written to one single disk. RAID 0 is disk striping that has no fault tolerance, but it improves performance by splitting the write operation across multiple disks.

## Understanding High Availability

13. ☑ B. Load balancing allows you to group multiple servers as a single unit, with each server being able to handle the request.
☒ A, C, and D are incorrect. Clustering allows you to have multiple servers provide the same service in an active/passive configuration—if the active server fails, the passive node takes over. RAID 5 is the striping of data across all drives, with parity information being written so that any missing data can be regenerated. Doing a full backup does not provide high availability or provide any performance benefit—it just allows you to recover lost data.

14. ☑ A. Clustering allows you to have multiple servers provide the same service in an active/passive configuration—if the active server fails, the passive node takes over.
☒ B, C, and D are incorrect. Load balancing allows you to group multiple servers as a single unit, with each server being able to handle the request. RAID 5 is the striping of data across all drives, with parity information being written so that any missing data can be regenerated. Doing a full backup does not provide high availability—it just allows you to recover lost data.

## INSIDE THE EXAM

## Understanding Types of Monitoring Systems

For the certification exam, remember the different types of monitoring systems. Common types of monitoring systems are signature-based, anomaly-based, and heuristic-based analysis.

A signature-based system detects suspicious activity based on the signatures in a file, whereas an anomaly-based system knows the normal activity (the baseline) and considers anything outside of the norm to be suspicious. A heuristic-based system identifies suspicious activity based on the manufacturer programming the device for the types of activity that have caused security problems in the past. Heuristic-based IDs are great for monitoring for zero-day exploits. For more information on these intrusion detection systems, check out Chapter 8.

Remember that an anomaly-based system is great for detecting a zero-day exploit because it is an unknown exploit, and a signature-based system would not have the signature of the exploit yet.

An anomaly-based system understands what is considered normal activity (a baseline) and then considers anything outside that normal activity to be “suspicious” activity. The anomaly-based monitoring system typically determines the baseline from the behavior of the person using the system. This is known as a behavior-based anomaly monitoring system.

The benefit of the behavior-based anomaly system is that you do not need to configure a definition file of known suspicious activity; the system learns what is normal based on the users’ activity. The drawback of a behavior-based system is that anything outside the norm is considered suspicious, which results in a large number of false alarms (false positives).

While reviewing the concepts of intrusion detection systems, we should also review the concept of a honeypot. A honeypot is a fake system that is set up to lure the attacker to that system. The honeypot system will be hardened so that it is not too easy for the attacker to get into, and while they are attempting to compromise the system, there is a high level of logging and auditing that is tracking the attacker.

Also note that you could have a single file set up as an attractive asset for the attacker, known as a honey file. For example, you could place a file on the desktop of the system called password.txt so that when attacker finally gets into the system, they see this file and open it. You will implement auditing on the text file so that you know when it is opened and by whom.

## exam watch

For the Security+ exam, remember the concept of a honeypot and honey file.
