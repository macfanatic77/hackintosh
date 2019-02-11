# First Boot

* Eject the drive that now contains MacOS Mojave installed, along with an active boot loader (via Clover installer) and a config.plist that we modified using Clover configurator.
* At the BIOS screen, choose boot drive and select the USB drive.
* **Boot from the USB drive** on Clover.
* Now you will probably see verbose text, and if everything goes right you will get to the screen that states “Please Choose your Language”
* Incidentally, this is where I hit my first problem. Please see [*Troubleshooting*](07_Troubleshooting.md).
* Once you have accepted the license agreement, and are greeted with the first menu, instead of choosing “Install MacOS”, instead choose “Start Disk Utility”.
* Under Disk Utility, select the drive that you plan to install MacOS into.
* Format this drive in **APFS format**.
* Now close Disk Utility and when you return to the Menu choose to “Install MacOS”.
* The installer will continue here, and should reboot at least twice. Don’t touch anything during this reboot, since Clover will automatically choose the “Preboot” volume, which is correct.
* Let the installation complete.
* The next time the system reboots, choose “**Boot from MacOS SSD**” (or whichever drive you wanted MacOS installed into) from the boot menu.
* If everything goes well, you should boot into MacOS.

# Checking to Ensure Everything is Working

* First place to head over to is Apple Menu -> About this Mac -> System Report.
* Make sure that an Audio Device, Ethernet, Bluetooth and WiFi are working. These are the usual culprits and in my case Audio and Ethernet was functional, but Bluetooth and WiFi was not.
* We will look at how to activate some of the other nonfunctioning systems out of the box, in the section on troubleshooting below.

# Transferring functioning EFI folder from USB boot loader to EFI partition on SSD.

* Assuming that everything is working, once everything is said and done, you should be ready to transfer the EFI partition from the USB drive to the SSD. This is because the SSD now has MacOS installed, but does not have a boot loader to inject the UEFI drivers and kexts during boot up. This information is in the EFI partition in the USB installer.
* So have to repeat the previous process. However, don’t format the SSD (It already now has MacOS installed). So what we need to do is just run Clover Installer/Builder to create the boot loader.
* Run it as we have done before on the USB drive, but under destination, now choose the SSD. Select the same options as we had done before.
* The next part is a little bit different. You don’t need to download a config.plist or load clover. We’ve already painstakingly tweaked clover and config.plist along with the kexts that we need. All those tweaks are in the USB drive’s EFI partition.
* So, using Clover Configurator, what you need to do is mount the EFI partitions in the USB drive that you loaded MacOS from, and the SSD to which you installed MacOS. Delete the EFI folder in the EFI partition inside the SSD and copy the EFI folder from the EFI partition in the USB. This should create a copy of the boot loader into the SSD.
* Now reboot your system. Remove the USB drive.
* If everything went as planned, your system should boot from the SSD without the USB.
