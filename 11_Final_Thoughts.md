# Final Thoughts and Updates

## Overclocking
I have managed to overclock this CPU from its stock 3.4 GHz speed to 4.9 GHz. I was quite conservative in my overclock because I am not a gamer, and the purpose of my overclock was to leverage the power of the 8700K processor with its unlocked cores. I did not **delid** the CPU to try to improve its thermals, tried to see how far I could push the CPU without generating an insane amount of heat.

One good thing I had going in my favor was the NZXT Kraken x62 AIO Water cooler which is known to be excellent. It did, in fact, deliver.

One of the challenges of overclocking is that most of the software to monitor temperature, core frequencies, stability etc are designed for the PC and not for the Mac. Therefore, I installed Windows 10 on my system first, and played around until I could get good stable and acceptable thermals with insane levels of stress. This may have been overkill but my reasoning was that if I could get it stable with these unrealistic workloads, it should be stable with anything else that I might do.

I used 3 main Windows software:
1. **Prime 95 (newest)** to do Thermal testing - Using small FFT preset : This delivers about 130% TDP to the CPU causing an insane amount of heat generation due to the use of AVX instruction set. During this test, my system was stable at my final voltage, and my max temperature was about 92C. 
2. **AIDA 64** : This was my stability tester. I ran this overnight.
3. **OCCT** : This was my second stability tester. I ran this overnight.
4. **CoreTemp and CPU-Z** : These were my software for CPU and Temperature monitoring.

I was able to get a stable Windows 10 overclock at 5 GHz with 1.32V. One of the things that I learned unfortunately is that a Windows overclock is not the same as a MacOS overclock. Once I had the system running on my Mac, I noticed that during Final Cut Pro or other graphics intensive software the system was not stable. Thus I had to repeat the overclock to make some fine adjustments. This required looking for Mac software to do the same, and there isn't a whole lot.

1. **Prime 95** : Fortunately, Prime 95 is also available for the Mac. Remember that this comes in two flavors. Everything upto V26.6 does **NOT** use AVX instructions and the newer versions do. This makes a huge difference. AVX instructions put a substantial (almost unrealistic) burden on your CPU and generate massive amounts of heat. So I used both versions of Prime 95 for testing. The non-AVX (pre-26.6) version was used for stability testing in the **Blend** mode. The AVX-based (newer) version was used for thermal testing. 
2. **Intel Power Gadget** : This was the Mac software that I used to monitor my CPU core voltage, CPU load and frequency.
3. **HWMonitor/HWsensors/iStat** : These Mac software was used to monitor CPU temperatures.

With this, I was able to find that the Mac was stable at slightly lower frequency. I had to downclock the CPU to 4.9 GHz and change the Core Voltage to 1.3V and I was able to get as stable a Mac system as I had with my PC. My peak CPU temperature never exceeded 80C on the Mac system.

# Updates

02/08/2019 : Updated to **Mojave 10.14.3 Supplemental Update** without event.
