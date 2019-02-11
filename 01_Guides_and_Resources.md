## Online Guides I Used

Below find a list of outside sources I utilized to learn about this process. If you have time, I encourage your read up on this as much as possible. One of the fun parts of this process is learning about how a Mac and a hackintosh works and then troubleshooting and putting it together.

* r/Hackintosh Vanilla Guide ([Getting Started - /r/Hackintosh Vanilla Desktop Guide](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/))
* u/cmer [GitHub - cmer/gigabyte-z390-aorus-master-hackintosh: A guide to build your own Hackintosh based on Gigabyte Z390 Aorus Master](https://github.com/cmer/gigabyte-z390-aorus-master-hackintosh)
* Gigabyte Z370 Gaming 5, 4K RX 580 - 10.13.4 SSDT [SUCCESS Gigabyte Z370 Gaming 5 - 4k RX 580 - 10.13.4 - SSDT | tonymacx86.com](https://www.tonymacx86.com/threads/success-gigabyte-z370-gaming-5-4k-rx-580-10-13-4-ssdt.250028/)
* Morganaut’s Build a Perfect Hackintosh [Build a Perfect Hackintosh - Beginners Tutorial - YouTube](https://www.youtube.com/watch?v=fA9AotXqkqA&t=781s) — An excellent starting point, but it doesn't complete some of the things to get a perfect hackintosh (for example, unless you use a separate PCIe card you still won't get bluetooth/wifi and will have trouble getting USB 3 speeds on your USB ports. But this was my **go to** videos to make myself comfortable with the broad steps.
* Carl Mercier’s Tutorial on Using Corpnewts USBMap Script. [How to fix USB 3 ports on a Hackintosh by generating your own SSDT or USBMap.kext - YouTube](https://www.youtube.com/watch?v=j3V7szXZZTc&t=83s) — This was the easiest method to create a custom SSDT to enable USB ports, but I ended up taking the hard route… well, just because I liked to challenge myself. The hard route is to do it manually using the instructions on the guide put together by Rehabman (listed below under “reading material”)
* Ibrahim Muslim’s Youtube Guide on SSDT Patching [USB 3.0 & 3.1 Permanent FIX | SSDT Patch | PART 1 of 3| For All MacOS X | 10.14.1 USB fix - YouTube](https://www.youtube.com/watch?v=dFihvGaLmMQ&t=10s) — I found this, a series of 3 Youtube videos, each running about 10 minutes to be rather long-winded but it gets the job done. Ultimately, after watching this video a few times, and re-reading Rehabman’s article (look under “A Guide to Creating a Custom SSDT for USBInjectAll.kext”), I was able to do it successfully.

## Reading Material

I read the following articles to inform me about exactly what was happening under the hood.

* Config.plist Wiki [Configuration](https://clover-wiki.zetam.org/Configuration#Config.plist-structure)
* Corpnewts USBMap Script [GitHub - corpnewt/USBMap: Py script for mapping out USB ports and creating a custom SSDT or injector kext (WIP).](https://github.com/corpnewt/USBMap) (I did not end up using this script although this makes the process insanely easy to generate your custom USB SSDT to enable USB ports. More later).
* The portion on the Coffee Lake Processor and the components of this config.plist in this guide was priceless. [Coffee Lake - /r/Hackintosh Vanilla Desktop Guide](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/config.plist-per-hardware/coffee-lake)
* An Idiots Guide to Lilu and its Plugins [An iDiot’s Guide To Lilu and its Plug-ins | tonymacx86.com](https://www.tonymacx86.com/threads/an-idiots-guide-to-lilu-and-its-plug-ins.260063/)
* A Guide to 10.11 USB Changes and Solutions by Rehabman [Guide 10.11+ USB changes and solutions | tonymacx86.com](https://www.tonymacx86.com/threads/guide-10-11-usb-changes-and-solutions.173616/) — Very important to understand how MacOS has changed how it accesses USB. An important primer to understand the next article which is quite complex.
* A Guide to Creating a Custom SSDT for USBInjectAll.kext [Guide Creating a Custom SSDT for USBInjectAll.kext | tonymacx86.com](https://www.tonymacx86.com/threads/guide-creating-a-custom-ssdt-for-usbinjectall-kext.211311/)

## Download Resources

* Clover Installer ([Clover EFI bootloader -  Browse /Installer at SourceForge.net](https://sourceforge.net/projects/cloverefiboot/files/Installer/)
* Clover Configurator  [Download Clover Configurator 5.4.0.0 | mackie100 projects](https://mackie100projects.altervista.org/download-clover-configurator/)
* GoldFish64’s Kext Repo [OneDrive](https://onedrive.live.com/?authkey=%21APjCyRpzoAKp4xs&id=FE4038DA929BFB23%21455036&cid=FE4038DA929BFB23) — This has a wonderful repo of always updated, newest kext versions.
* OsxAptioFix2Drv-Free2000.efi  [EFI-X99/OsxAptioFix2Drv-free2000.efi at master · koush/EFI-X99 · GitHub](https://github.com/koush/EFI-X99/blob/master/CLOVER/drivers64UEFI/OsxAptioFix2Drv-free2000.efi)  — seems to be a very useful driver, but I did not use it for my motherboard. I’m putting it here since I experimented with it, and it is not available in most repos.
* VegaTab-En — Vega 64 Kext Creator - [VGTab-en.zip - Google Drive](https://drive.google.com/open?id=1BHmkaoCzXD6xAq_-iKrhPM0z2eQhaitI) (**this is an external link**). If it doesn't work, please google search this. I have not been able to find an external repo where this is stored. 
