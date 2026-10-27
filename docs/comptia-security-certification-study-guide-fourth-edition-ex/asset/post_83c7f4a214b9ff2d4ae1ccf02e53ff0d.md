
## INSIDE THE EXAM

### BCP and DRP Terminology

For the Security+ certification exam, it is critical that you understand BCP and DRP concepts such as business impact analysis, alternative sites, and having a functional recovery plan, but the Security+ certification exam also expects you to know a few other terms related to business impact assessment:

- Succession planning is the process of ensuring that you have employees within the organization who can fill key leadership roles to ensure that the business can continue if you lose key personnel.
- IT contingency planning is the preparation of a recovery plan for when something goes wrong with the IT systems and infrastructure.
- Mean time to repair (MTTR), also known as mean time to recovery, is the average time for a system or device to recover from a failure.
- Mean time between failures (MTBF) is the amount of time between failures of a system or device.
- Mean time to failure (MTTF) is the amount of time a device is expected to last in production before it fails. MTTF is usually a value reported by the manufacturer on hardware, which you can use as evaluation criteria when selecting hardware.

The difference between MTBF and MTTF is that a device such as a RAID array may have a MTBF that indicates how frequently you can expect the RAID array to fail, but due to the redundancy you most likely will not lose data (it is considered repairable). The MTTF is a rating typically on hardware such as hard drives and is a time specified that the drive may fail. This failure is permanent and unrecoverable, so you put the drive in the RAID array, which is a recoverable/repairable technology.

- Recovery time objective (RTO) is a BCP term for the amount of time allowable before a business function must be restored to a functional state after a failure.
- Recovery point objective (RPO) is a BCP term to represent how much of a system is expected to be recovered. For example, your company may expect that when a system fails, you should be able to restore up to the point of failure, whereas another company may only expect recovery of data up to 24 hours prior to the point of failure.

You are sure to get some questions on RPO, RTO, MTTR, MTTF, and MTBF on the Security+ exam, so know those terms well.

### Types of Backups

Backup software enables you to run a number of types of backups such as full, incremental, differential, and copy backups. You may see backup concepts related to these four types of backups on the exam, so we will focus on them in this section in addition to talking about snapshots. The key to backing up data is to ensure that you can restore it in the event of a system failure. These three types of backups will function well together if correctly used.

#### Full Backup

A full backup backs up every file on the specified volume or volumes (or partitions). Many companies run a full backup every day, no matter what. Under such a system, the restore process requires only the most recent tape. However, a full backup necessitates a large storage capacity and a lot of time. If you have large amounts of data, running a daily full backup may not be practical because it may take too long to perform. Full backups are easy to restore from because you only need to perform a restore from the one backup set.

Every file has an archive bit that flags whether the file needs to be backed up. When you change a file, this bit is flagged automatically, which means that the file needs to be backed up. In theory, any file that has been changed needs to be backed up because you want to be sure you can always bring the file to its most recent state.

To view the archive bit in Windows, right-click the file, choose Properties, and click the Advanced button on the General tab. You will see the option File Is Ready for Archiving, as shown in Figure 18-1.

**FIGURE 18-1 Looking at the archive bit in Windows**

![](img/3e3af33e2c753e920c09ecd40b5a457a_745_0.png)

> **Exam Watch**
> Full backups back up every file that is selected and then clear the archive bit.

The important thing to understand when a full backup is performed is that it backs up all the files you select (whether the archive bit is set or not), and then it clears the archive bit so that the operating system and applications know that the file has been backed up.

#### Incremental Backup

An incremental backup backs up only the files that have changed or that were added since the last incremental or full backup. It does this by backing up only files that have the archive bit set (meaning the file needs to be backed up). This is different from a full backup in that a full backup will back up any file that is selected because a full backup does not read the archive bit to determine whether to back up the file—it simply backs up whatever you tell it to. An incremental backup will back up whatever files you have selected that have the archive bit set.

> **Exam Watch**
> Incremental backups back up any files that have changed and then clear the archive bit so that the next backup will not back the file up (unless you do a full backup).

Using a combination of the full backup and incremental backup is highly effective and less time-consuming than running a full daily backup. A number of companies will set up a backup schedule that performs a full backup Friday night and then backs up only the changes each night through the week by performing an incremental backup. However, to restore, you will need the last full backup tape as well as every incremental backup tape made since the last full backup.

An incremental backup clears the archive bit to report that the file has been backed up. If you were to perform an additional incremental backup the next day, the same file would not get backed up (unless it was changed), because the archive bit would not be set. Your restore strategy (known as the order of restoration) with incremental backups is to restore the full backup and then restore each incremental backup since the full backup. This will ensure that you get the buildup of changes since the full backup.

#### Differential Backup

A differential backup backs up the files that have changed or that were added since the last full backup by looking for any files that have the archive bit set. The differential backup is different from the incremental in that the differential does not clear the archive bit after the backup is done. This means that if you were to do another differential backup the next time you backed up, the same files (and any new ones) would get backed up. As a result, every differential backup will have all changes since the last full backup. The order of restoration strategy would be to restore the last full backup and then to restore your last differential backup.

> **Exam Watch**
> Differential backups back up any files that have had changes, but they do not clear the files’ archive bit. Because the archive bit is not cleared, each differential backup will back up all files changed since the last full backup.

Differential and incremental backups can make the restoration process a little more complex because you have to restore from the full backup first and then restore from the incremental or differential backups to make sure any files that have changed since the last full backup are restored. An important difference between differential and incremental backups is that incremental backups take less time to perform (because you are getting only changes since the last full or incremental backup) but more time to restore (because you are restoring multiple incremental backups). Differential backups take more time to perform but less time to restore.

If you decide to take the simpler but more time-intensive approach by performing full backups each time, you can restore from the most recent full backup and get all the files restored in one session.

> **Exam Watch**
> You may see a question or two on the exam about the different types of backups. Be sure to know the differences between full, incremental, and differential backups and also be comfortable with the restoration order with each scenario.

#### Copy Backup

A copy backup is like a full backup but it does not clear the archive bit. This is important because a copy backup will not disrupt the backup cycle if you run this type of backup. Copy backups are useful if you decide you need to back up the data in the middle of a backup cycle—for example, head office asks you to send a backup of all the data. You can run the copy backup knowing it will not mess with your regular backup strategy.
