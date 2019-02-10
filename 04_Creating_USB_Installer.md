# Creating USB Installer

* For ease of use, I used a 32 GB USB Drive and downloaded the “Install MacOS Mojave.app” installer.
* I formatted the USB drive using **Disk Utility**, being careful to ensure that the main drive is erased, with *GUID Partition Map* and *MacOS Journaled Format*. I used Disk Utility that ships with my Macbook because I know how to use it, but this could also be done from the command line as highlighted in the Vanilla Hackintosh installation guide that used to be on this subreddits' sidebar, but now is on Github. I am linking it [here](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/building-the-usb-installer). 
* Then, run the following command to install Mojave.

&#8203;

    sudo “/Applications/Install macOS Mojave.app/Contents/Resources/createinstallmedia” —volume /Volumes/USB

* This will take some time, without much output (even 30-40 minutes) but it will complete.
* This process now places the MacOS installer into the USB drive, but the USB drive still is **not** bootable. This is because in order for a PC to boot you need a special piece of software known as a **bootloader.** The bootloader in this case, will be stored in an *invisible partition* in your USB drive, called the "EFI" partition.  To create this into a bootable drive, we have to utilize a boot loader which is Clover Installer/Builder and create an EFI partition.
