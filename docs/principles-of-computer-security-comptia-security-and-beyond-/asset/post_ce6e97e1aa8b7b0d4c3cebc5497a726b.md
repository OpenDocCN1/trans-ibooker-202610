
**FIGURE 18-6 AWS services**

You’ll see a large selection of services in the following categories:
Compute, Containers, Storage, Database, Migration & Transfer,
Networking & Content Delivery, Developer Tools, Customer
Enablement, Robotics, Blockchain, Satellite, Quantum Technologies,
Management & Governance, Media Services, Machine Learning,
Analytics, Security, Identity, & Compliance, AWS Cost
Management, Front-end Web & Mobile, AR & VR, Application
Integration, Customer Engagement, Business Applications, End User
Computing, and Internet of Things.

d. Pick any five services from five different categories and explain why using them through AWS is advantageous.

## Lab Analysis

1. What is cloud computing and why is it so important today?

2. What are some similarities and differences between Azure and AWS?

3. What’s the problem with saying “There is no cloud. It’s just someone else’s computer”?

## Key Term Quiz

Use the terms from the list to complete the sentences that follow.

- IaaS
- PaaS
- SaaS

https://owasp.org/www-project-top-ten/. The number one item on this list at the time of this book’s publication was injection, which is the focus of this chapter.

30 MINUTES

## Lab Exercise 19.01: Configuring WampServer and DVWA

WampServer is a web development platform. WAMP contains everything you need to create dynamic web applications:

- W: Windows operating system (This indicates what operating system it runs on, not that it comes with it.)
- A: Apache HTTP Server web server
- M: MySQL and MariaDB relational database management systems (RDBMS)
- P: PHP server-side scripting language

WampServer is free and published under the GNU General Public License (GPL). Variants include LAMP for Linux and MAMP for macOS.

Here’s a description of DVWA from www.dvwa.co.uk/:

> Damn Vulnerable Web App (DVWA) is a PHP/MySQL web application that is damn vulnerable. Its main goals are to be an aid for security professionals to test their skills and tools in a legal environment, help web developers better understand the processes of securing web applications and aid teachers/students to teach/learn web application security in a classroom environment.

### Learning Objectives

In this lab exercise, you’ll download, install, and configure WampServer and

Packages (Visual C++ 2008 SP1), you should use Google to search for the new official Microsoft links. Alternatively, follow the instructions at the very end of the setup screen (from “If you have a 64-bit Windows...”), as shown in Figure 19-2.

![](img/6e921d997751f0a0f87e5170ac9657a1_677_0.png)

**FIGURE 19-2 WampServer installation information**

### Step 3 Complete the installation.

- a. Back in the Wampserver installer, after having ensured that all the required elements are in place, click the Next > button. Keep the default destination location and click the Next > button. Click the Install button on the Ready To Install screen. Then you will be presented with a question, “Do you want to choose another Browser installed on your system?”

  If you don’t have the Google Chrome browser or the Mozilla Firefox browser, download and install one or both now:

  - Google Chrome: www.google.com/chrome/
  - Mozilla Firefox: www.mozilla.org/en-US/firefox/new/

- b. Back in the Wampserver installer, in response to the browser question, click the Yes button and browse to `C:\Program Files (x86)\Google\Chrome\Application` or `C:\Program Files\Google\Chrome\Application` (your Chrome executable will be stored in one of these two locations) and then click chrome.exe to select Chrome. Alternatively, browse to `C:\Program Files\Mozilla Firefox` and then click firefox.exe to select Firefox.

- c. When prompted, “Do you want to choose another text editor installed on your system?” click the No button. Read the information on the next screen and click the Next > button. Then click the Finish button on the final screen of the installer.

### Step 4 Start WampServer.

- a. Launch Wampserver64 via a shortcut on the desktop or by clicking the Start button or in the search box, typing Wampserver64, and selecting Wampserver64.

- b. You’ll see the WampServer icon in the notification area on the taskbar (if you don’t see the WampServer icon, click the arrow to expand the icons list). It should turn green, which means you’re good to go! Click it to open the program menu, as shown in Figure 19-3.

the title bar; MariaDB is a fork of MySQL), type in and press ENTER for each of the lines that follow (as shown in Figure 19-6), to create the dvwa database, create the dvwa user, grant all privileges to the dvwa user, and flush (reload) the privileges from the grant tables. These lines, as well as the following paragraph, come directly from the README.md file in the dvwa folder.

![](img/6e921d997751f0a0f87e5170ac9657a1_682_0.png)

**FIGURE 19-6 Setting up the database, the user, and user privileges**

Note, if you are using MariaDB rather than MySQL (MariaDB is default in Kali Linux), then you can’t use the database root user, you must create a new database user. To do this, connect to the database as the root user and then use the following commands:

```
create database dvwa;
create user dvwa@localhost identified by 'YES';
grant all on dvwa.* to dvwa@localhost;
flush privileges;
```

### Step 9 Change a password in the configuration file.

- a. Right-click the config.inc.php file from Step 6d, select Open With, and click More apps. Scroll down and select Notepad. The configuration file should open up in Notepad.

![](img/6e921d997751f0a0f87e5170ac9657a1_684_0.png)

**FIGURE 19-8 DVWA login page**

### Step 11 Provide the following default credentials and click the Login button:

**Username:** admin

**Password:** password

Websites that are vulnerable to SQL injection are easy to spot: In any text input form field, type a single quote and submit it. If a SQL error message is returned, that site is vulnerable to what you’re about to do!

## Learning Objectives

In this lab exercise, you’ll perform SQL injection attacks. At the end of this lab exercise, you’ll be able to

- Understand how SQL injection attacks work
- Perform SQL injection attacks
- Make recommendations to remediate potential SQL injection attacks

## Lab Materials and Setup

The materials you need for this lab are

- The *Principles of Computer Security: CompTIA Security+ and Beyond* textbook
- A fully completed Lab Exercise 19.01, with all the necessary programs and files in place
- A web browser with an Internet connection

## Let’s Do This!

Note that you need to complete Lab Exercise 19.01 fully before attempting this one. If you’re continuing from Lab Exercise 19.01, you’re all set. If you took a break or just want to re-create the initial steps, perform the following:

1. Start WampServer.
2. Open the MariaDB console from the WampServer menu, and log in with a blank password.
3. Go to http://127.0.0.1/dvwa/login.php.
4. Log in with Username: **admin** and Password: **password**.
5. Click the DVWA Security menu item, change the Impossible dropdown setting to Low, and click the Submit button.
6. Click the SQL Injection menu item.

![](img/6e921d997751f0a0f87e5170ac9657a1_687_0.png)

### Step 1

SQL is not case sensitive, but it’s considered a best practice to uppercase the keywords. (Although in the four commands [Figure 19-6] taken from the README.md file, everything was lowercase.)

In the MariaDB console, type in the commands listed next and shown in Figure 19-9. I’ve included a link to a description of each command and a short description of what each command does.

```
c:\wamp64\bin\mariadb\mariadb10.4.10\bin\mysql.exe
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 30
Server version: 10.4.10-MariaDB mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```

```
MariaDB [(none)]> SHOW DATABASES;
```

| Database           |
|--------------------|
| dvwa               |
| information_schema |
| mysql              |
| performance_schema |
| test               |

5 rows in set (0.001 sec)

```
MariaDB [(none)]> USE dvwa;
Database changed
MariaDB [dvwa]> SHOW TABLES;
```

| Tables_in_dvwa |
|----------------|
| guestbook      |
| users          |

2 rows in set (0.001 sec)

```
MariaDB [dvwa]> DESCRIBE users;
```

| Field        | Type          | Null | Key | Default           | Extra                      |
|--------------|---------------|------|-----|-------------------|----------------------------|
| user_id      | int(6)        | NO   | PRI | NULL              |                            |
| first_name   | varchar(15)   | YES  |     | NULL              |                            |
| last_name    | varchar(15)   | YES  |     | NULL              |                            |
| user         | varchar(15)   | YES  |     | NULL              |                            |
| password     | varchar(32)   | YES  |     | NULL              |                            |
| avatar       | varchar(70)   | YES  |     | NULL              |                            |
| last_login   | timestamp     | NO   |     | current_timestamp() | on update current_timestamp() |
| failed_login | int(3)        | YES  |     | NULL              |                            |

8 rows in set (0.045 sec)

**FIGURE 19-9 Examining the users table in the dvwa database**
