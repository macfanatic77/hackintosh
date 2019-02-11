## BIOS Settings (Gigabyte BIOS F11)

**Note:** I initially updated the BIOS to version F12c which is the latest from Gigabyte. The version that my motherboard shipped with was F6. This created some problems where the system would hang during very first boot up of MacOS (see *[Troubleshooting](07_Troubleshooting.md)*).

* Load Optimized Defaults

<details>
  <summary> M.I.T - Check this settings only if you plan to overclock on this board. For most people this part is unnecessary. Click the arrowhead to the left to look at how the M.I.T. settings should be configured.</summary>
  
* M.I.T. (You can ignore the M.I.T. settings section here, if you don’t have the same memory frequency I have. I did this because I wanted to be able to enable XMP with plans to overclock my CPU later, but this created a problem during boot up where the USB drives would get ejected when recovering from sleep-wake. The solution to this was to enable XMP but dial down the memory frequency to 2666 MHz as I have done below. However, this part is unnecessary if you don’t care about getting the best performance out of your memory).

   * \-> Memory Frequency Settings
   * \-> Extreme Memory Profile (X.M.P.) -> Profile1
   * \-> System Memory Multiplier -> 26.66
   * \-> Memory Ref Clock -> Auto
   * \-> Memory Odd Ratio -> Auto
   * \-> Memory Boot Mode -> Normal
   * \-> Memory Frequency -> 2666 MHz
   </details>
   
   
   
* BIOS
   * \-> Security Options -> System
   * \-> Boot Option Priorities -> UEFI OS 
   * \-> Fast Boot -> **Disabled**
   * \-> Windows 8/10 Features -> Windows 8/10
   * \-> CSM Support -> **Disabled**
* Peripherals
   * \-> Initial Display Output -> PCIe 1 Slot
   * \-> Above 4G Decoding -> Disabled (I enabled this and system crashed)
   * \-> LEDs in Sleep, Hibernation or Soft Off State -> Disabled
   * \-> Intel Platform Trust Technology -> Disabled
   * \-> SW Guard Extensions (SGX) -> Disabled
   * \-> USB 3.0 DAC-UP 2 -> Normal
   * \-> Trusted Computing -> Disabled
   * \-> Intel BIOS Guard Technology -> Disabled
   * \-> USB Configuration
   * \-> XHCI Hand Off -> **Enabled**
   * \-> Network Stack Configuration
   * \-> Network Stack -> **Disabled**
   * SATA and RST Configurations
   * \-> SATA controllers -> Enabled
   * \-> SATA Mode Selection -> **AHCI**
* Chipset
   * \-> VT-d -> **Disabled**
   * \-> Internal Graphics -> **Enabled \*\***
   * \-> DVMT Pre-Allocated -> 128M
   * \-> DVMT Total Gfx Mem -> 256M
   * \-> Audio Controller -> Enabled
   * \-> PCH LAN controller -> Enabled
   * \-> Wake on LAN -> Disabled
   * \-> IOAPIC 24-119 -> **Enabled**
* Power
   * \-> Platform Power Management -> Disabled
   * \-> AC Back -> Always On
   * \-> ErP -> Disabled

\*\* If you are wondering why I enabled Internal Graphics in the BIOS when I had such a fancy pants GPU it was because of my need to keep the internal graphics for non-display purposes. The Internal Graphics (iGPU) can be used by MacOS for high-end *compute* tasks such as hardware acceleration during video rendering and encoding. In fact, Final Cut Pro X uses Intel QuickSync technology to do precisely this. I found that if I disabled Internal Graphics in the BIOS I lose the ability to do this. But I am **not using** the iGPU connected to any display. In other words, I am running the iGPU in "headless" mode. If you want to know how to do this look under the *Troubleshooting* section where I discuss how I fixed the Intel QuickSync issue.
