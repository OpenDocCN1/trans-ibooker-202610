
## P

*   enforcing IAM user password policies in AWS account, [Enforcing IAM User Password Policies in Your AWS Account](#ch01.html#ix_pswdpol)\-[Challenge](#ch01.html#idm46022860395136)
*   storing, encrypting, and accessing with Secrets Manager, [Problem](#ch01.html#ix_pwdstoencrypt)\-[Challenge](#ch01.html#idm46022859939248)
*   peering VPCs for inter-VPC network communication, [Problem](#ch02.html#ix_peerVPC)\-[Challenge](#ch02.html#idm46022858886208)
*   permissions
    *   locking down for SSM users, [Discussion](#ch01.html#idm46022860094240)
    *   permission sets, [Discussion](#ch09.html#idm46022854750544)
*   permissions boundaries, using to delegate IAM administrative capabilities, [Problem](#ch01.html#ix_permbdry)\-[Challenge](#ch01.html#idm46022860189664)
*   PHI (protected health information), categorizing for further analysis, [Problem](#ch08.html#ix_PHIcat)\-[Discussion](#ch08.html#idm46022855233264)
*   physician dictation analysis using Transcribe Medical and Comprehend Medical, [Problem](#ch08.html#ix_physdict)\-[Discussion](#ch08.html#idm46022855238768)
*   PII (personally identifiable information), redacting from text using Comprehend, [Problem](#ch08.html#ix_PIIredact)\-[Challenge](#ch08.html#idm46022855360736)
*   Policy Simulator (IAM), testing IAM policies with, [Problem](#ch01.html#ix_PolSim)\-[Challenge](#ch01.html#idm46022860327808)
*   Polly service, converting text to speech, [Problem](#ch08.html#ix_Polly)\-[Challenge](#ch08.html#idm46022855499120)
*   PostgreSQL
    *   Aurora Serverless database, allowing REST access using RDS Data API, [Problem](#ch04.html#ix_PostgreREST)\-[Challenge](#ch04.html#idm46022857332256)
    *   creating Amazon Aurora serverless PostgreSQL database, [Problem](#ch04.html#ix_Postgre)\-[Challenge](#ch04.html#idm46022858018704)
*   `PostgreSQL` package, installing, [Validation checks](#ch04.html#idm46022858072832)
*   `PowerUserAccess` IAM policy, [Solution](#ch01.html#idm46022860837392), [Discussion](#ch01.html#idm46022860779152)
    *   attaching to role, [Steps](#ch01.html#idm46022860811840)
*   prefix lists in security groups, managing CIDRs with, [Problem](#ch02.html#ix_prfxCIDR)\-[Challenge](#ch02.html#idm46022859128768)
*   principle of least privilege, [Discussion](#ch01.html#idm46022860558592), [Discussion](#ch01.html#idm46022860485536)
    *   (see also least privilege access)
*   privilege escalation, IAM service mitigating risk of, [Steps](#ch01.html#idm46022860828144)
*   producers and consumers (streaming data), [Discussion](#ch07.html#idm46022855948864)
*   Prometheus, Amazon Managed Service for, [Introduction](#ch06.html#idm46022856634896)
*   protected health information (PHI), categorizing for further analysis, [Problem](#ch08.html#ix_prothealth)\-[Discussion](#ch08.html#idm46022855231840)
*   provisioned capacity for DynamoDB table, autoscaling, [Problem](#ch04.html#ix_provcapDynDB)\-[Challenge](#ch04.html#idm46022857503424)
*   provisioned concurrency, reducing Lambda startup times with, [Problem](#ch05.html#ix_provconc)\-[Challenge 2](#ch05.html#idm46022856740112)
*   `Proxy ID`, [Steps](#ch04.html#idm46022857854960)
*   public access, blocking for S3 bucket, [Problem](#ch01.html#ix_pubacc)\-[Challenge](#ch01.html#idm46022859875872)

## Q

*   Query Editor in RDS Console, [Steps](#ch04.html#idm46022857376048), [Discussion](#ch04.html#idm46022857335408)
*   quotas and limits (Kinesis service), [Discussion](#ch07.html#idm46022855934848)

## R

*   RDS Data API
    *   short-lived database connections, [Discussion](#ch04.html#idm46022857796016)
    *   using to enable REST access to Aurora Serverless, [Problem](#ch04.html#ix_RDSData)\-[Challenge](#ch04.html#idm46022857334048)
*   `rds-data:CommitTransaction` permission, [Discussion](#ch04.html#idm46022857337792)
*   `rds-data:RollbackTransaction` permission, [Discussion](#ch04.html#idm46022857340352)
*   Reachability Analyzer (VPC), using to verify and troubleshoot network paths, [Problem](#ch02.html#ix_ReachAnal)\-[Challenge](#ch02.html#idm46022859326896)
*   `ReadCapacityUnits` scaling target, [Steps](#ch04.html#idm46022857541664)
*   recovery point (backup), [Steps](#ch03.html#idm46022858373648)
*   recovery point objectives, replicating S3 buckets to meet, [Problem](#ch03.html#ix_recptobj)\-[Challenge 2](#ch03.html#idm46022858656912)
*   recovery time objective (RTO), decreasing for EC2 instance using AWS Backups, [Discussion](#ch03.html#idm46022858333376)
*   redirect response for all HTTP traffic to HTTPS, [Steps](#ch02.html#idm46022859236880)
*   `Redis` Python package, installing, [Steps](#ch05.html#idm46022856707536)
*   `Redis`, Lambda function with `Redis` client, accessing VPC resources, [Solution](#ch05.html#ix_Redis)\-[Challenge 1](#ch05.html#idm46022856679792)
*   Regions
    *   creating and restoring EC2 backups to another Region using AWS Backup, [Problem](#ch03.html#ix_Regbkup)\-[Challenge](#ch03.html#idm46022858320976)
    *   creating VPC in, spreading subnets across Availability Zones, [Discussion](#ch02.html#idm46022859645824)
    *   listing all, [Fast Fixes](#app01.html#idm46022854670944)
    *   listing all that have a particular service, [Fast Fixes](#app01.html#idm46022854662480)
    *   listing Region names and endpoints in table, [Fast Fixes](#app01.html#idm46022854692448)
    *   setting by grabbing value from EC2 instance metadata, [Fast Fixes](#app01.html#idm46022854652688)
    *   stopping all running instances for current working Region, [Fast Fixes](#app01.html#idm46022854702992)
    *   for VPCs, [Discussion](#ch02.html#idm46022859729728)
*   Rekognition Video, using to detect text in a video, [Problem](#ch08.html#ix_Rekog)
*   Relational Database Service (RDS), [Introduction](#ch04.html#idm46022858162240)
    *   automating password rotation for RDS databases, [Problem](#ch04.html#ix_RDSpswdauto)\-[Challenge](#ch04.html#idm46022857573568)
    *   deleting RDS cluster, [Cleanup](#ch04.html#idm46022858055392)
    *   leveraging RDS Proxy for database connections from Lambda, [Problem](#ch04.html#ix_RDSProxy)\-[Challenge](#ch04.html#idm46022857787648)
    *   migrating databases to, using DMS, [Problem](#ch04.html#ix_RDSmigrDB)\-[Challenge](#ch04.html#idm46022857416560)
    *   naming constraints, [Steps](#ch04.html#idm46022858124912)
    *   using IAM authentication with RDS database, [Problem](#ch04.html#ix_RDSDBauth)\-[Challenge](#ch04.html#idm46022857908656)
*   replication
    *   Aurora Serverless databases, [Discussion](#ch04.html#idm46022858028352)
    *   DMS replication tasks, [Solution](#ch04.html#ix_replDMS)\-[Challenge](#ch04.html#idm46022857418368)
    *   replicating data between EFS and S3 with DataSync, [Problem](#ch03.html#ix_replEFSS3)\-[Challenge 2](#ch03.html#idm46022858175440)
    *   replicating S3 buckets to meet recovery point objectives, [Problem](#ch03.html#ix_replS3)\-[Challenge 2](#ch03.html#idm46022858655200)
*   `ReplicationStatus`, [Validation checks](#ch03.html#idm46022858685984)
*   Resource Access Manager, [Discussion](#ch09.html#idm46022854755152)
*   REST
    *   enabling REST access to Aurora Serverless using RDS Data API, [Problem](#ch04.html#ix_RESTaccAurSrvl)\-[Challenge](#ch04.html#idm46022857325264)
    *   REST API exposed by RDS, [Discussion](#ch04.html#idm46022857802544)
*   restores
    *   restoring a file from an EBS snapshot, [Problem](#ch03.html#ix_resfileEBS)
    *   restoring EC2 backups to another Region using AWS Backup, [Steps](#ch03.html#idm46022858361856)
*   roles
    *   creating, [Steps](#ch01.html#idm46022860815216)
    *   retrieving for a user, [Steps](#ch01.html#idm46022860822528)
*   root user
    *   email alerts for root login, setting up, [Problem](#ch09.html#ix_rootlgin)\-[Challenge](#ch09.html#idm46022854903120)
    *   setting up multi-factor authentication for, [Problem](#ch09.html#ix_rootMFauth)\-[Challenge](#ch09.html#idm46022854834096)
