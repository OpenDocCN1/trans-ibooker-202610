# Handling issues with Composer

One common issue developers face after the migration to PHP 8 has concluded is with third-party software. In this section, we discuss potential issues surrounding the use of the popular Composer package manager for PHP.

The first issue you might encounter has to do with versions of Composer itself. In the year 2020, Composer version 2 was released. Not all of the 300,000+ packages residing on the main packaging website (https://packagist.org/) have been updated to version 2, however. Accordingly, in order to install a given package, you might find yourself having to switch between Composer 2 and Composer 1. The latest releases of each version are available here:

+   - Version 1: https://getcomposer.org/download/latest-1.x/composer.phar
+   - Version 2: https://getcomposer.org/download/latest-2.x/composer.phar

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
Extracting archiveCreated project in /srv/api-tools-skeleton
Loading composer repositories with package information
```