# iSCSI

As noted earlier, NFS shares files, and iSCSI shares block devices. This means that a VMFS will need to be put on the iSCSI target after it has been shared with an ESXi system. There are different reasons why people choose to use either NFS or iSCSI for storage, and it would take a rather long time to discuss the advantages of one over the other. As a matter of principle, however, vSphere can use either NFS or iSCSI, and many systems use both simultaneously.

Before diving in, here are some quick definitions of some key iSCSI concepts shown in Figure 9-9. An *iSCSI target* is the block device to be shared, an *iSCSI initiator* is the client that attaches to the target, and the *iSCSI Qualified Name (IQN)* is the name that’s used to identify both targets and initiators.

![](bbox=[0.134, 0.472, 0.908, 0.744])

In this section, you will learn how to create an iSCSI target on the Ubuntu 20.04 system running on Player.