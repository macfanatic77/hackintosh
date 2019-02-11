# Basics of the Vanilla Method

In the Vanilla method the objective is to install **all of the configuration** for the entire MacOS inside the EFI partition in the bootable drive (whether this is the USB drive, or eventually the hard drive in which your OS resides). This is different than some of other other standard methods for hackintosh installation that requires that these configuration files be placed within the operating system itself.

![](http://cecs.wright.edu/~pmateti/Courses/4350/Lectures/SysProgs/Figures/bootloader.png)
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

The EFI partition is built by the **Clover Builder** (also known as Clover Installer), a piece of software that generates the partition initially on your USB drive. Once we have a functioning Mac, we will copy this partition over to your system boot drive (hard drive or SSD), so that your hackintosh can start without having to stick a USB drive to get the bootloader going.  There is another piece of software called the **Clover Configurator** which will read the information in the EFI partition and give you a graphical interface to configure it. As you troubleshoot, you will become intimately familiar with Clover Configurator. This configuration information produced by Clover Configurator is written into another file called *config.plist* which will be in the /EFI/Clover/ folder inside that EFI partition. Remember, the EFI partition is normally invisible so you cannot access it unless it has been specifically mounted. Clover Configurator can mount that partition whenever you need to change something in the configuration of your hackintosh.


