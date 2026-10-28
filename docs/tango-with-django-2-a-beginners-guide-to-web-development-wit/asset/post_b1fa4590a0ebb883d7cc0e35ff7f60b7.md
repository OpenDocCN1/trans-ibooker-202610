
Have Python installed on your computer. If you are using a Linux distribution or macOS, you will have it installed. Some of your operating system’s functionality is implemented in Python⁴⁷, hence the need for an interpreter! Unfortunately, most modern operating systems that come preloaded with Python use a version that is much older than what we require. Exceptions include Ubuntu, coming with version 3.6.5 which should be sufficient for your needs. If you do need to install 3.7, we must install this version of Python *side-by-side* with the old one.

There are many different ways in which you can install Python. We demonstrate here the most common approaches that you can use on Apple’s macOS, various Linux distributions and Windows 10. Pick the section associated with your operating system to proceed. Note that we favour the use of package managers⁴⁸ where appropriate to ensure that you have the means of maintaining and updating the software easily (when required).

## Apple macOS

The simplest way to acquire Python 3 for macOS is to download a .dmg image file from the official Python website⁴⁹. This will provide you with a step-by-step installation interface that makes setting everything up straightforward. If your development environment will be kept lightweight (i.e. Python only), this option makes sense. Simply download the installer, and Python 3 should then be available on your Mac’s terminal!

However, package managers make life easier⁵⁰ when development steps up and involves a greater number of software tools. Installing a package manager makes it easy to maintain and update the software on your computer – and even to install new software, too. macOS does not come preinstalled with a package manager, so you need to download and install one yourself. If you want to go down this route, we’ll introduce you to *MacPorts*, a superb package manager offering a large host of tools⁵¹ for you to download and use. We recommend that you follow this route. Although more complex, the result will be a complete development environment, ready for you to get coding and working on whatever project you (or your future self!) will be entrusted with.

⁴⁷ http://en.wikipedia.org/wiki/Yellowdog_Updater,_Modified
⁴⁸ https://en.wikipedia.org/wiki/Package_manager
⁴⁹ https://www.python.org/downloads/mac-osx/
⁵⁰ https://softwareengineering.stackexchange.com/questions/372444/why-prefer-a-package-manager-over-a-library-folder
⁵¹ https://www.macports.org/ports.php

A prerequisite for using MacPorts is that you have Apple’s *Xcode* environment installed. This download is several gigabytes in size. The easiest way to acquire this is through the App Store on your macOS installation. You’ll need your Apple account to download that software. Once Xcode has been installed, follow the following steps to set up MacPorts.

1. Verify that Xcode is installed by launching it. You should see a welcome screen. If you see this, everything is ready, so you can then quit the app.
2. Open a Terminal window. Install the Xcode command line tools by entering the command

   ```bash
   $ xcode-select --install
   ```

   This will download additional software tools that will be required by Xcode and additional development software that you later install.

3. Agree to the Xcode license, if you have not already. You can do this by entering the command

   ```bash
   $ sudo xcodebuild -license
   ```

   Read the terms to the bottom of the page, and type Y to complete – but only if you agree to the terms!

4. From [the MacPorts installation page](https://www.macports.org/install.php), download the MacPorts installer for your correct macOS version.
5. On your Mac’s Finder, open the directory where you downloaded the installer to, and double-click the file to launch the installation process.
6. Follow the steps, and provide your password to install the software.
7. Once complete, delete the installer file – you no longer require it. Close down any Terminal windows that are still open.

Once the MacPorts installation has been completed, installing Python is straightforward.

1. Open a new Terminal window. It is important that you launch a new window after MacPorts installation to ensure that all the changes the installer made come into effect!
2. Enter the following command.

   ```bash
   $ sudo port install python37
   ```

   After entering your password, this will take a few minutes. Several dependencies will be required – agree to these being installed by responding with Y.

3. Once installation completes, activate your new Python installation. Enter the command

   ```bash
   $ sudo port select --set python python37
   ```

4. Test that the command succeeds by issuing the command `$ python`. You should then see the interpreter for Python 3.7.2 (or whatever version you just installed – as long as it starts with 3.7, this will be fine).

Once this has been completed, Python has been successfully installed and is ready to use. However, we still need to set up virtual environments to work with your installation.

1. Enter the following commands to install `virtualenv` and helper functions provided in the user-friendly wrapper.

   ```bash
   $ sudo port install py37-virtualenv
   $ sudo port install py37-virtualenvwrapper
   ```

2. Activate `py37-virtualenv` with the following command.

   ```bash
   $ sudo port select --set virtualenv virtualenv37
   ```

3. Edit your `~/.profile` file. This can be done with the *TextEdit* app in macOS by issuing the command `$ open ~/.profile`. Add the following four lines at the end of the file.

   ```bash
   export VIRTUALENVWRAPPER_PYTHON='/opt/local/bin/python3.7'
   export VIRTUALENVWRAPPER_VIRTUALENV='/opt/local/bin/virtualenv-3.7'
   export VIRTUALENVWRAPPER_VIRTUALENV_CLONE='/opt/local/bin/virtualenv-clone-3.7'
   source /opt/local/bin/virtualenvwrapper.sh-3.7
   ```

4. Save the file and close all open Terminals. After doing this, open a new Terminal. Everything should now be working and ready for you to use. Test with the following command. You should **not** see the `command not found` error.

   ```bash
   $ mkvirtualenv
   ```

### Installing Additional Software with MacPorts

MacPorts provides an extensive, preconfigured library of open-source software suited specifically for development environments. When you need something new, it’s a cinch to install. **For example**, you want to install the *LaTeX* typesetting system, search [the MacPorts ports list](https://www.macports.org/ports.php) — the resultant package name being `texlive-latex`. This could then be installed with the command `$ sudo port install texlive-latex`. All software that LaTeX is dependent upon is also installed. This saves you significant amounts of time trying to find all the right bits of software to make things work.

To view the packages MacPorts has already installed on your system, issue the command `$ port list installed`. You will see python37 listed!

## Linux Distributions

There are many different ways in which you can download, install and run an updated version of Python on your Linux distribution. Unfortunately, methodologies vary from distribution to distribution. To compound this, almost all distributions of Linux don’t have a precompiled version of Python 3.7 ready for you to download and start using (at the time of writing), although the latest release of Ubuntu uses Python 3.6 (which is sufficient).

If you do choose to install a new version of Python, we’ve put together a series of steps that you can follow. These will allow you to install Python 3.7.2 from scratch.

The steps have been tested thoroughly in Ubuntu 18.04 LTS; other distributions should also work with minor tweaks, especially concerning the package manager being used in step 1. A cursory search on your favourite search engine should reveal the correct command to enter. For example, on a *Red Hat Enterprise Linux* installation, the system package manager is yum instead of apt.

### Assumption of Knowledge

To complete these steps, we assume you know the basics for Bash interaction, including what the tilde (~) means, for example. Be careful with the sudo command, and do not execute it except for the steps we list requiring it below.

1. Install the required packages for Python to be built successfully. These are listed below, line by line. These can be entered into an Ubuntu Terminal as-is; slight modifications will be required for other Linux distributions.

   ```bash
   $ sudo apt install wget
   $ sudo apt install build-essential
   $ sudo apt install libssl-dev
   $ sudo apt install libffi-dev
   $ sudo apt install python-dev
   $ sudo apt install zlib1g-dev
   $ sudo apt install libbz2-dev
   $ sudo apt install libreadline-dev
   $ sudo apt install libsqlite3-dev
   ```

2. Once the above packages are installed, download the source for Python 3.7.2. We create a pytemp directory to download the file to. We’ll delete this once everything has completed.

   ```bash
   $ mkdir ~/pytemp
   $ cd ~/pytemp
   $ wget https://www.python.org/ftp/python/3.7.2/Python-3.7.2.tgz
   ```

3. Extract the .tgz file.
