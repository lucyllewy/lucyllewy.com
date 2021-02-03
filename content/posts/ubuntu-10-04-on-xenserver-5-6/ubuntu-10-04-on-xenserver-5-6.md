---
title: "Ubuntu 10.04 on XenServer 5.6"
date: 2010-06-29 03:12:05
published: true
categories: [sysadmin]
wpid: 696
---

These instructions are taken from [here](https://www.aikidokatech.com/index.php?option=com_content&view=article&id=65:ubuntu1004bxs56&catid=37:xen) and updated and formatted a bit by me. They assume that you are installing the *server* version of Ubuntu 10.04 and the release version of [Citrix](https://www.citrix.com) [XenServer](https://xenserver.org/) 5.6.

Install Ubuntu 10.04 onto XenServer 5.6

1. **Create a VM**
  First create a [VM](https://en.wikipedia.org/wiki/Virtual_machine) in XenCenter using the other install media template.
2. **Install Ubuntu**
  Install [ubuntu](https://www.ubuntu.com/) to your liking and reboot into the new system.
3. **Get GeTTY running on hvc0**
  Tell getty to display on *hvc0* (This does do the trick of getting a *display* on the console, but I am still unable to login or get any key-presses to work *at all*. I have to login via ssh to manage the system.)  
  `sudo cp /etc/init/tty1.conf /etc/init/hvc0.conf`
4. **Edit it to replace "tty1" with "hvc0"**
  `sudo nano -w /etc/init/hvc0`
5. **Create a link in /boot to itself**
  `sudo ln -s . /boot/boot`
6. **Install openssh-server**
  `sudo apt-get update sudo apt-get install openssh-server`
7. **Shutdown the VM**
8. **Retrieve the UUID for the VM**
  Assuming your VM is named "ubuntu10"  
  `xe vm-list name-label=ubuntu10 params=uuid`
9. **Retrieve the VBD UUID** This is the UUID for the storage attached to the VM  
  `xe vm-disk-list uuid=<UUID-from-step8>`
10. **Clear out the HVM boot policy**
  `xe vm-param-set uuid=<UUID-from-step8> HVM-boot-policy=`
11. **Set the PV [bootloader](https://en.wikipedia.org/wiki/Booting) to pygrub**
  `xe vm-param-set uuid=<UUID-from-step8> PV-bootloader=pygrub`
12. **Lastly, set the VBD for the VM to be bootable**
  `xe vbd-param-set uuid=<VBD UUID-from-step9> bootable=true`
13. **Start the VM and login**
  If your console fails to appear, try connecting to the VM using SSH. If you missed the step about getting GeTTY to output to hvc0, the login prompt will never appear.
14. **Attach the xs-tools.iso to the VM and mount the image**sudo mount /dev/cdrom1 /mnt
15. **Install the XenServer tools**
  Make sure you use the proper file for your architecture ([amd64](https://en.wikipedia.org/wiki/X86-64) or i386). A quick "ls" will confirm the version of the utilities in your xs-tools.iso. File names may (will) differ.  
  `sudo dpkg -i /mnt/Linux/xe-guest-utilities_5.5.901-562_i386.deb`
16. **Unmount the iso image and then detach it in XenCenter**
  `sudo umount /mnt`
17. **Reboot the VM**

You are now up and running. Update the system. The original author reports issues like XenCenter not displaying the NIC IP and no performance data.