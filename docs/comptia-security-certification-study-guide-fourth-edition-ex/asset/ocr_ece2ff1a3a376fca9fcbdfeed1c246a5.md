bit after the backup is done. This means that if you were to do another differential backup the next time you backed up, the same files (and any new ones) would get backed up. As a result, every differential backup will have all changes since the last full backup. The order of restoration strategy would be to restore the last full backup and then to restore your last differential backup.

> exam
> Watch
> Differential backups back up any files that have had changes, but they do not clear the files’ archive bit. Because the archive bit is not cleared, each differential backup will back up all files changed since the last full backup.

Differential and incremental backups can make the restoration process a little more complex because you have to restore from the full backup first and then restore from the incremental or differential backups to make sure any files that have changed since the last full backup are restored. An important difference between differential and incremental backups is that incremental backups take less time to perform (because you are getting only changes since the last full or incremental backup) but more time to restore (because you are restoring multiple incremental backups). Differential backups take more time to perform but less time to restore.

If you decide to take the simpler but more time-intensive approach by performing full backups each time, you can restore from the most recent full backup and get all the files restored in one session.

> exam
> Watch
> You may see a question or two on the exam about the different types of backups. Be sure to know the differences between full, incremental, and differential backups and also be comfortable with the restoration order with each scenario.

# Copy Backup

A copy backup is like a full backup but it does not clear the archive bit. This is important because a copy backup will not disrupt the backup cycle if you run this type of backup. Copy backups are useful if you decide you need to back up the data in the middle of a backup cycle—for example, head office asks you to send a backup of all the data. You can run the copy backup knowing it will not mess with your regular backup strategy.