# Basics of the Vanilla Method

In the Vanilla method the objective is to install **all of the configuration** for the entire MacOS inside the EFI partition in the bootable drive (whether this is the USB drive, or eventually the hard drive in which your OS resides). This is different than some of other other standard methods for hackintosh installation that requires that these configuration files be placed within the operating system itself.

![](https://web1.cs.wright.edu/~pmateti/Courses/4350/Lectures/SysProgs/Figures/bootloader.png)
###### The above schematic shows how a bootloader interacts with the operating system. 

For example, in the Unibeast/Multibeast method which has been a standard in hackintosh installation for a very long time, the configuration information can be split between the bootloader and between folders within the operating system (/system/library/extensions). This works well for the most part, until the operating system itself gets updated (a MacOS update for example), and the config information that is in the operating system folders can no longer be read or are modified by the update itself.

The **Vanilla** method tries to avoid this problem altogether by maintaining complete geographic isolation of the bootloader and configuration files from the operating system itself. The way it does this by the creation of a separate **EFI** partition in the boot drive. This EFI partition is not visible or accessible to us or to the standard operating system, but it is there and accessible to the computer's firmware during the bootup process.


## What happens during the boot process of a hackintosh

When a normal Mac boots up, the hardware in the Mac will send proprietary information to the MacOS indicating first that the hardware is genuine Apple (this is known as the **SMC**) and then it will send information to MacOS about the Mac's hardware configuration (this is done via UEFI drivers, ACPI tables etc). Once MacOS has this information it can begin the boot process smoothly. 

In a hackintosh, this is not so simple. The Apple hardware (and SMC) is not present, so our hackintosh has to "fake" the SMC information and send a message to MacOS stating this is a real Mac, when it isn't. Then, our hackintosh configuration file has to be read in a way that is interpretable to the MacOS system and fed to the system so that as the MacOS is loading up it recognizes the way your PC is setup.

When this is done in a hackintosh installed via the vanilla method, the **bootloader** will read the configuration for your hackintosh off a bunch of files that will be placed in the EFI partition and **inject** this information into the MacOS operating system as it is loading. 

*The whole concept of injection was a little weird to me early on as I was learning about this until I understood what it means. The configuration is literally fed into the MacOS as the system is booting up. In other words, nothing about the configuration is written into the MacOS when the system is powered down*.

The major advantage of this particular style of installation (Vanilla method) is that because the "spoof" files and configuration data are all stored in a separate, **isolated** EFI partition, the MacOS containing drive in your hackintosh  can function completely normally in any real Mac. I personally liked this method a lot for a few reasons:

1. If I ever need to access my data or get a computer booted from my hard drive, any normal Mac can boot directly into your hackintosh vanilla hard drive. 
2. Because you know the location of the "artificial" configuration files you introduced, whenever you have to troubleshoot you only need to access one location (EFI partition)
3. In general, updates to MacOS don't harm your computer (with some exceptions of course). 

## What is Clover?

(I originally had some misinformation regarding how and what Clover is and how it operates as the boot manager for MacOS in our hackintosh. I am going to post content from the message sent to me by [midi1996](https://github.com/midi1996) who clarified this for me. Thank you for contributing!). 

Clover is a "Boot Manager" that allows MacOS to load the "boot.efi" which is the bootloader for MacOS. Clover itself is not a bootloader. When the system starts, Clover Boot Manager (CBM) injects "on the fly" the information pertaining to your specific hackintosh configuration to the MacOS bootloader/operating system. This information includes SMBIOS, Boot arguments, Patched ACPI files etc. This is stuff that MacOS needs to understand the hardware configuration of your hackintosh. Without this information MacOS cannot start seamlessly.

The information about your hackintosh configuration that clover injects is recorded in the configuration file *config.plist*. This file is a .plist (property list) file which stores information in XML format. It can literally be edited in any text editor (textedit or Xcode), but **Clover Configurator** a separate piece of software (that is not associated with Clover at all, incidentally), that allows beginners to change the content of the config.plist within a GUI interface. As I am told, more advanced plist editors such as Xcode, Clover Configurator Pro etc can make more precise changes to the config.plist. However, if you are reading this guide, like me, you are probably still a beginner so may want to stick to the easy way.

A typical hackintosh runs on two sets of drivers. First, are the drivers are required by the boot manager (Clover). These are UEFI drivers that we typically install when we install the boot manager. The UEFI drivers often have the .efi extension in their filename and include drivers that help Clover load partitions (HFSPlus, VboxHfs, ApfsDriverLoader), and fix firmware issues that MacOS may typically complain of during the loading period (AptioMemoryFix, OsxAptioFixDrv etc). By itself, Clover does not require these files, but will need to them to correctly pass on information about your system to the MacOS bootloader.

The second set of drivers are kernel extensions, better known as "kext" files. These drivers are **not** required by Clover, but are needed by MacOS. In a genuine Apple MacOS installation, these kext files are stores in the system drive (usually in /system/library/extensions), and in some of the older (and less elegant) methods of hackintosh installation these kext files can be distributed in these intrinsic locations. What makes the Vanilla method so unique is that we geographically isolate ALL the files (UEFI drivers required by Clover and the kext files required by MacOS) into the ESI (EFI system partition) where the boot manager and boot loader resides. That way, if there is ever an issue in our hackintosh system we can be sure that the issue is usually in one of the files or configuration residing in that partition. 

###### Many thanks for [midi1996](https://github.com/midi1996) for educating me on this!
