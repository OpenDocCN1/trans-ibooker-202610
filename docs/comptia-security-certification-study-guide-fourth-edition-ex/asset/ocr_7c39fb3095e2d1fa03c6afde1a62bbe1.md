# Types of Backups

Backup software enables you to run a number of types of backups such as full, incremental, differential, and copy backups. You may see backup concepts related to these four types of backups on the exam, so we will focus on them in this section in addition to talking about snapshots. The key to backing up data is to ensure that you can restore it in the event of a system failure. These three types of backups will function well together if correctly used.

# Full Backup

A full backup backs up every file on the specified volume or volumes (or partitions). Many companies run a full backup every day, no matter what. Under such a system, the restore process requires only the most recent tape. However, a full backup necessitates a large storage capacity and a lot of time. If you have large amounts of data, running a daily full backup may not be practical because it may take too long to perform. Full backups are easy to restore from because you only need to perform a restore from the one backup set.

Every file has an archive bit that flags whether the file needs to be backed up. When you change a file, this bit is flagged automatically, which means that the file needs to be backed up. In theory, any file that has been changed needs to be backed up because you want to be sure you can always bring the file to its most recent state.

To view the archive bit in Windows, right-click the file, choose Properties, and click the Advanced button on the General tab. You will see the option File Is Ready for Archiving, as shown in Figure 18-1.

# FIGURE 18-1

Looking at the archive bit in Windows

![](bbox=[0.37, 0.58, 0.8, 0.84])