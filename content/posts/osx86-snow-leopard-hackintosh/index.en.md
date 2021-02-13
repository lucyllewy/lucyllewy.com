---
title: "OSX86 Snow Leopard Hackintosh"
date: 2010-02-18 23:26:53
published: true
modified: 2010-05-20 17:12:55
categories: [random]
wpid: 621
---

![Screenshot of Mac OS X Snow Leopard desktop](https://upload.wikimedia.org/wikipedia/en/8/82/Snow_Leopard_Desktop.png)

Image curtesy of WikiPedia ([https://en.wikipedia.org/wiki/File:Snow\_Leopard\_Desktop.png](https://en.wikipedia.org/wiki/File:Snow_Leopard_Desktop.png))

**Updated: 20/May/2010.**

As a much delayed followup to [my post](/hackintosh/) about getting OS X 10.5 Leopard running on a "Generic [Beige-Box PC](https://en.wikipedia.org/wiki/Beige_box)", I have finally discovered how to install and run [OS X 10.6](https://www.apple.com/macosx/) SNOW Leopard on the same system. The Mainboard is the MSI 975X Platinum PowerUp Edition, and I've got a [Core2Duo](https://en.wikipedia.org/wiki/Intel_Core_2) E6600 installed with 8GB of 800MHz DDR2 RAM.

I had numerous issues with this install from the outset, but perseverance prevailed. The most worrying problem I had was that in my initial attempts at booting the Retail Snow Leopard DVD it would hang at various points requiring a hard reset, at which time I was faced with a completely wiped CMOS/[BIOS](https://en.wikipedia.org/wiki/BIOS). I have now discovered that the issue is with my [Real-Time Clock](https://en.wikipedia.org/wiki/Real-time_clock) settings in my BIOS DSDT table, so I had to boot into my working install of 10.5 Leopard and run a DSDT patcher to get a "dsdt.aml" file that I could use with the Chameleon Boot-loader to override what my BIOS was reporting.

After patching the DSDT with the RTC fix, I could now boot into snow leopard without worrying that my BIOS would be reset. Next up was to disable the AppleIntelCPUPowerManagement.kext distributed with Snow. This is easy, and just requires NullCPUPowerManagement.kext to be placed into your /Extra folder for chameleon to pick up.

Onto determining why I was getting kernel panics caused by IOATAFamily.kext. After trawling insanelymac for a few hours I found that it was due to my mainboard only having one IDE Channel powered by the Intel chipset (with the other channel powered by [JMicron](https://en.wikipedia.org/wiki/JMicron)). It was a known issue on notebooks, but I figured it was the same issue I was having anyway and downloaded the replacement IOATAFamily.kext. This kext cannot be put into Chameleon's /Extra directory, though, as the [Apple](https://www.apple.com) one will always override it. So I had to remove the original kext from /System/Library/Extensions from the install DVD first, and then replace it with the hacked version.

The final problem with getting the system to boot was figuring out why LoginWindow.app was crashing, or more to the point why fontd was causing loginwindow to crash. I found a brief post from someone referring to a beta build of snow leopard stating that the encryption had changed for some of the binaries, and that dsmos.kext, the stalwart of most 10.5 Leopard installations was no longer suitable. Luckily, netkas has already been working hard and has produced a new fakesmc.kext to replace the dsmos.

I was now, with all these tweaks, able to boot Snow Leopard.

Hardware was still an issue, however, but there are solutions for my nvidia card and the onboard alc888 audio. The scariest fix here was the audio, as again I had to delve into the dsdt table to add a brand new section replacing the existing audio section. I couldn't get my JMicron kext to work, so I've just disabled the controller in the BIOS.

Bootup instructions
-------------------

To get the installation to boot I followed the following steps:

You need a pre-existant way of getting os x to boot. a hacked install dvd of 10.5 should work such as the ipc disk, also the boot cd called "dfe for hard disk" that certainly used to be available on torrent trackers can be used to boot a retail leopard dvd and then, once leopard has installed, to boot the installed partition (you need to do this via the dfe disk until you have a bootloader on the hard disk, which makes it slightly harder than the ipc install dvd). the easiest way is to install the ipc 10.5 system and boot into that.

Once in the installer, you MUST partition using the GPT option to be able to boot, which should be default when using OS X's [Disk Utility](https://en.wikipedia.org/wiki/Disk_Utility) to configure the disk. To be sure, when you open Disk Utility select the drive, choose the number of partitions you require, click Options and then select "[GUID Partition Table](https://en.wikipedia.org/wiki/GUID_Partition_Table)".

Make sure when you partition your disk to save a 10GB partition which will be used for the install image of Snow Leopard. This partition should be the first 10GB of the drive after any GPT required sections.

OS X's partitioner in Disk Utility is destructive in that you cannot partition a drive without wiping it's contents. There are ways around this such as by using `diskutil resizeVolume`, with HFS+ volumes, at the commandline but this requires an experienced hand at the tiller so I don't recommend it for n00bs.

When you have verified the size of your partitions (10GB + "The Rest"GB, e.g.), you can click apply to wipe the disk and prepare it with the new partitions.

When you have booted into Leopard, open disk utility and "restore" the 10.6 dvd to the 10GB partition you reserved earlier.

Ensure that you have chameleon installed (a version capable of booting snow leo), see <https://web.archive.org/web/20150910220201/http://chameleon.osx86.hu/>, and that it's being used to boot into os x.

If you installed chameleon onto your OS X partition then create the Extra directory in /

Or, if you installed chameleon onto your EFI partition (this is only available as an option for disks partitioned using GPT instead of [MBR](https://en.wikipedia.org/wiki/Master_boot_record)) then you need to mount the partition and create the Extra directory there:

```bash
mkdir /Volumes/BOOT sudo mount_hfs /dev/disk0s1 /Volumes/BOOT mkdir /Volumes/BOOT/Extra
```

Copy the kexts below to Extra/Extensions/ then run the following commands to set the ownership and permissions (mostly useful for if you're going to kextcache the extensions into Extra/Extensions.mkext)

```bash
find /path/to/Extra/Extensions -type f -exec chmod 644 '{}' \\;
find /path/to/Extra/Extensions -type d -exec chmod 755 '{}' \\;
chown -R root:wheel /path/to/Extra/Extensions
```

Copy the `dsdt.aml` file to `Extra/dsdt.aml`.

If you have an nvidia card, run osx86tools (see below for the download) and configure the nvidia card. do NOT select the option to write the "efi string" to the `system com.apple.Boot.plist`, instead copy the whole string to the clipboard (it's a series of what looks like hexadecimal)

Copy `/Library/Preferences/SystemConfiguration/com.apple.Boot.plist` to `Extra/com.apple.Boot.plist` and open in your favourite editor (I use the terminal application and 'nano'). Just before the last 'tag' create a new and pair with the following:

```xml
<key>device-properties<key>
<string>[your efi string for your graphics card - without the square brackets - it should all be on one line]<string>
```

Reboot into the chameleon bootloader and press a key at the prompt to "enter kernel parameters" or similar message, and select your osx installer partition which should be listed as bootable.

Hopefully this is enough to get around the issues, but you may find that you need to copy chameleon's Extra directory and file named "boot" to the root of your installation partition, and to `dd` the `boot1h` to the `rdisk0sX` partition that corresponds to your installation. (I did this myself before I tried installing, so I don't know whether it works without)

If you get a kernel panic which results in the grey screen of death telling you to restart your computer you need to follow the debugging tips at the bottom of this post to help determine where it's stopping. The diagnostics will help you do your google searches for information on how to get around the KP

Files
-----

The files I used are all below except for chameleon's bootloader which I've already posted the link to, so you don't have to go hunting.

- ~~`dsdt.aml` place this in `/Extra` – includes audio and rtc fix for MSI 975X Platinum PowerUp Edition.~~
  
  EDIT: the audio portion of the old dsdt.aml file no longer works for 10.6.3 released on 30 March 2010, so replace your dsdt.aml file with this one (same filename) and follow the VoodooHDA.kext instructions below.

- [fakesmc.kext](/wp-content/uploads/2010/02/fakesmc.kext_.zip): place this into /Extra/Extensions
  
  ~~LegacyHDA kexts place these into `/System/Library/Extensions` – these work in tandem with the audio dsdt fix~~.
  
  ~~EDIT: the LegacyHDA kexts don't work with the 10.6.2 update. Use `LegacyHDA.kext` which again is designed to work with my custom DSDT.aml file. I manually edited this kext from an online source. Place this kext in `/Extra/Extensions` and delete any copies of the kexts you downloaded from the bundle I've crossed off at the beginning of this paragraph.~~

  EDIT2: `LegacyHDA.kext` doesn't work with the latest release of snow, 10.6.3 as of 30 March 2010. Replace the `dsdt.aml` file in `/Extra` with the rtc-only fixed version(above), remove `LegacyHDA.kext` from `/Extra/Extensions` (and recreate your `/Extra/Extensions.mkext` if you're using one), and unzip & copy `VoodooHDA.kext` to `/System/Library/Extensions` (followed by a permissions fix scan using disk utility and, in the terminal,

  ```bash
  sudo touch /System/Library/Extensions
  ```

  to force a recreation of the system's main kext caches upon your next reboot)

- [NullCPUPowerManagement.kext](/wp-content/uploads/2010/02/NullCPUPowerManagement.kext_.zip): place this into `/Extra/Extensions`

- [OpenHaltRestart.kext](/wp-content/uploads/2010/02/OpenHaltRestart.kext_.zip): place this into `/Extra/Extensions` – ~~I don't know if this will work, yet as I've not tested it thoroughly.~~ EDIT: rebooting works fine.

- [AHCI kexts](/wp-content/uploads/2010/02/AHCI-kexts.zip): Place these into `/Extra/Extensions` – They fix SATA issues and are standard on virtually all OSX86 installs.

- [OSX86Tools](/wp-content/uploads/2010/02/OSX86Tools.zip): put this anywhere you like, though `/Applications` makes the most sense. Use it to create an EFI string for your graphics card, which should then be put into your `com.apple.Boot.plist` file in `/Extra`

Debugging
---------

When booting for the first time, or to try and diagnose a crash (the "grey screen of death" kernel panic) you should boot the system by passing some diagnostic flags to the kernel. At the chameleon screen it should prompt you to press any key to enter kernel parameters, at which point you should press spacebar and enter:

```
-f -v
```

that's the dash or "minus" sign. The `-f` tells os x to reload all kexts and the `-v` tells it to dump diagnostic messages to the monitor.

If chameleon doesn't prompt you to press a key to enter kernel parameters, then you should boot a working copy of OSX and edit your active `com.apple.Boot.plist` (if you followed the instructions above this should be in `/Extra`) and paste directly after the first instance of `""` two new lines which read:

```xml
<key>Timeout</key>
<string>5</string>
```

This will give you 8 seconds in which to press a key before the system boots. NOTE: when you press a key chameleon will also provide you with the partition selection screen to choose another partition than the default. You should use right and left keys to select the correct OS X partition and then enter `-f -v` before hitting return/enter to actually boot.

You can specify that the bootup always dumps debugging information as it runs by specifying Kernel Flags in the `com.apple.Boot.plist`. The relevant entry should already be present if you copied your `com.apple.Boot.plist` from `/Library/Preferences/SystemConfiguration/com.apple.Boot.plist`, but should be an empty string section ready for you to insert your changes:

### Original

```xml
<key>Kernel Flags</key>
<string></string>
```

### With Debugging

```xml
<key>Kernel Flags</key>
<string>-v debug=0x100</string>
```