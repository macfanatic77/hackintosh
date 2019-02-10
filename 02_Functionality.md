# Functionality

## What Works

1. Ethernet
2. Audio
3. APFS
4. Sleep/Wake
5. Headless iGPU with Native Support for Quicklook, Preview, JPG, Final Cut Pro X
6. All USB ports functioning at USB 3 speeds
7. iServices (iMessage, Appstore, Facetime, Handoff, Continuity)
8. WiFi
9. Bluetooth (via Broadcomm PCIe card)
10. Unlock and Apple Watch
11. Air Drop
12. Apple Music
13. Power Nap

# What Doesn’t Work

1. USB-c ports — Don’t work. I believe its because USB-C ports in my motherboard are run by an ASMedia controller and all the support I am finding is for Intel, which support the other USB 2 and 3 ports.
2. iGPU HDMI or DVI output during installation. Seems to be a common issue and it doesn’t matter to me since I installed via Vega Card.
3. Built-in WiFi — Didn’t work (MacOS didn’t even recognize that the system had WiFi).
4. Onboard Bluetooth — Recognized by MacOS, but was acting weird and not connecting to devices, until I permanently disabled the port on the motherboard and enabled the USB port for the Bluetooth from my PCIe card.
5. Netflix DRM in Safari - For some bizarre reason it works on Chrome.

## Haven’t Tested

1. Thunderbolt 3 - The header is present on this motherboard, and I have to purchase a Titan Ridge Card to test it out. People say it works, so I will report back when I test it out.
2. FileVault — I did not install some of the initial drivers required for this.
