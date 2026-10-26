
If you are using an older Linux version, you may get a CMake version that is lower than 3.12.1. Then, you need to install it locally. Use the following commands:

```
wget \
https://github.com/Kitware/CMake/releases/download/v3.15.1/cmake-3.15.1-Linux-x86_64.sh

sh cmake-3.15.1-Linux-x86_64.sh
```

When you see the software license, type y and press Enter. When asked about the installation location, type y and press Enter again. This should install it to a new folder in your system.

Now, we will add that folder to our path. Type the following. Note that the first line is a bit too long and the line breaks in this document. You should write it as a single line, as follows:

```
echo "export PATH=\"$HOME/cmake-3.15.1-Linux-x86_64/bin:$PATH\"" >> .bash_profile

source .profile
```

Now, when you type the following:

```
cmake -version
```

You should see the following output:

```
cmake version 3.15.1

CMake suite maintained and supported by Kitware (kitware.com/cmake).
```

3.15.1 is the current latest release at the time of writing this document. Since it is newer than 3.12.1, this will suffice for our purposes.

## Installing Git

Test the current installation by typing the following:

```
git --version
```

You should see a line such as the following:

```
git version 2.17.1
```

If you see the following line instead, you need to install git:

```
command 'git' not found
```

Here is how you can install **git** in Ubuntu:

```
sudo apt install git
```

## Installing g++

Test the current installation by typing the following:

```
g++ --version
```

You should see an output such as the following:

```
g++ (Ubuntu 7.4.0-1ubuntu1~18.04) 7.4.0

Copyright (C) 2017 Free Software Foundation, Inc.

This is free software; see the source for copying conditions. There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

If it is not installed, type the following code to install it:

```
sudo apt install g++
```

## Installing Ninja

Test the current installation by typing the following:

```
ninja --version
```

You should see an output such as the following:

```
1.8.2
```

If it is not installed, type the following code to install it:

```
sudo apt install ninja-build
```

## Installing Eclipse CDT and cmake4eclipse

There are multiple ways of installing Eclipse CDT. To get the latest stable version, we will use the official installer. Go to this website and download the Linux installer: https://www.eclipse.org/downloads/packages/installer.

Follow the instructions there and install **Eclipse IDE for C/C++ Developers**. Once you have installed it, run the Eclipse executable. If you did not change the default configuration, typing the following command in the terminal will run it:

```
~/eclipse/cpp-2019-03/eclipse/eclipse
```

You will choose a workspace folder and then you will be greeted with a **Welcome** tab in the main Eclipse window.

Now, we will install **cmake4eclipse**. An easy way to do this is to go to this website and drag the **Install** icon to the Eclipse window: [https://github.com/15knots/cmake4eclipse#installation](https://github.com/15knots/cmake4eclipse#installation). It will ask you to restart Eclipse, after which you are ready to modify the CMake project to work with Eclipse.

## Installing GoogleTest

We will install **GoogleTest** in our system, which will also install other packages that are dependent on it. Write the following command:

```
sudo apt install libgtest-dev google-mock
```

This command installs the include files and source files for **GoogleTest**. Now, we need to build the source files that we installed to create the **GoogleTest** library. Run the following commands to do this:

```
cd /usr/src/gtest
sudo cmake CMakeLists.txt
sudo make
sudo cp *.a /usr/lib
```

## Installing the Code Bundle

Copy the code bundle for the class to the `C:/Code` folder.

## Additional Resources

The code bundle for this book is also hosted on GitHub at [https://github.com/TrainingByPackt/Advanced-CPlusPlus](https://github.com/TrainingByPackt/Advanced-CPlusPlus).

We also have other code bundles from our rich catalog of books and videos available at [https://github.com/PacktPublishing/](https://github.com/PacktPublishing/). Check them out!

## Introduction

C++ is one of the oldest and most popular languages that you can use to write efficient code. It is both "close to the metal," like C, and has advanced object-oriented features, like Java. Being an efficient low-level language makes C++ the language of choice for domains in which efficiency is paramount, such as games, simulations, and embedded systems. At the same time, being an object-oriented language with advanced features such as generics, references, and countless others makes it suitable for large projects that are developed and maintained by multiple people.

Almost any programming experience involves organizing your code base and using libraries written by others. C++ is no different. Unless your program is simple, you will distribute your code into multiple files that you need to organize, and you will use various libraries that fulfill tasks, usually in a much more efficient and robust way than your code would. C++ projects that do not use any third-party libraries are edge cases that do not represent the majority of projects, which use many libraries. These projects and their libraries are expected to work in different hardware architectures and operating systems. Therefore, it is important to spend time on project setup and understand the tools used to manage dependencies if you are going to develop anything meaningful with C++.

Most modern and popular high-level languages have standard tools to maintain projects, build them, and handle their library dependencies. Many of these have repositories that host libraries and tools that automatically download and use libraries from those repositories. For example, Python has **pip**, which takes care of downloading and using appropriate versions of libraries that the programmer wants to use. Similarly, JavaScript has **npm**, Java has **maven**, Dart has **pub**, and C# has **NuGet**. In most of these languages, you list the name of the library and the version that you would like to use, and the tool automatically downloads and uses the compatible version of the library. These languages benefit from the fact that the programs are built and run in a controlled environment in which a certain level of hardware and software requirements are satisfied. C++, on the other hand, is expected to work in a variety of contexts with different architectures, including very primitive hardware. Hence, C++ programmers are less pampered when it comes to building programs and performing dependency management.

## Managing C++ Projects

In the world of C++, we have several tools that help in managing project sources and their dependencies. For example, **pkg-config, Autotools, make,** and **CMake** are the most notable ones in the community. Compared to the tools of the other high-level languages, these are much more complicated to use. **CMake** has arisen among these as the de facto standard for managing C++ projects and their dependencies. It is more opinionated compared to **make**, and it is accepted as the direct project format for most IDEs (Integrated Development Environments).

While **CMake** helps with managing projects and their dependencies, the experience is still far from higher-level languages in which you list the libraries and their versions that you want to use and everything else is taken care of for you. With CMake, you still are responsible for installing libraries properly in your development environment, and you are expected to use compatible versions for each library. In popular Linux distributions with extensive package managers, you can easily install binary versions of most popular libraries. However, sometimes, you may have to compile and install the libraries yourself. This is a part of the whole C++ developer experience, which you will gather by learning more about the development platform of your choice. Here, we will focus more on how to properly set up our CMake projects, including understanding and resolving issues related to libraries.

## The Code-Build-Test-Run Loop

In order to base our discussion on a solid foundation, we will immediately start with a practical example. We will start with a C++ code base template that you can use as a starting point for your own projects. We will see how we can build and compile it using CMake on the command line. We will also set up the Eclipse IDE for C/C++ developers and import our CMake project. The use of an IDE will provide us with facilities that ease the creation of source code and enable us to debug our programs line by line to view what exactly happens during the execution of our program and correct our mistakes in an informed fashion rather than trial and error and superstition.
