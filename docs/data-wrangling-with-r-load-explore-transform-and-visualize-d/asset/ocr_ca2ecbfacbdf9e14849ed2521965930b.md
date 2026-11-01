attached to the database, to allow traffic only from the IP address of your client machine. For production deployments, the database should be deployed in a private subnet and accessed through more secure mechanisms with additional setup.

Here, we will use an Amazon EC2 instance to connect with the RDS MySQL database and load the sample database from the aws-database-migration-samples Git repository (https://github.com/aws-samples/aws-database-migration-samples). We will use the ticket database and load it into RDS MySQL with the following steps:

+   1. Log in to the EC2 instance and install MySQL client and git. Execute the following commands to install git and MySQL on EC2:

```
sudo yum update #as needed
sudo yum install git
git version
sudo yum install mysql
mysql --version
```

The following screenshot shows the successful installation of git and MySQL on the EC2 instance:

```
Dependency Installed:
    git-core.x86_64 0:2.37.1-1.amzn2.0.1                git-core-doc.noarch 0:2.37.1-1.amzn2.0
    perl-Error.noarch 1:0.17020-2.amzn2                perl-Git.noarch 0:2.37.1-1.amzn2.0.1
    perl-TermReadKey.x86_64 0:2.30-20.amzn2.0.2

Complete!
[ec2-user@ip-10-0-30-62 ~]$ git version
git version 2.37.1
[ec2-user@ip-10-0-30-62 ~]$ sudo yum install mysql
Loaded plugins: extras_suggestions, langpacks, priorities, update-motd
Resolving Dependencies
--> Running transaction check
```

Figure 9.2: MySQL installation on the EC2 instance

+   2. Connect to the migration-samples Git repository and clone the code. Execute the following commands on the EC2 instance:

```
git clone https://github.com/aws-samples/aws-database-migration-samples.git
cd aws-database-migration-samples/mysql/sampledb/v1/
```

You can list the files under the cloned folder for MySQL to ensure the code is successfully copied.

```
[ec2-user@ip-10-0-30-62 ~]$ git clone https://github.com/aws-samples/aws-database-migration-sample
Cloning into 'aws-database-migration-samples'...
remote: Enumerating objects: 1015, done.
remote: Total 1015 (delta 0), reused 0 (delta 0), pack-reused 1015
Receiving objects: 100% (1015/1015), 2.58 MiB | 13.63 MiB/s, done.
Resolving deltas: 100% (449/449), done.
[ec2-user@ip-10-0-30-62 ~]$ ls
aws-database-migration-samples
[ec2-user@ip-10-0-30-62 ~]$ cd aws-database-migration-samples/mysql/sampledb/v1/
[ec2-user@ip-10-0-30-62 v1]$ ls
data  install-rds.sql  README.md  remove-sampledb.sql  schema  user
[ec2-user@ip-10-0-30-62 v1]$
```

Figure 9.3: Copying the sample data from GitHub to the EC2 instance

+   3. Connect to the MySQL database and ensure connectivity is working. Connect to the MySQL database using the following command and provide the password when prompted: