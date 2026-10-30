
Fedora 34 (Server Edition)
Kernel 5.11.12-300.fc34.aarch64 on an aarch64 (tty1)

Web console: https://fedora:9090/ or https://10.0.0.234:9090/

Fedora login:

- 3. I logged into the web console as the user I created.

![](img/430161835ae267467687add4a1cdc429_172_0.png)

- 4. After logging into the web console, I was able to set the hostname by entering it in the **Real host name** text box.

![](img/430161835ae267467687add4a1cdc429_172_1.png)

5. In the logs, I noticed that SELinux was enabled.

![](img/430161835ae267467687add4a1cdc429_173_0.png)

6. SELinux is a powerful security tool, but I have seen it be overly restrictive. As I have a fairly secure environment, I opted to remove it. In the Applications tab, I selected it and clicked **Remove**.

![](img/430161835ae267467687add4a1cdc429_173_1.png)

7. In the upper right, I selected Shutdown from the drop-down menu. In the host client, I used Edit settings to change the VM to have one CPU and 2 GB RAM. This is needed as I plan on running multiple VMs on the ESXi server.

Fusion) or on one of the supported public clouds. It’s freely available, is easy to install on vSphere, and can be used as a base OS for your project. It is a little bit different to install and use than other Linux distributions.

There is a Raspberry Pi and full ISO Arm64 image. The Pi image is meant to run natively on a Pi; you will want to install the full Arm64 Photon OS 4.0 GA release.

The download site for the Arm images is https://github.com/vmware/photon/wiki/Downloading-Photon-OS

![](img/430161835ae267467687add4a1cdc429_175_0.png)

The following are the steps to install Photon OS on your Pi.

## Download Photon OS

We will be downloading the installation ISO directly to the ESXi datastore. Later we will use the ISO to install the system.

- 1. SSH into the ESXi system.
- 2. Gain root user privileges by entering:

  `sudo bash`

## CHAPTER 8　VIRTUAL MACHINES ON ESXI

- 3. Change to the directory where you want to store the ISO by entering:

  `cd /vmfs/volumes/rPi-USB-Datastore-01`

- 4. Open the firewall to allow the HTTP connections by entering:

  `esxcli network firewall ruleset set --enabled=true --ruleset-id=httpClient`

- 5. Download the Arm image by entering:

  `wget https://packages.vmware.com/photon/4.0/GA/iso/photon-4.0-1526e30ba-aarch64.iso`

![](img/430161835ae267467687add4a1cdc429_176_0.png)

It is 2.9 GB in size and will take a few minutes for the ISO to be downloaded.

You can also download the ISO to your desktop system and use SCP to copy it over to your ESXi host.

## Create a Photon VM

- 1. Log in to the ESXi Host Client.
- 2. In the Navigator, select **Virtual Machines** and then click **Create / Register VM**.
- 3. In the New virtual machine wizard, select Create a new virtual machine, and click Next.

![](img/430161835ae267467687add4a1cdc429_177_0.png)

- 4. In the Name text box, enter PhotonOS40. From the Guest OS family drop-down menu, select Linux. In the Guest OS version drop-down menu, select VMware Photon OS (64-bit) and click Next.

![](img/430161835ae267467687add4a1cdc429_177_1.png)

- 5. Verify that rPi-USB-Datastore-01 is the datastore, and click **Next**.
- 6. Set the virtual hardware to three CPUs, 4 GB of memory, and a 32 GB hard disk.
- 7. Expand **CD/DVD Drive 1**, select **Datastore ISO file**, and select your Photon OS ISO image. Verify that **Connect at power on** is selected.
- 8. Expand **Hard disk 1** and select **Thin provisioned**. Click **Next**.

## Customize settings

Configure the virtual machine hardware and virtual machine additional options.

![](img/430161835ae267467687add4a1cdc429_178_0.png)

- 9. Verify your settings and click **Finish**.

## Install Photon on the VM

I will be using VMRC, which was previously installed, to install the OS.

- 1. Right-click the Photon OS virtual machine and select **Launch remote console**.

![](img/430161835ae267467687add4a1cdc429_179_0.png)

- 2. At the top of the VMRC, click the **Power On** icon.
- 3. Accept the EULA.

## CHAPTER 8　VIRTUAL MACHINES ON ESXI

![](img/430161835ae267467687add4a1cdc429_180_0.png)

- 4. Follow the prompts to install the Photon Minimal OS.

![](img/430161835ae267467687add4a1cdc429_180_1.png)

- 5. Accept all the defaults.
- 6. It takes a few minutes to install the OS. It displays an elapsed timer during the install. It took me 96 seconds to install it.

![](img/430161835ae267467687add4a1cdc429_181_0.png)

- 7. When the install has completed, press any key to boot it.

## Configuring Photon

Once the OS has been installed, you will want to do some post-installation configuration.

- 1. The system will reboot with a splash screen. The OS does some configuration during this time, and so you will see the splash screen while it does the configuration (see Figure 8-3).

- 5. Restart the SSH service by entering:

```bash
systemctl restart sshd
```

- 6. Verify that the root user can log in using SSH:

```bash
ssh root@localhost
```

- 7. Log out of the SSH session by entering:

```bash
exit
```

- 8. Shut down the VM by entering:

```bash
init 0
```

- 9. Edit the settings to change it to one CPU and 2 GB RAM if you plan on running multiple instances of Photon on it. You can even use 1 GB of RAM.

## Installing VMware Tools on Photon

VMware Tools should be installed on all guest OSs if possible. Fortunately, you can install open-vm-tools as a package rather than having to build it.

- 1. Power on the VM and log in to it as root from VMRC or SSH.
- 2. Install the open-vm-tools package by entering:

```bash
tdnf -y install open-vm-tools
```

- 3. Enable and start VMware Tools by entering:

```bash
systemctl enable vmtoolsd.service
systemctl start vmtoolsd.service
```

- 4. Verify what services are started by entering:

  `systemctl status`

- 5. In the output, verify that vmtoolsd.service is shown.

![](img/430161835ae267467687add4a1cdc429_184_0.png)

- 6. In the host client, verify that VMware Tools is running.

![](img/430161835ae267467687add4a1cdc429_185_0.png)

Photon OS is an extremely lightweight OS. After I rebooted my VM, it only used 0.5 GB of RAM. It was nice that it had a package for the installation of VMware Tools. It is a little bit different to administer than other Linux distributions, but there are user and admin guides on the Photon OS website.

Fun fact: The package manager tdnf is an acronym for Tiny Dandified Yum!

## Snapshots

ESXi has the ability to take snapshots of VMs. This is a powerful technology as it allows you to preserve the state and data of a machine at a particular point in time. I find it useful to take a snapshot before doing a big change to a VM, like updating it or installing an application on it. If the update completes successfully, I commit the snapshot into the VM.

I have also found this feature useful when teaching classes. After walking my students through a process, I can revert the machine back to its original state and have the next class do the same exercise without having to reinstall the OS.

A VM can have multiple snapshots, and you can take snapshots regardless of whether a VM is powered on or off. If a snapshot is taken of an active VM, it will capture the current state of its RAM as well as its disks. Due to the resources required when taking a live snapshot, I take snapshots of VMs that have been powered down. There are many nuances with snapshots that we will not be covering here, but VMware has many excellent documents available to assist and answer questions on the topic.

## Working with Snapshots

Snapshots can be initiated from the ESXi Host Client.

- 1. Log on to the ESXi Host Client.
- 2. In the Navigator, double-click **Virtual Machines**.
- 3. Verify that the Fedora machine is powered off.
- 4. Right-click the Fedora VM and, from the **Snapshots** drop-down menu, select **Take snapshot**.

![](img/430161835ae267467687add4a1cdc429_186_0.png)
