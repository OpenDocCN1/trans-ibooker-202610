
## Testing and troubleshooting the migration

In an ideal world, the migration troubleshooting will take place on the staging server, or simulated virtual environment, well before the actual move to production. However, as the seasoned developer well knows, we need to hope for the best, but prepare for the worst! In this section, we cover additional aspects of testing and troubleshooting that can be easily overlooked.

For the purposes of this section, you can exit the temporary shell if you were following the Debian/Ubuntu or the Red Hat/CentOS/Fedora installation process. Return to the Docker container used for this course and open a command shell into the PHP 8 container. Please refer to the Technical requirements section of Chapter 1, Introducing New PHP 8 OOP Features, for more information on how to do this if you are unsure.

## Testing and troubleshooting tools

There are too many fine testing and troubleshooting tools available to document here, so we focus on a few open source tools to help with testing and troubleshooting.

## Working with Xdebug

Xdebug is a tool that provides diagnostics, profiling, tracing, and step-debugging, among other features. It's a PHP extension, and is thus able to give you detailed information in case you run into problems that you cannot easily solve. The main website is https://xdebug.org/.

To enable the Xdebug extension, you can install it just as you would any other PHP extension: using the pecl command, or by downloading and compiling the source code from https://pecl.php.net/package/xdebug.

Also, the following /etc/php.ini settings should be set, at a minimum:

```
zend_extension=xdebug.so
xdebug.log=/repo/xdebug.log
xdebug.log_level=7
xdebug.mode=develop,profile
```

Figure 11.2 shows the output using the xdebug_info() command called from /repo/ch11/php8_xdebug.php:

![](img/dbf2674d5b31107c344104b5edcb21c5_463_0.png)

Figure 11.2 – xdebug_info() output

Let's now have a look at another tool that checks your application from an outside perspective.

## Using Apache JMeter

An extremely useful open source tool for testing web applications is Apache JMeter (https://jmeter.apache.org/). It allows you to develop a series of test plans that simulate requests from a browser. You can simulate hundreds of user requests, each with their own cookies and session. Although mainly designed for HTTP or HTTPS, it's also capable of a dozen other protocols as well. In addition to an excellent graphical UI, it also has a command-line mode that makes it possible to incorporate JMeter in an automated deployment process.

Installation is quite simple, involving a single download from `https://jmeter.apache.org/download_jmeter.cgi`. You must have the **Java Virtual Machine (JVM)** installed before JMeter will run. Test plan execution is beyond the scope of this book, but the documentation is quite extensive. Also, please bear in mind that JMeter is designed to be run from a client, not on the server. Accordingly, if you wish to test the website in the Docker container for this book, you'll need to install Apache JMeter on your local computer, and then build a test plan that points to the Docker container. Normally the IP address for the PHP 8 container is `172.16.0.88`.

Figure 11.3 shows the opening screen for Apache JMeter running on a local computer:

![](img/dbf2674d5b31107c344104b5edcb21c5_464_0.png)

Figure 11.3 – Apache JMeter

From this screen you can develop one or more test plans, indicating the URL(s) to access, simulate GET and POST requests, set the number of users, and so forth.

> **Tip**
>
> If you encounter this error while trying to run jmeter: `Can't load library: /usr/lib/jvm/java-11-openjdk-amd64/lib/libawt_xawt.so`, try installing *OpenJDK 8*. You can then use the techniques mentioned in the earlier section to switch between versions of Java.

Let's now have a look at potential issues with Composer following a PHP 8 upgrade.

## Handling issues with Composer

One common issue developers face after the migration to PHP 8 has concluded is with third-party software. In this section, we discuss potential issues surrounding the use of the popular Composer package manager for PHP.

The first issue you might encounter has to do with versions of Composer itself. In the year 2020, Composer version 2 was released. Not all of the 300,000+ packages residing on the main packaging website (https://packagist.org/) have been updated to version 2, however. Accordingly, in order to install a given package, you might find yourself having to switch between Composer 2 and Composer 1. The latest releases of each version are available here:

- Version 1: https://getcomposer.org/download/latest-1.x/composer.phar
- Version 2: https://getcomposer.org/download/latest-2.x/composer.phar

Another, more serious, issue has to do with platform requirements of the various Composer packages you might be using. Each package has its own composer.json file, with its own requirements. In many cases, the package provider might add a PHP version requirement.

The problem is that while most Composer packages now work on PHP 7, the requirements were specified in such a manner as to exclude PHP 8. After a PHP 8 update, when you use Composer to update your third-party packages, an error occurs and the update fails. Ironically, most PHP 7 packages will also work on PHP 8!

As an example, we install a Composer project called laminas-api-tools. At the time of writing, although the package itself is ready for PHP 8, a number of its dependent packages are not. When running the command to install the API tools, the following error is encountered:

```
root@php8_tips_php8 [ /srv ]#
composer create-project laminas-api-tools/api-tools-skeleton
Creating a "laminas-api-tools/api-tools-skeleton" project at "./api-tools-skeleton"
Installing laminas-api-tools/api-tools-skeleton (1.3.1p1)
  - Downloading laminas-api-tools/api-tools-skeleton (1.3.1p1)
  - Installing laminas-api-tools/api-tools-skeleton (1.3.1p1):
Extracting archive
Created project in /srv/api-tools-skeleton
Loading composer repositories with package information
Updating dependencies
Your requirements could not be resolved to an installable set of packages.
    Problem 1
      - Root composer.json requires laminas/laminas-developer-tools dev-master, found laminas/laminas-developer-tools[dev-release-1.3, 0.0.1, 0.0.2, 1.0.0alpha1, ..., 1.3.x-dev, 2.0.0, ..., 2.2.x-dev] but it does not match the constraint.
    Problem 2
      - zendframework/zendframework 2.5.3 requires php ^5.5 || ^7.0 -> your php version (8.1.0-dev) does not satisfy that requirement.
```

The core problem, highlighted in the last portion of the output just shown, is that one of the dependent packages requires PHP ^7.0. In the composer.json file, this indicates a range of versions from PHP 7.0 through to and including PHP 8.0. In this particular example, the Docker container used runs PHP 8.1, so we have a problem.

Fortunately, in such cases, we are confident that if this package runs in PHP 8.0, it should also run in PHP 8.1. Accordingly, all we need to do is to add the --ignore-platform-reqs flag. When we retry the installation, as you can see from the following output, it is successful:

```
root@php8_tips_php8 [/srv]#
composer create-project --ignore-platform-reqs \
    laminas-api-tools/api-tools-skeleton
Creating a "laminas-api-tools/api-tools-skeleton" project at "./api-tools-skeleton"
Installing laminas-api-tools/api-tools-skeleton (1.6.0)
  - Downloading laminas-api-tools/api-tools-skeleton (1.6.0)
  - Installing laminas-api-tools/api-tools-skeleton (1.6.0):
Extracting archive
Created project in /srv/api-tools-skeleton
Installing dependencies from lock file (including require-dev)
Verifying lock file contents can be installed on current
platform.
Package operations: 109 installs, 0 updates, 0 removals
  - Downloading laminas/laminas-zendframework-bridge (1.3.0)
  - Downloading laminas-api-tools/api-tools-asset-manager (1.4.0)
```
