---
title: "Installing and using WSL2 on Windows Server 2022 Core (without the Desktop Experience)"
date: 2022-09-14T11:38:41+01:00
draft: false
---

Installing and running WSL2 on Windows Server 2022 Core, i.e. without the desktop experience installed, is a bit more involved than when you have the desktop environment available to access locally or via a remote desktop session. Here I'm going to list the required steps, and caveats, for getting WSL2 installed and running in Windows Server 2022 Core.

## Optional: Enable remote access

Some of the steps below involve complex text that needs to be typed or copied into a console session. It is easier to copy and paste with a remote session via "Remote Desktop" to ensure the accuracy of the complex text. This is entirely optional, but can reduce the chance of typos.

1. Login to your server
1. If SConfig does *not* start automatically, launch it by typing `sconfig` followed by <kbd>enter</kbd>
1. In SConfig choose the "Remote desktop" option by typing <kbd>7</kbd> followed by <kbd>enter</kbd>
1. Next, press <kbd>e</kbd> followed by <kbd>enter</kbd> to choose to enable Remote desktop
1. Decide whether you want to require "network level authentication" for your remote clients. This is the most secure option, and all official Microsoft clients support this feature
1. If you want to require Network Level Authentication, choose the first option by pressing <kbd>1</kbd> followed by <kbd>enter</kbd>. Otherwise, choose the second option to disable the requirement by pressing <kbd>2</kbd> followed by <kbd>enter</kbd>
1. Press <kbd>enter</kbd> to go back to the main menu

## Update your system

Micsoroft Update should be enabled for WSL2 to [keep the Linux kernel and up-to-date automatically](), according to the Microsoft documentation.

> NOTE: I haven't checked to see how to set up systems that use WSUS to manage their updates, so this assumes you are *not* using WSUS.

### Microsoft Update

If your server does not currently have Microsft Update enabled, which will be the case if you haven't explicitly enabled it as part of another installation or by any other means, then you can enable it with the steps below.

> NOTE: I recommend doing these steps in a Remote Desktop session so that you can copy and paste the command to avoid any typos in the UUID value (the long hexadecimal text)

1. Log into your server as an administrator
1. If SConfig starts automatically, exit to a console session
1. If PowerShell is not your default shell, start it by typing `powershell` followed by <kbd>enter</kbd>
1. In PowerShell run the following command:
    ```powershell
    (New-Object -ComObject Microsoft.Update.ServiceManager).AddService2("7971f918-a847-4430-9279-4a52d1efe18d",6,"")
    ```

> Thank you to [Don Zoomik](https://blog.zoomik.pri.ee) for converting the [VBScript example on Microsoft Docs](https://docs.microsoft.com/windows/win32/wua_sdk/opt-in-to-microsoft-update?WT.mc_id=AZ-MVP-5004323) to [PowerShell shared via StackOverflow](https://serverfault.com/a/1004581/530672), which I've based this on. I used a different flag `6` instead of `7` from the example to ensure that Microsoft Update is registered immediately.

### Run the update process

We now need to ensure that your Windows Server 2022 installation has run at least once after enabling Microsoft Update. We also verify that all quality updates are installed from Microsoft Update. This is easy with the `SConfig` utility:

1. Log into your server as an administrator
1. If SConfig does *not* start automatically, launch it by typing `sconfig` followed by <kbd>enter</kbd>
1. Press the <kbd>6</kbd> key on your keyboard to select the "Install Updates" option, and press <kbd>enter</kbd>
1. Choose the "All quality updates" option by pressing the <kbd>1</kbd> key followed by <kbd>enter</kbd>

#### If there are updates available

1. Press <kbd>a</kbd> followed by <kbd>enter</kbd> to install all the available updates
1. If prompted to reboot press <kbd>y</kbd> followed by <kbd>enter</kbd> to reboot your server
1. Otherwise press <kbd>enter</kbd> to return to the main menu

Repeat these three steps until you have successfully installed all available updates.

#### If there aren't any updates available

1. Press <kbd>enter</kbd> to return to the main menu. Your system is already up to date so we don't need to do anything further here

## Install WSL2

Once your server is configured for Microsoft Update and fully up-to-date with patches, by following the process above, we can install WSL2.

1. Login to your server as an administrator
1. If SConfig starts automatically, exit to a console session
1. Use the following command to install WSL2:
    ```cmd
    wsl --install
    ```
1. You now need to reboot your server for WSL2 to be usable for the next steps

    > NOTE: The `wsl --install` command will print the following two errors as part of its output. They can be safely ignored.
    > ```
    > A error was encountered during installation, but installation may continue. Component: 'WSL Kernel' Error Code: 0x80040154
    > A error was encountered during installation, but installation may continue. Component: 'Ubuntu' Error Code: 0x80040154
    > ```
    > I'm unsure what causes the first error, but we will work around it by installing the kernel manually via the MSI package in the next step.
    >
    > The second error is a byproduct of running on a system without the Desktop Experience and its supporting infrastructure. The install command attempts to download and install the Ubuntu distro, or another distro if you select one when issuing the `wsl --install` command, from the Windows Store but fails with a message similar to the one above.
    >
    > The reason for the failure is that the distros on the Windows Store are distributed as `.appx` packages. These packages are unsupported (as far as I can tell) on Servers without the Desktop Experience enabled, so the installation attempt fails. We will work around this in the next section.

## Installing the WSL2 kernel

Once your server is rebooted, we need to manually install the WSL2 kernel via an `.msi` package or WSL2 will not function at all.

1. Login to your server as an administrator
1. If SConfig starts automatically, exit to a console session
1. Download the MSI package with:
    ```powershell
    curl -o wsl_update_x64.cab https://catalog.s.download.windowsupdate.com/d/msdownload/update/software/updt/2022/03/wsl_update_x64_8b248da7042adb19e7c5100712ecb5e509b3ab5f.cab
    ```
    > NOTE: you can find the latest kernel package by loading https://www.catalog.update.microsoft.com/Search.aspx?q=wsl in your web browser, and selecting the `download` button to the right of the newest entry. The popup window will include two links; one for x64 and one for arm64. Right-click the x64 link and select "copy link" or equivalent. When you have the address copied to your system clipboard, paste it into the command above replacing the original address.
1. Expand the MSI package from the `.cab` file
    ```powershell
    expand.exe wsl_update_x64.cab wsl_update_x64.msi
    ```
1. Install it with:
    ```powershell
    msiexec.exe /package wsl_update_x86.msi /passive /promptrestart
    ```

## Installing a distro

1. Choose a distro from the list below and copy the URL
    - Ubuntu 16.04: https://aka.ms/wsl-ubuntu-1604
    - Ubuntu 18.04: https://aka.ms/wsl-ubuntu-1804
    - Ubuntu 20.04: https://aka.ms/wslubuntu2004
    - Ubuntu 22.04: https://aka.ms/wslubuntu2204
    - Debian GNU/Linux: https://aka.ms/wsl-debian-gnulinux
    - Kali Linux: https://aka.ms/wsl-kali-linux-new
    - SUSE Linux Enterprise Server 12: https://aka.ms/wsl-sles-12
    - SUSE Linux Enterprise Server 15 SP2: https://aka.ms/wsl-SUSELinuxEnterpriseServer15SP2
    - SUSE Linux Enterprise Server 15 SP3: https://aka.ms/wsl-SUSELinuxEnterpriseServer15SP3
    - openSUSE Leap 15.2: https://aka.ms/wsl-opensuseleap15-2
    - openSUSE Leap 15.3: https://aka.ms/wsl-opensuseleap15-3
    - openSUSE Tumbleweed: https://aka.ms/wsl-opensuse-tumbleweed
    - Oracle Linux 7.9: https://aka.ms/wsl-oraclelinux-7-9
    - Oracle Linux 8.5: https://aka.ms/wsl-oraclelinux-8-5

    > NOTE: if you can find a filesystem `.tar.gz` file for your preferred Linux Distro, you may use that.
    >
    > For example, Ubuntu provides suitable files at https://cloud-images.ubuntu.com/releases/. You will find the appropriate `.tar.gz` file within the directory for your chosen Ubuntu release codename with the suffix `-server-cloudimg-amd64-wsl.rootfs.tar.gz`, e.g. https://cloud-images.ubuntu.com/releases/jammy/release/ubuntu-22.04-server-cloudimg-amd64-wsl.rootfs.tar.gz for the Ubuntu 22.04 (Jammy) release. Ubuntu only provides suitable images since the 16.04 (Xenial) release.
1. Login to your server (preferably as an unprivileged user)
1. If SConfig starts automatically, exit to a console session
1. If PowerShell is not your default shell, start it by typing `powershell` followed by <kbd>enter</kbd>
1. If you are using a URL from the above list, insert it into the following command replacing `https://aka.ms/wslubuntu2204`. Paste or type the whole, amended, command into PowerShell and execute it by pressing <kbd>enter</kbd>:
    ```powershell
    curl.exe -L -o wsl-distro.zip https://aka.ms/wslubuntu2204
    ```
    Otherwise, if you have the direct link to a `.tar.gz` file, download it directly and skip to step 11:
    ```powershell
    curl.exe -L -o install.tar.gz https://cloud-images.ubuntu.com/releases/jammy/release/ubuntu-22.04-server-cloudimg-amd64-wsl.rootfs.tar.gz
    ```
1. Unpack the ZIP file with:
    ```powershell
    Expand-Archive wsl-distro.zip .\extracted-distro-step1
    ```
1. Use `Get-ChildItem .\extracted-distro-step1` to list the contents of the directory `.\extracted-distro-step1`, which is where we unpacked the ZIP file.
1. Find the name of the `.appx` file that contains the actual WSL2 filesystem `.tar.gz` file; it will be named like `distro.appx`, `distro_x64.appx`, or `distro.*_x64.appx`, where `distro` is the name of the distro you downloaded, and `*` is the a version number like `0.10.0` - the actual nunmber isn't important as it should be the same for every `.appx` file in the extracted directory. e.g. For Ubuntu 22.04 the filename from the package that I downloaded while preparing this tutorial was `Ubuntu_2204.0.10.0_x64.appx`.
1. With the file name you discovered in the previous step, renaming the `.appx` file to be suffixed `.zip` and extract it with the following two commands:
    ```powershell
    Rename-Item .\extracted-distro-step1\Ubuntu_2204.0.10.0_x64.appx distro.zip
    Expand-Archive .\extracted-distro-step1\distro.zip .\extracted-distro-step2
    ```
1. Use `Get-ChildItem .\extracted-distro-step2` to find the `.tar.gz` file included in the extracted `.appx` file. For Ubuntu 22.04 this file is named `install.tar.gz`, but different distros may vary the name they use
1. Import the filesystem `.tar.gz` file as a new WSL2 distro with this command:
    ```cmd
    mkdir C:\Users\Administrator\WSL-disks
    wsl --import Ubuntu2204 C:\Users\Administrator\WSL-disks\Ubuntu2204 .\extracted-distro-step2\install.tar.gz
    ```
    Replace `Ubuntu2204` with any name you desire. This will be used to identify your distro instance in `wsl --list` output, and for you to explicitly select if it isn't the default distro with `wsl --distro Ubuntu2204`.

    Also replace `C:\Users\Administrator\WSL-disks\Ubuntu2204` with any directory you have writable access to. This directory will be used to store the virtual disk image that holds the distro's filesystem - the disk image will be named `ext4.vhdx`. Each distro instance that you import should use a unique directory to prevent overwriting a different distro instance's disk image. The parent directory, here `C:\Users\Administrator\WSL-disks`, must already exist, but the `Ubuntu2204` directory will be created by `wsl --import`.

    If you downloaded and extracted an `.appx` package, replace `.\extracted-distro-step2\install.tar.gz` with the path to the distro's `.tar.gz` filesystem archive that you found in the `extracted-distro-step2` directory. Otherwise, if you downloaded a `.tar.gz` file directly replace `.\extracted-distro-step2\install.tar.gz` with the path to the `.tar.gz` file you downloaded.
1. Start the distro with `wsl --distro Ubuntu2204`. Again, replace `Ubuntu2204` with the name you assigned your distro instance
1. Create an unprivileged user so that you may run without root privileges until required. This is Linux best practice, as running as root by default is discouraged. Add the user with the following commands:
    ```bash
    adduser desired-username
    adduser desired-username users
    adduser desired-username sudo
    ```
    You will be prompted to provide a password for the user, and again to confirm that you typed it correctly. You may not see any output as you type the password as the output is usually disabled while entering passwords. Some distros will print `*` characters as you type a password into the prompt to provide feedback but this is quite rare.
    > NOTE: some distros don't have a `sudo` group. These distros often use `wheel` instead, so if you get a message indicating that there is no `sudo` group on the system then replace `sudo` with `wheel` in the command and try again.
1. Configure the WSL2 distro to default to your new user account when launching a shell, unless overidden with the `--user` parameter when calling `wsl` (e.g. `wsl --user root --distro Ubuntu2204`):
    ```bash
    cat > /etc/wsl.conf <<'EOF'
    [user]
    default = "desired-username"
    EOF
    ```
    The command above uses a "heredoc", which is a POSIX shell feature that copies the contents between two markers, here `'EOF'` and `EOF` (the quotes around the first `EOF` tell the shell not to expand any variables that might be in the heredoc. Variables are indicated by a `$` sign followed by name, e.g. `$HOME`.) The marker at the end of the heredoc must be on a line by itself with no leading spaces.
   
    In the command above we're using redirectors `<<` to copy the heredoc into the command `cat`. We are using a different redirector `> /etc/wsl.conf` to take the output of `cat` and write it into the file named `/etc/wsl.conf`. This combination of redirections with the `cat` command allow us to write a heredoc directly into a file that we choose to save us using a text editor.
   
    If you prefer to use a text editor then that is fine. Ensure that you add the following content into the `/etc/wsl.conf` file with your preferred text editor:
    ```ini
    [user]
    default = "desired-username"
    ```
    > NOTE: The `wsl.conf` file follows the INI file, with headings, format.

    You can find out about the other options you can add into the `wsl.conf` file in [Advanced settings configuration in WSL](https://docs.microsoft.com/windows/wsl/wsl-config?WT.mc_id=AZ-MVP-5004323#configuration-settings-for-wslconf) on [the WSL section of the Microsoft Docs website](https://docs.microsoft.com/windows/wsl/?WT.mc_id=AZ-MVP-5004323).
1. Finally, terminate the WSL2 distro instance so that your changes to `/etc/wsl.conf` are recognised; in PowerShell run:
    ```powershell
    wsl.exe --terminate Ubuntu2204
    ```

## Finished

Congratulations, you now have WSL2 installed, and have set up a distro instance for your Windows User.

WSL2 distros are not shared system-wide so each user on the Server has their own separate WSL2 instances if they install any, and cannot access another user's instances.

> NOTE: User accounts do *not* need any special privileges to run WSL2 distros.

One thing that I've noticed is that despite the documentation provided by Microsoft indicating that enabling Microsoft Update will keep the Linux kernel updated automatically, I don't believe this currently works in Windows Server 2022. We've performed the required steps to enable the capability but it seems that WSL2 and Microsoft Update do not currently update the kernel on Windows Server 2022 installations. You cannot update the kernel with `wsl --update`, either, as the command will report that the currently running kernel is already up-to-date even if Microsoft have released updated versions. For now, use the [Microsoft Update Catalog](https://www.catalog.update.microsoft.com/Search.aspx?q=wsl) as shown in the [Installing the WSL2 kernel](#installing-the-wsl2-kernel) section above.
