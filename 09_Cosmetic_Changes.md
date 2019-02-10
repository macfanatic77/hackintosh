# How to Change “About This Mac” CPU Information

1.) Open a terminal and use the following commands:
Remember in order to enable these commands, SIP has to be deactivated. Load the config.plist file on your EFI partition, and navigate to the *Rt Variables* section and set the `CsrConfig` to `0x67` (SIP inactive).  This way you can modify the Mac OS system files. After you are done, remember to change the SIP back to the ON (`0x0`) position.

```
cp /System/Library/PrivateFrameworks/AppleSystemInfo.framework/Versions/A/Resources/English.lproj/AppleSystemInfo.strings ~/Desktop/
sudo mv /System/Library/PrivateFrameworks/AppleSystemInfo.framework/Versions/A/Resources/English.lproj/AppleSystemInfo.strings /System/Library/PrivateFrameworks/AppleSystemInfo.framework/Versions/A/Resources/English.lproj/AppleSystemInfo.strings-Backup
```

2.) Open “AppleSystemInfo.strings” on your Desktop with TextWrangler and change
```
<key>UnknownCPUKind</key>
<string>Unknown</string>
```
to what ever you want, in my case I chose;

```
<key>UnknownCPUKind</key>
<string>4.9 GHz Intel Core i7-8700k</string>
```
Save “AppleSystemInfo.strings”

3.) Run the following terminal commands:Code:
```
sudo codesign -f -s - ~/Desktop/AppleSystemInfo.strings

sudo cp ~/Desktop/AppleSystemInfo.strings /System/Library/PrivateFrameworks/AppleSystemInfo.framework/Versions/A/Resources/English.lproj/
```
and reboot your system.

4.) Open your config.plist with Clover Configurator and in Section “CPU” set “Type” to “Unknown”. Save the config.plist and reboot.
