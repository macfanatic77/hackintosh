# Changing "About This Mac"

The following section deals with tricks you can use to change the Apple Menu -> About This Mac, to say literally whatever you want. It does not damage or change the hackintosh in a permanent way and therefore is applying a cosmetic change.

## Changing Mac Information

This section will change the reported hardware model and production year on the main “About This Mac” page. On a Hackintosh this page will usually report the Model you have selected in your SMBIOS settings, but you might want it to say something different. 

1) Open a Finder window and navigate to your home User folder (the one containing your Documents, Pictures, Movies, etc.)

2) Inside that, open the Library folder. If you cannot see it, it may be hidden. You can find it with our guide. Do not confuse it with `/Library` by mistake, it must be `/Users/YOUR_USERNAME/Library`(where YOUR_USERNAME is whatever your username is in your hackintosh).

3) Inside the folder, open Preferences and look for the file entitled `com.apple.SystemProfiler.plist`.

4) Copy the file, and paste one somewhere as a backup. Then paste another copy to your Desktop to edit. Do not try to edit the file in place, it will not work.

5) Right-click the copy on your Desktop and open it with your .plist editor of choice. I use TextWrangler, which is free on the App Store.

6) In the editor, look through the file for the CPU Names entry which describes the current Model and Year of your computer. Mine originally said “iMac 27-inch, mid-2017”.

7) Edit the text in that line to report your preferred hardware name and year, or whatever text you like. Save the document and exit.

8) Drag the edited file from your Desktop back into the ~/Library/Preferences folder we got it from to overwrite the original. Authenticate with your user password if required.

9) After a reboot the changes should be visible in the “About This Mac” window.

To revert, simply replace your backup of the original file, overwriting your edited .plist.


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
