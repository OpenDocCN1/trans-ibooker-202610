
## Other Aspects of Organization Recovery

There are other aspects of organization recovery that must also be considered, including, but not limited to, the following:

- **People** Ensuring safety of human life, adequate staffing during a disaster, and proper succession plans are in place
- **Documentation** Ensuring that critical documentation (policies, procedures, plans, and so on) is available
- **Hardware** Consideration of network and computer equipment, replacement time requirements, SLAs from suppliers, as well as dangers of legacy and/or proprietary devices
- **Software** Necessary applications, utilities, and operating systems for production
- **Communication** Plans for communication to various stakeholders (customers, suppliers, media, and shareholders)
- **Infrastructure** Requirements for voice and data communication, electric, and HVAC
- **Supplies** Requirements for paper, cabling, physical storage, and so on
- **Other risks** Potential for looting, vandalism, or other fraudulent activity after a disaster

## Executive Succession Planning

The temporary or permanent loss of an organization’s senior leadership can cause significant harm to the organization’s operations. It is critical that organizations have an executive succession plan in place to protect the continuity of operations and leadership when a loss of executive staff is experienced. For example, the succession plan for the CISO may specify that the deputy CISO, information security officer (ISO), or information security manager (ISM) takes the helm. Succession planning is all about being ready to deal with these circumstances, so that a company can be skillfully guided through this critical period following extreme circumstances.

## User Environment Recovery

Recovery of the end-user environment is critical to resuming organizational operations. This includes decisions around notification to employees, ensuring critical employee resources are available, and ensuring safe working conditions for employees. To facilitate the recovery of the employee environment, the BCP team must do at least the following:

- Develop a calling tree to inform employees of the disaster and to keep people updated
- Identify the critical users required to keep critical functions running
- Identify how current automated tasks can be carried out manually
- Identify user resource and supply requirements
- Develop transportation procedures for users to the new facility if required

## Data Restoration

The traditional approach to restoration of data lost due to a disaster is to use a data backup solution. With this approach, data is backed up to centralized storage such as cloud storage, network attached storage (NAS), or storage area networks (SANs). Backup procedures should be documented, with regular backups taking place on a daily, weekly, and/or monthly basis. The three main types of data backups are as follows:

- **Full** Backs up *all* files on the system. Provides the fastest restoration but takes the longest to perform the backup.
- **Incremental** Backs up all files that have *changed since the last backup of any type*. Backups can be performed more quickly but restoration takes longer because the full backup must be restored as well as each incremental backup performed thereafter.
- **Differential** Backs up all files that have changed since the last *full backup*. For restoration, the full backup is restored and then the most recent differential is restored.

![Images](Images/exam.jpg)

**EXAM TIP** CCISO candidates should be familiar with the different types of backups.

In a perfect world, backed-up data is restored to the exact state it was in just prior to the disaster; in practice, the backup/restore process may result in a loss of some data due to the time between the last backup and the disaster event. To eliminate the potential for the loss of data using backups, organizations can use other methods to ensure *data resiliency*. One such solution is *disk mirroring*, which provides redundancy by writing the same data to more than one disk. If either storage medium is lost, the other is available for operations. Modern mirroring solutions can provide fast and seamless recovery from disasters, although there are implementation factors to consider. If the redundant disks are in the same location, they may be subject to the same disaster, such as a flood or fire, which makes such an implementation unsuitable for recovery from such an event. Also, the same event that caused data to be lost, such as a server going down and causing write errors, may produce the same errors on both disks, rendering the data unrecoverable. Therefore, many organizations mirror a partial solution for data recovery but still rely on backups.

Data can be stored on a variety of mediums, including tape and disks. Redundancy can be implemented at the tape or disk level using RAID (for disks) or RAIT (for tapes). *Redundant array of independent disks (RAID)* is a technology used for redundancy across disks. RAID essentially allows data to be written across multiple disks to allow for faster read/write operations as well as redundancy. The level of redundancy is dependent on the level of RAID that is used. *Redundant array of independent tapes (RAIT)* is similar to RAID but uses tapes instead of disks.

![Images](Images/note.jpg)

**NOTE** For some organizations utilizing cloud services, the cloud service provider may provide automated backup and recovery functionality as part of the subscription. This may be included in the offering or available as an add-on. In either case, it is important to ensure that data in the cloud is backed up and can be restored just as reliably as data stored locally.

## Singe Point of Failure

*Any* system or network that uses only one component to do a job creates a *single* *point of failure*. If that single component fails, there is no alternate one to take its place. Examples of controls to mitigate the risk of single points of failure include:

- Redundant LAN routes
- On-demand backup WAN connections
- Redundant and fault-tolerant technologies
- Documented procedures on how to deal with failures
- Properly trained personnel

## Develop the Plan

After the BCP team has developed the BCP policy, performed the BIA, implemented preventive controls, and developed recovery strategies and solutions, the next step is to develop the plan. The general structure of a business continuity plan is illustrated in [Figure 4-24](#ch4a.xhtml#fig4_24).

![Images](Images/F04-24.jpg)

**Figure 4-24** BCP structure (Source: NIST)

### BCP Confidentiality

Most organizations have two versions of their business continuity plan, an internal version and a sanitized version to share with customers and other critical partners. Customers in particular may be interested to know that the organization has a BCP, and some may require receiving a version of it to satisfy their own compliance requirements. However, the BCP by its very nature is a sensitive document that could cause harm to the organization if released. It details the recovery technologies, solutions, and strategies utilized by the organization in the event of a disaster. This information could be very valuable to an attacker. This is why a sanitized “public” version is developed. The sanitized BCP should not contain:

- Contact lists
- Business recovery procedures
- Process flows
- Risk mitigation methods
- BIA results

The sanitized version should be specific enough for customers to satisfy their security and compliance requirements but general enough as to not reveal sensitive information.

## Test the Plan
