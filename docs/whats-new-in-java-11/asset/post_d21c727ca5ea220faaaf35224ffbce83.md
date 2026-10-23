
# What's New in Java 11?

A Quick-Start Reference to the Long-Term Support Release

Benjamin Muschko

![](img/2331c4028acf1b9f21065457e9b2d8d4_1_0.png)

Beijing · Boston · Farnham · Sebastopol · Tokyo

# What’s New in Java 11?

by Benjamin Muschko

Copyright © 2019 O’Reilly Media, Inc. All rights reserved.

Printed in the United States of America.

Published by O’Reilly Media, Inc., 1005 Gravenstein Highway North, Sebastopol, CA 95472.

O’Reilly books may be purchased for educational, business, or sales promotional use. Online editions are also available for most titles (http://oreilly.com/safari). For more information, contact our corporate/institutional sales department: 800-998-9938 or corporate@oreilly.com.

Acquisitions Editor: Nancy Davis

Development Editor: Jeff Bleiel

Production Editor: Justin Billing

Copyeditor: Octal Publishing, LLC

Proofreader: Sharon Wilkey

Interior Designer: David Futato

Cover Designer: Karen Montgomery

Illustrator: Rebecca Demarest

December 2018: First Edition

# Revision History for the First Edition

- 2018-12-17: First Release

The O’Reilly logo is a registered trademark of O’Reilly Media, Inc.

What’s New in Java 11?, the cover image, and related trade dress are trademarks of O’Reilly Media, Inc.

The views expressed in this work are those of the author, and do not represent the publisher’s views. While the publisher and the author have used good faith efforts to ensure that the information and instructions contained in this work are accurate, the publisher and the author disclaim all responsibility for errors or omissions, including without limitation responsibility for damages resulting from the use of or reliance on this work. Use of the information and instructions contained in this work is at your own risk. If any code samples or other technology this work contains or describes is subject to open source licenses or the intellectual property rights of others, it is your responsibility to ensure that your use thereof complies with such licenses and/or rights.

978-1-492-04756-8

[LSI]

## What’s New in Java 11

## Introduction

For decades, Java has been one of the most commonly used programming languages in enterprise software, and its release train is moving faster than ever. Starting with Java 9, released in 2017, the release cadence has been changed to every six months. Java 10 added new features to the platform. For many users, however, the changes were not substantial enough to justify the migration effort to Java 10. But Java 11 goes beyond the typical new features and bug fixes.

As a Java developer, it’s difficult to keep up with the latest and greatest developments included in every single release. This report explains the benefits of upgrading to Java 11 and gives you a swift but condensed and insightful overview of its compelling features.

Migrating from an earlier version of Java might not be as straightforward as you would expect. Java 11 removes a variety of technologies and APIs from the platform. In this report, you learn about migration strategies that help you to get up to speed as quickly as possible. But, despite potential obstacles, the new features in Java 11 make upgrading worth it.

## Convergence of Oracle JDK and OpenJDK

OpenJDK, the open source counterpart of the Oracle JDK, has been around since Java 7. For the most part, language features, core Java libraries, and the tooling of both distributions are very similar, if not exactly the same. In addition to those aspects, Oracle JDK versions before Java 11 include commercial features like Flight Recorder, Java Mission Control, and the Z Garbage Collector that could be activated by passing in the `-XX:+UnlockCommercialFeatures` option when starting up a new JVM. With Java 11, commercial features become free of charge and have been added to OpenJDK. We discuss some of those features in a later section.

Oracle’s long-term goal was to converge both distributions by making the commercial tooling fully open source. With Java 11, this milestone has been reached along with some minor packaging and cosmetic differences. So, which distribution should you pick for your project, you might ask? Let’s have a look at the license implications for each option.

## Licensing of the Oracle JDK and OpenJDK

There’s a single, distinguishing factor between the Oracle JDK and OpenJDK: the license agreement. The OpenJDK continues to be licensed with the GNU General Public License, version 2 (GPL 2) with a classpath exception. This license allows you to use Java for open source projects as well as commercial products without any restrictions or attribution for development and production purposes. The classpath exception decouples the licenses terms of the OpenJDK from the licenses terms applicable to external libraries bundled with the executable program. Regarding the Oracle JDK, the license has changed from the Binary Code License (BCL) for Oracle Java SE technologies to a commercial license. As a result, you can no longer use the Oracle JDK for production without buying into the Java SE subscription model.

> **NOTE**
> 
> The subscription model also applies to any Java 8 patch release published after January 2019 if it is used for commercial products.

It’s important that you understand the license implications to make a choice that’s right for your organization. If you are working on mission-critical software and you’d like to rely on Oracle’s support for long-term patches, the commercial Oracle JDK license is likely the best option. For open source software and products that require only short-term patch releases for the next six months, the OpenJDK should fully suffice. Either way, you will need to decide which Java distribution to use as soon as you upgrade to Java 11. Be aware that there are alternative, free distributions of the OpenJDK from other vendors. For more information, see the Java Champions’ blog post “Java Is Still Free”.

## Six-Month Release Cadence: What It Means to You

In the early days of Java, new releases were a big deal. They used to happen only every two to three years and contained a lot of significant, sometimes breaking, changes like Generics in Java 5, or syntax changes in Java 8, or the Java 9 module system. Migrating a software project to a new Java version often required a lot of planning and effort. Some projects even had to postpone the software production process until the migration had been fully completed.

Many industry leaders moved to a Continuous Delivery model to drive innovation and mitigate risk: fewer changes, more frequent releases. Oracle is committed to following this example by releasing a new major version every six months, and a minor release every two months. As a result, upgrading to a new version of Java will have less of an impact on your project. Figure 1-1 shows the expected dates of the upcoming major and minor Java releases.

![](img/2331c4028acf1b9f21065457e9b2d8d4_6_0.png)

Figure 1-1. Java’s projected release cycle

With more frequent releases comes the responsibility of ensuring platform stability. For that very reason, Oracle provides a long-term support release every three years. Next, we have a look at what “long-term support” means in practice.

## Long-Term Support

Oracle offers lifetime Premier Support for all major Java versions that have been designated a long-term support (LTS) release. The benefit of an LTS release is that paying customers will receive periodic patch updates even after the next major version has been released. For example, the next LTS release after Java 11 will be Java 17, which is planned for September 2021. In the meantime, you will continue to receive new releases, as explained in the previous section. For any major Java version, OpenJDK users will receive only two more patch releases. After that, it is assumed that you will need to upgrade to the next major version.

NOTE

On October 30, 2018, Amazon announced free long-term support for OpenJDK 8 and OpenJDK 11 Java runtimes in Amazon Linux 2, a Linux server operating system from Amazon Web Services (AWS). On November 14, James Gosling announced Corretto, a production-ready, long-term support distribution of Java.

LTS releases are especially attractive for more conservative enterprise customers with a slower upgrade rate. You might be unsure whether you need a commercial or free license for your software projects. A safe bet is to start with one of the free OpenJDK-based distributions. If you
