# TROUBLESHOOTING

This section represents my log of issues that I ran into and how I fixed each one of them. I am including this as a spoiler so open it up and read about them only if you had the same problems. Otherwise, you can continue on. It is obvious that some of these problems are specific to my build and may not apply to you. If you run into problems, post the questions on various hackintosh forums and ask someone. Search google. There is an excellent chance that someone else had the very same problem you did and may have found a solution.

## Settings Changes were NOT taking effect after reboot.
 
On several occasions I ran into this weird bug where I would either remove a SSDT or DSDT patch and/or make a change and reboot the system, but the system would behave as if I didn’t make the change. I could not figure out until I found some information that it helps to boot into windows. Bizarre, right?

When this didn’t happen, all I needed to do was to go into a Windows installation from Clover, play around a little bit, exit and reboot into MacOS and the problem was solved.



## System Hangs during First Boot, even before the first graphics screen was visible.
 
This is one of the first errors I encountered that frustrated the heck out of me.  I tweaked with a lot of memory drivers including deleting AptioMemoryFix.efi, adding OsxAptioFixDrv2, OsxAptioFixDrv3, EMUVariable64.efi etc, and nothing worked. Finally, it turned out I had upgraded my Gigabyte Motherboard BIOS to a version (F12c) that was causing this issue. I downgraded the BIOS to F11 and the system booted up immediately.

## USB drives constantly get ejected during sleep and when computer wakes up drives have been improperly ejected.
 
This was a much more interesting problem, and reportedly has to do with RAM management. I noticed that in the BIOS, if I turned off **XMP (Extended Memory Profile)** this problem went away. However, I did not want to do this since I fully intended to overclock my system. I then found a great post on [TonyMacX86.com](https://www.tonymacx86.com/threads/usb-drives-getting-ejected-when-hackintosh-wakes-from-sleep-with-xmp-enabled.251488/page-2#post-1865336) which had the solution in this post that I linked. Basically, it turns out that the fastest Apple computers today (Jan 2019) the iMac Pro has only 2666 MHz DDR4 RAM, so you cannot go over that number in your base config. The solution was manually setting the RAM frequency down to 2666 MHz in the base config and then changing it up to 3200 MHz in the XMP and activating the XMP to Profile 1.  I am going to repost the relevant section from the BIOS that corresponds to this.

So this meant I had to change the M.I.T. settings in my BIOS to reflect this:
M.I.T. (You can ignore the M.I.T. settings if you don’t have the same memory frequency I have. I did this because I wanted to be able to enable XMP but this created a problem during boot up where the USB drives would get ejected when recovering from sleep-wake. The solution to this was to enable XMP but dial down the memory frequency to 2666 MHz as I have done below. However, this part is unnecessary if you don’t care about getting the best performance out of your memory).   
* -> Memory Frequency Settings  
  -> Extreme Memory Profile (X.M.P.) -> Profile1   
  -> System Memory Multiplier -> 26.66          
  -> Memory Ref Clock -> Auto   
  -> Memory Odd Ratio -> Auto   
  -> Memory Boot Mode -> Normal   
  -> Memory Frequency -> 2666 MHz 

</details>

## Onboard WiFi was not detected, and Bluetooth was extremely buggy.

As it turns out my motherboard has an internal Wifi card based on Intel which is not supported by Apple. Apple’s WiFi is supported by Broadcom. As such, I was able to buy a WiFi card from Ebay/NewEgg for about $50. The card I bought is linked below. Fenvi FV T919. This is a WiFi and Bluetooth card (PCIe x1 based, but the Bluetooth works via a separate USB controller that is accessed via a motherboard USB header).

The subsequent problem I ran into was with Bluetooth. The motherboard Bluetooth was recognized by MacOS alongside the BT from the Fenvi card, but when I try to connect there appeared to be some conflict rendering the overall BT nonfunctional. I fixed this during the time I applied the USB patch with an SSDT.  Please see that section for further information. But briefly, I ended up identifying that the internal BT controller works off of a USB controller (HS02) which I could shut off when I did the custom SSDT for USB ports. Once I shut off the USB controller for the internal BT, the Fenvi card’s bluetooth was the only thing that was active and it was fully operational.


## Audio not high quality.

Audio issues were really minor, but I did notice that the audio quality could be better. The fix for this was to apply a DSDT patch and change the Audio layout.  

In Clover Configurator   
-> ACPI -> DSDT patches   
* Add Patches -> Added “HDAS to HDEF”   
* Under Devices -> Audio -> Inject -> 11.   
* Select the checkbox “ResetHDA”

## Vega 64 Fans were loud and GPU was overheating with minimal load for no apparent reason.

This was one of my major problems, but reportedly well documented.  I’m not certain why this happens, but the fix was fairly straightforward and documented in both this [Youtube tutorial](https://www.youtube.com/watch?v=nxnEbsWT4pU&t=234s).

The key here was to download the file **VegaTab_en.zip** and create a kext file called Vegatab_FE.kext (this name is specific for my Vega 64 Frontier Edition, but the name maybe different for yours). I had to do some tweaking of the kext file that would be generated by changing some of the fan parameters. For me, I sent the idle speed to 800 rpm, target temperature to 70C, target fan speed to 1500 rpm and the max fan speed to 2000 rpm. This was copied into the EFI/Clover/kexts/other/ folder. I am not going to go over this in detail, since this is well described.



## FCPX and Preview cannot work properly due to unavailable iGPU hardware acceleration and unavailable Intel QuickSync.

The solution to this is rather problematic because doing one, enabling QuickSync and hardware acceleration breaks some less used features such as Netflix DRM in Safari or iTunes DRM. Since I am using this for video editing I focused on getting QuickSync enabled.

* In Clover Configurator, load up the config.plist file.
* In **ACPI**:
   * Click List of Patches and enable the following:
      `Change GFX0 to IGPU`
* In **Devices**:
   * `SetInject` to *11*. (This is for my motherboard)
   * Now to properly enable our headless iGPU, we need to fake the `device id` and `changeig-platform-id`. To do so, Click **Properties**, select `PciRoot(0x0)/Pci(0x2,0x0)`. Then, click the + button to add a property.
      Add:
      Property Key: `device-id`
      Property Value: `923E0000`
      Value Type: `DATA`
      
      Add:
      Property Key: `AAPL,ig-platform-id`
      Property Value: `0300923E`
      Value Type: `DATA`
      
      

## Getting iMessage, App Store, Hand Off, Continuity and Other Apple iServices to Work.

Full functionality of iMessage, App Store, iTunes, Handoff and Continuity is dependent on the functioning **SMBIOS, Rt Variables and System Parameters**. The SMBIOS is essentially the identifier that is injected into the MacOS to specify what type of Mac your hackintosh is. This is very important. In addition, the Mac serial number and your Ethernet adapter hardware address (MAC) is how MacOS and Apple identify your computer. The idea is to setup a Mac Serial number that is unique and therefore can be linked exclusively to your iCloud account. Problem is that if the number that is generated is already in one of the Macs in existence somewhere, these Apple services will not work. It has to be **truly** unique.

1. In Clover Configurator, first mount the EFI partition of the boot drive.
2. Go to **Rt Variables** section.
3. Under the ROM section, you can leave this section to state `UseMacAddr0`. Alternatively, you can copy the MAC address of your `En0` ethernet adapter and paste it here, in all lower case, without the colons. I chose to do the latter, although I am told that leaving `UseMacAddr0` accomplishes the same thing.
4. Now pay attention to the `BooterConfig` and `CsrActiveConfig`. Leave the `BooterConfig` alone.  The `CsrActiveConfig`, refers to the functionality of **SIP (System Integrity Protection)**.  SIP protects your Mac from having malicious code injected, and protects the important areas most vulnerable to virus, hackware, ransomware and malware.
5. You can Activate SIP by setting `CsrActiveConfig` to `0x00` to Deactivate SIP by setting it to `0x067`. I prefer to keep it active in my final drive (SSD), but it should remain inactive in a USB drive 
6. Now navigate to the **SMBIOS** section in Clover Configurator.
7. In this screen, pay attention to the arrowheads (far right, under the buttons Model Lookup | Check Coverage. Click that to open up and choose the Product Name closest to what you have. Because I have an i7-8700k I chose **iMac 18,3**.
8. Now, click as many times as possible on the *Generate New* button next to *Serial Number*.  The idea is to generate a serial number that fits your chosen Mac model, but is unique.
9. Once you are satisfied, click on *Check Coverage*, to go to the Apple Website to see if that model number is associated with any known Mac. Remember, **you ARE looking for an error message** here stating that the iMac with the serial number you chose is not available. If it shows that it is a valid Mac, keep doing this again, and again, and again until you find a serial number that is NOT associated with a Mac in existence.
10. Once you have a unique serial number that nobody else has, click on Model Lookup to ensure that serial number does indeed correspond to the iMac type you chose under product name. This is an extra verification step.
11. Remember to click the checkbox that says **Trust** — so that this is trusted Mac serial.
12. That’s it. If you reboot the system now you should be able to setup your iCloud account and everything should work well.



## I was unable to boot from USB but could boot from system SSD even when the EFI partition was copied from system SSD to USB drive : A lesson on SIP (Systems Integrity Protection).

This particular problem was a lesson to me on how USB drives behave differently than a system SSD even with the same configuration. I stumbled upon this baffling error, where, if I boot MacOS using the EFI partition in the system SSD, everything boots fine. But if I now copy that EFI partition to the USB drive and boot MacOS in the system SSD from the USB drive’s boot loader, right after the Apple Logo, the screen goes black and freezes with some pixelation. I found out, thanks to the help from a member in TonyMacx86 that this was because of SIP. Evidently when SIP is enabled the USB drive cannot act properly as the boot loader if the system is booting into an operating system residing elsewhere.  But it is not an issue if the OS and boot loader are on the same physical drive. Perhaps this is a protection mechanism. Once I disabled SIP in the USB drive, everything worked like a charm.

## USB drives do not work properly or USB 3.0 drives giving only USB 2.0 speeds.

This issue took me the longest to resolve, since I decidedly took the more challenging way of reading two really long, technically detailed articles about the issue.

* [A Guide to 10.11 USB Changes and Solutions by Rehabman Guide 10.11+ USB changes and solutions](https://www.tonymacx86.com/threads/guide-10-11-usb-changes-and-solutions.173616/) 
* [A Guide to Creating a Custom SSDT for USBInjectAll.kext Guide](https://www.tonymacx86.com/threads/guide-creating-a-custom-ssdt-for-usbinjectall-kext.211311/) 

The gist of these two articles is that since 10.11 MacOS introduced new USB drivers that rely on ACPI to get the USB information specific to that chipset. This means, that in order to properly use USB, the ACPI tables (DSDT and SSDT) has to be very accurate. Unfortunately, this means that since various Hackintosh motherboards don’t have the same port configuration as any Mac, the ACPI tables that MacOS is looking for will be different that what is present in your computer.  So to subvert this issue, **we can inject all available USB ports** into the MacOS (kind of a "throwing the kitchen sink" approach). It's not elegant, but at least temporarily it gets the job done. This is what the famous **USBInjectAll.kext** accomplishes. However, as of 10.14, MacOS also finalized a 15-port limit.  This seems like a lot until you realized that the 15 port limit includes USB2 and USB3, where USB2 counts as 1 port, and USB3 counts as 2 ports. So 15 ports isn’t all that much. So in order to really get your Mac to run USB 3 at USB 3 speeds and USB 2 and USB 2 speeds, you need to help MacOS out by creating an ACPI table that specifies which ports are what, in accordance with your motherboard configuration. This is what we will do by creating a Custom SSDT. 

So the point is that you can get all of your ports active (albeit at slow speeds, at least was the case with me) with the USBInjectAll.kext, but in order to leverage the speeds from USB3 ports, we have to create a custom SSDT (ACPI table) to instruct the system accurately which ports are where and for what purpose they are used for.

The actual guide to doing this way too complicated for me to write here, so I will point to a few wonderful sources.  First and foremost, read the two articles linked above, the first one which underscores the problem, and the second one which teaches you how to use two pieces of software (IORegistryExplorer and MacIASL 2) to create SSDT which can be loaded on Clover Configurator.

The way to do this is explained in the articles linked above, but is also outlined in the [Youtube video by Ibrahim Muslim](https://www.youtube.com/watch?v=dFihvGaLmMQ&t=81s). This is rather long winded video but you will get a good feel for how to do this. There are 3 parts to this video. It takes about 30 minutes to watch all three parts and he didn't really edit the video well, but it's a good video to watch if this is the first time you are doing this.

I also found that CorpNewt had created an easy to use script called USBMap that enables you to map the USB ports and port configuration on your motherboard and you can create an SSDT using this script all automatically [GitHub - corpnewt/USBMap](https://github.com/corpnewt/USBMap). I think this is by far the easiest option.   

There is a fabulous [Youtube video from Carl Mercier](https://www.youtube.com/watch?v=j3V7szXZZTc&t=151s) demonstrating how to use corpnewts USBMap script to map out all the ports and create a custom SSDT. 

All of the above are great resources. I ended up using the manual method which uses IORegistryExplorer and MacIASL to get the job done. It took more time, but I felt like I learned a lot more testing it out piecemeal.  The result was great.

If you are using the Gigabyte Z370 Aorus Gaming 5 motherboard, this is the port map I discovered.

    1. HS01/SS01 - Unknown
    2. HS02/SS02 - Bluetooth Device
    3. **HS03/SS03 - USB 3.0 DACUP Yellow (left)**
    4. **HS04 /SS04 - USB 3.0 DACUP Yellow (right)**
    5. **HS05/SS05 - USB 3.0 BLUE (left)**
    6. **HS06/SS06 - USB 3.0 BLUE (right)** 
    7. **HS07/SS07 - Front RIGHT USB (next to power switch)** 
    8. **HS08/SS08 - Front LEFT USB (next to indicators)** 
    9. HS09  - Internal USB2.0_2 motherboard header
    10. HS10 - Internal USB2.0_2 motherboard header
    11. **HS11 - Internal USB2.0_1 motherboard header**
    12. **HS12 - Internal USB2.0_1 motherboard header**
    13. **HS13 - USB 2.0 BLACK (left)**
    14. HS14 - USB 2.0 BLACK (right)
    15. USR1 - Unknown
    16. USR2 - Uknown

I have bold-faced above, the ports I decided to keep.

    HS03, SS03 
    HS04, SS04 
    HS05, SS05 
    HS06, SS06 
    HS07, SS07 
    HS08, SS08 
    HS11 
    HS12 
    HS13
    Total number of active ports = 15


* Note that I disabled HS02/SS02 which is the internal Bluetooth. As soon as I disabled this, my Bluetooth card became fully operational since it was connected to the USB 2.0_1 motherboard header via an internal USB hub.
* My motherboard has at least 2x USB-C connectors, which are not operational. Unfortunately, they are not intel USB controllers but are ASMedia controllers. Hoping that they may work, I also installed the USBGenericXHCI.kext file. We will have to see if it actually does.

If you are interested in using my custom SSDT I would be happy to share. Just leave a comment. However, for the spirit and sake of learning I urge you to practice creating it yourself. Also, please know that my custom SSDT only works if you have the **EXACT** same motherboard as I have. 
