# Creating USB Installer

* For ease of use, I used a 32 GB USB Drive and downloaded the “Install MacOS Mojave.app” installer. You can use anything larger than 8GB for this purpose.

* I formatted the USB drive using **Disk Utility**, being careful to ensure that the main drive is erased, with *GUID Partition Map* and *MacOS Journaled Format*. I used Disk Utility that ships with my Macbook because I know how to use it, but this could also be done from the command line as highlighted in the Vanilla Hackintosh installation guide that used to be on this subreddits' sidebar, but now is on Github. I am linking it [here](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/building-the-usb-installer). 

If you are curious about using the command line, here is how you do it.

On another Mac, start the Terminal (located in /Applications/Utilities) and type 

'diskutil list'

This will give you a list of all the connected disks and their partitions. 

Take note of the disk ID (something like /dev/diskx) where `x` is a specific number. **Do not guess this part because we will erase it, and if you have the wrong disk ID then you could be erasing your hard drive instead of the USB drive**. Then type the following replacing disk# with your actual identifier:


`diskutil partitionDisk /dev/disk# GPT JHFS+ "INSTALLER" 100%`

This will partition the disk as listed above and rename it to "INSTALLER".


* Then, run the following command to install Mojave.

&#8203;

    sudo “/Applications/Install macOS Mojave.app/Contents/Resources/createinstallmedia” —volume /Volumes/INSTALLER

* This will take some time, without much output (even 30-40 minutes) but it will complete.
* This process now places the MacOS installer into the USB drive, but the USB drive still is **not** bootable. This is because in order for a PC to boot you need a special piece of software known as a **bootloader.** The bootloader in this case, will be stored in an *invisible partition* in your USB drive, called the "EFI" partition.  To create this into a bootable drive, we have to utilize a boot loader which is Clover Installer/Builder and create an EFI partition.
