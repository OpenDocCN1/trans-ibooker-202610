# Configuring Storage Replica

Now that our servers are prepped and tested, it's time to build the real deal! Since we have met all of our prerequisites and built out servers with SR in mind, all it takes is one PowerShell command to get this Storage Replica partnership started. I will list the command below, which is pretty self-explanatory. The only piece that may be slightly unfamiliar is SourceRGName and DestinationRGName. The "RG" stands for "Replication Group," which is simply a name you must define for the replication group on each server:

```
```
New-SRPartnership -SourceComputerName FS01 -SourceRGName RepGroup1
-SourceVolumeName S: -SourceLogVolumeName L: -DestinationComputerName
FS02 -DestinationRGName RepGroup2 -DestinationVolumeName S:
-DestinationLogVolumeName L:
```
```

Boom! Just like that, Storage Replica is configured with FS01's S: drive being primary storage, automatically and synchronously replicated to FS02's S: drive. Depending on performance and networking specs, the initial sync between volumes can take a bit of time, during which you will see **ReplicationState = InitialBlockCopy** in the results of the Get-SRGroup command, as you can see in *Figure 12.28*. Once the initial sync has completed, that field will instead show **ReplicationState = ContinuouslyReplicating**. Any data that is stored on FS01's S: drive is automatically replicated over to FS02:

```
```
PS C:\Users\administrator.CONTOSO> get-srgroup

AllowVolumeResize     : False
AsyncRPO              :
ComputerName          : FS01
Description           :
Id                    : 6acd8be2-6638-4008-9c02-2f1dbfbe32e7
IsAutoFailover        :
IsCluster             : False
IsEncrypted           : False
IsInPartnership       : True
IsMounted             : False
IsPrimary             : True
IsSuspended           : False
IsWriteConsistency    : False
LastInSyncTime        :
LogSizeInBytes        : 8589934592
LogVolume             : L:\
Name                  : RepGroup1
NumOfReplicas         : 1
Partitions            : {d2d6afbc-3417-47c9-8421-7275dbf5eccb}
Replicas              : {MSFT_WvrReplica (PartitionId = "d2d6afbc-3417-47c9-8421-7275dbf5eccb")}
ReplicationMode       : Synchronous
ReplicationStatus     : InitialBlockCopy
TemporaryPath         :
PSComputerName        :
```
```

Figure 12.28: Details of our Storage Replica group