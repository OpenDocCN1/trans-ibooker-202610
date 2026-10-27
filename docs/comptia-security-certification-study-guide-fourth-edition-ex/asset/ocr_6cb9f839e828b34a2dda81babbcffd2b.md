> exam Watch
> Full backups back up every file that is selected and then clear the archive bit.

The important thing to understand when a full backup is performed is that it backs up all the files you select (whether the archive bit is set or not), and then it clears the archive bit so that the operating system and applications know that the file has been backed up.

# Incremental Backup

An incremental backup backs up only the files that have changed or that were added since the last incremental or full backup. It does this by backing up only files that have the archive bit set (meaning the file needs to be backed up). This is different from a full backup in that a full backup will back up any file that is selected because a full backup does not read the archive bit to determine whether to back up the file—it simply backs up whatever you tell it to. An incremental backup will back up whatever files you have selected that have the archive bit set.

> exam Watch
> Incremental backups back up any files that have changed and then clear the archive bit so that the next backup will not back the file up (unless you do a full backup).

Using a combination of the full backup and incremental backup is highly effective and less time-consuming than running a full daily backup. A number of companies will set up a backup schedule that performs a full backup Friday night and then backs up only the changes each night through the week by performing an incremental backup. However, to restore, you will need the last full backup tape as well as every incremental backup tape made since the last full backup.

An incremental backup clears the archive bit to report that the file has been backed up. If you were to perform an additional incremental backup the next day, the same file would not get backed up (unless it was changed), because the archive bit would not be set. Your restore strategy (known as the order of restoration) with incremental backups is to restore the full backup and then restore each incremental backup since the full backup. This will ensure that you get the buildup of changes since the full backup.

# Differential Backup

A differential backup backs up the files that have changed or that were added since the last full backup by looking for any files that have the archive bit set. The differential backup is different from the incremental in that the differential does not clear the archive