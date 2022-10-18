# MagiskHide Props Config
## By Didgeridoohan @ XDA Developers


[Support Thread @ XDA](https://forum.xda-developers.com/apps/magisk/module-magiskhide-props-config-t3789228)


## Installation
Install through the Magisk Manager Downloads section. Or, download the zip from the Manager or the [module support thread](https://forum.xda-developers.com/apps/magisk/module-magiskhide-props-config-t3789228), and install through the Magisk Manager -> Modules, or from recovery.

The current release is always attached to the OP of the [module support thread](https://forum.xda-developers.com/apps/magisk/module-magiskhide-props-config-t3789228). Any previous releases can be found on [GitHub](https://github.com/Magisk-Modules-Repo/MagiskHide-Props-Config/releases).


## Usage
After installing the module and rebooting, run the command `props` (as su) in a terminal emulator (you can find a one on [F-Droid](https://f-droid.org/) or in the [Play Store](https://play.google.com/store/apps)), and follow the instructions to set your desired options.
```
su
props
```
You can also run the command with options. Use -h for details.


## Spoofing device's fingerprint to pass the ctsProfile check
If your device can't pass SafetyNet fully, the CTS profile check fails while basic integrity passes, that means MagiskHide is working on your device but Google doesn't recognise your device as being certified.

This might be because your device simply hasn't been certified or that the ROM you are using on your device isn't recognised by Google (because it's a custom ROM). 

To fix this, you can use a known working fingerprint (one that has been certified by Google), usually from a stock ROM/firmware/factory image, and replace your device's current fingerprint with this. You can also use a fingerprint from another device, but this will change how your device is perceived.

There are a few pre-configured certified fingerprints available in the module, just in case you can't get a hold of one for your device. If you have a working fingerprint that could be added to the list, or an updated one for one already on there, please post that in the [module support thread](https://forum.xda-developers.com/apps/magisk/module-magiskhide-props-config-t3789228) toghether with device details.

### Can I use any fingerprint?
It's possible to use any fingerprint that's certified for your device. It doesn't have to match, either device or Android version. If you don't use a fingerprint for your device, the device might be percieved as the device that the fingerprint belongs to, in certain situations (Play Store, etc). The Android version doesn't matter much, and if you're using a ROM with an Android version much newer than what is officially available for your device, you are going to have to use an older fingerprint if you want to use the one for your device. But, like already stated, that doesn't really matter.

### Finding a certified fingerprint
#### The getprop method
If you don't want to use one of the provided fingerprints, you can get one for your device by running the getprop command below on a stock ROM/firmware/factory image that fully passes SafetyNet.
```
getprop ro.build.fingerprint
```
If you're already on a custom ROM that can't pass the CTS profile check, this might not be an option... Head over to your device's forum and ask for help. If someone can run the getprop command on their device for you, you're good to go. Or, you can try the other method described below.

#### The stock ROM/firmware/factory image method
Another way to find a certified fingerprint is to download a stock ROM/firmware/factory image for your device and extract the fingerprint from there.

You can find the file to download in your device's forum on XDA Developers (either as a firmware file, a proper stock ROM, or in the development section as a debloated stock ROM), from the manufacturer's website, or elsewhere on the great interweb (just remember to be careful when downloading unknown files, it's dangerous to go alone!).

Once you have the file downloaded, there are several different ways that the fingerprint can be found. In all cases you'll have to access the file somehow, and in most cases it's just a matter of unpackaging it. After that it depends on how the package is constructed.

- Sometimes there'll be a build.prop file directly in the zip/package. You'll likely find the fingerprint in there.
- For some devices you'll have to unpackage the system.img to get to the build.prop file. On Windows, you can use something like [this tool](https://forum.xda-developers.com/showpost.php?p=57742855&postcount=42). You'll also find more info in the [main thread for that post](https://forum.xda-developers.com/android/software-hacking/how-to-conver-lollipop-dat-files-to-t2978952).
- Other times you'll find the fingerprint in META-INF\com\google\android\updater-script. Look for "Target:" and you'll likely find the fingerprint there.
- Etc... Experiment, the fingerprint will be in there somewhere.

Take a look below for an example of what a device fingerprint looks like.

### Custom fingerprints list
You can add your own fingerprint to the list by placing a file, named `printslist`, in the root of your internal storage with the fingerprint. It needs to be formated as follows:`device name=fingerprint`.
Here's an example:
```
Google Nexus 6=google/shamu/shamu:7.1.1/N8I11B/4171878:user/release-keys
```

### I still can't pass the ctsProfile check
If you've picked a certified fingerprint from the provided list, or you're using a fingerprint that you know is certified but still can't pass the ctsProfile check, try one or more of the following:
- First, do you pass basicIntegrity? If you don't, there's something else going on that this module can't help you with. Take a look under "Miscellaneous MagiskHide issues" below.
- Go into the script options and move the execution of the boot script to post-fs-data. See "Boot stage" below.
- Try a different fingerprint (pick one from the provided list).
- Some ROMs will just not be able to pass the ctsProfile check, if they contain signs of a rooted/modified device that Magisk can't hide. Check in your ROM thread or with the creator/developer.
- You might have remnants of previous tampering on your device. A clean install of your system may be required.
- If you can't get things working, and want help, make sure to provide logs and details. See "Logs, etc" below.


## Keeping your device "certified"
If you're using a custom ROM, the chances of it being [perceived as uncertified by Google](https://www.xda-developers.com/google-blocks-gapps-uncertified-devices-custom-rom-whitelist/) are pretty high. If your ROM has a build date later than March 16 2018, this might mean that you can't even log into your Google account or use Gapps without [whitelisting your device with Google](https://www.google.com/android/uncertified/) first.

Magisk, and this module, can help with that.

Before setting up your device, install Magisk, this module and use the configuration file described below to pass the ctsProfile check. This should make your device be perceived as certified by Google and you can log into your Google account and use your device without having to whitelist it. Check [here](https://github.com/Magisk-Modules-Repo/MagiskHide-Props-Config/blob/master/common/prints.sh) for usable fingerprints (only use the part to the right of the equal sign).

If you're having issues getting your device certified, take a look in the Magisk troubleshooting guide linked below.


## Current fingerprints list version
The fingerprints list will update without the need to update the entire module. Keep an eye on the [module support thread](https://forum.xda-developers.com/apps/magisk/module-magiskhide-props-config-t3789228) for info.

Just run the `props` command and the list will be updated automatically. Use the -nw option to disable or disable it completely in the script settings (see below). If you've disabled the this setting you can update the list manually in the `Edit device fingerprint` menu.

**_Current fingerprints list version - v18_**


## Improved root hiding - Editing build.prop and default.prop
Some apps and services look at the actual files, rather than the set prop values. With this module feature you can make sure that the actual prop in build.prop and default.prop is changed to match whatever value the prop has been set to by either MagiskHide or the module. If there's a prop value set by the module (see below), that value takes precedence.


## Set/reset MagiskHide Sensitive props
By default, if MagiskHide detects certain sensitive prop values they will be changed to known safe values. These are currently:
- ro.debuggable (set to "0" by MagiskHide - sensitive value is "1")
- ro.secure (set to "1" by MagiskHide - sensitive value is "0")
- ro.build.type (set to "user" by MagiskHide - sensitive value is "userdebug")
- ro.build.tags (set to "release-keys" by MagiskHide - sensitive value is "test-keys" or "dev-keys")
- ro.build.selinux (set to "0" by MagiskHide - sensitive value is "1")

If, for some reason, you need one or more of these to be kept as their original value (one example would be resetting ro.build.type to userdebug since some ROMs need this to work properly), you can reset to the original value with this module. Keep in mind that this might trigger some apps looking for these prop values as a sign of your device being rooted.


## Change/set custom prop values
It's quite easy to change prop values with Magisk. With this module it's even easier. Just enter the prop you want to change and the new value and the module does the rest, nice and systemless. Any changes that you've previously done directly to build.prop, default.prop, etc, you can now do with this module instead.


## Prop script settings
There are a couple of persistent options that you can set for the `props` script. These are currently "Boot stage", "Script colours" and "Fingerprints list check". The options are found under "Script settings" when running the `props` script. The settings menu can also be opened by using the -s option when running the `props` script (use -h for details).

### Boot stage
It's possible to move the execution of the boot script from the default late_start service to post-fs-data.d. This is required for the SafetyNet fix and custom props to work on some ROM/device combinations (known: LineageOS 15.1). The reason late_start service is default is that it's best to try to keep the number of scripts running during post-fs-data mode as low as possible, but if late_start service doesn't work, it needs to run in post-fs-data instead.

### Script colours
This option will disable or enable colours for the `props` script.

### Fingerprints list check
This option will disable or enable the automatic updating of the fingerprints list when the `props` script starts. If the fingerprints list check is disabled, the list can be manually updated from within the script, under the `Edit device fingerprint` menu, or with the -f option when running the `props` script (use -h for details).


## Configuration file
You can use a configuration file to set your desired options, rather than running the `props` command. Download the [settings file](https://raw.githubusercontent.com/Magisk-Modules-Repo/MagiskHide-Props-Config/master/common/propsconf_conf) or extract it from the module zip (in the common folder), fill in the desired options (follow the instructions in the file), place it in /cache (or /data/cache if you're using an A/B device) and reboot. This can also be done directly at the first install (through Manager or recovery) and even on a brand new clean install of Magisk, before even rebooting your device. Instant settings.


## Miscellaneous MagiskHide issues
If you're having issues passing SafetyNet, getting your device certified, or otherwise getting MagiskHide to work, take a look in the [Magisk and MagiskHide Installation and Troubleshooting Guide](https://www.didgeridoohan.com/magisk). Lots of good info there (if I may say so myself)...

But first: have you tried turning it off and on again? Toggling MagiskHide off and on usually works if MagiskHide has stopped working after an update of Magisk or your ROM.


## Issues, support, etc
If you have questions, suggestions or are experiencing some kind of issue, visit the [module support thread](https://forum.xda-developers.com/apps/magisk/module-magiskhide-props-config-t3789228) @ XDA.

### Props don't seem to set properly
If it seems like props you're trying to set with the module don't get set properly (ctsProfile still doesn't pass, custom props don't work, etc), go into the script options and change the execution of the boot script to post-fs-data. See "Boot stage" above.

### Device issues because of the module
In case of issues, if you've set a prop value that doesn't work on your device causing it not to boot, etc, don't worry. There are options. You can follow the advice in the [Magisk troubleshooting guide](https://www.didgeridoohan.com/magisk/Magisk#hn_Module_causing_issues_Magisk_functionality_bootloop_loss_of_root_etc) to remove or disable the module, or you can use the module's built-in options to reset all module settings to the defaults.

Place a file named `reset_mhpc` in /cache (or /data/cache on A/B devices) and reboot.

It is possible to use this in combination with the configuration file described above to keep device fingerprint or any other settings intact past the reset. Just make sure to remove any custom props that might have been causing issues from the configuration file.

### Logs, etc
In case of issues, please provide the different log files, found in /cache (or /data/cache for A/B devices), together with a detailed description of your problem. The logs available could include "magisk.log" and any files starting with "propsconf". Providing the output from terminal might also be useful.

If you have the latest beta release of Magisk installed, the "magisk_debug.log" is also useful. If there's no new beta released, there's always a beta version of the latest stable Magisk release (the only difference is the more verbose logging), so that you can collect the debug log.


## Source
[GitHub](https://github.com/Magisk-Modules-Repo/MagiskHide-Props-Config)


## Credits
@topjohnwu @ XDA Developers, for Magisk  
@Zackptg5, @veez21 and @jenslody @ XDA Developers, for help and inspiration


## Changelog
### v2.2.2  
- This is not the changelog you're looking for. You can go about your business. Move along.
- Fixed a bug with setting custom props where the value contains spaces.
- Added a couple of fingerprints (OnePlus 6 and Xiaomi Mi Note 2) and cleaned out a few old ones, list v 18.
- As usual, a whole bunch of script improvements that hopefully won't break anything.

### v2.2.1  
- Added a check for entering empty values for fingerprint and custom props.
- Added a command option to go directly to the settings menu. Run `props` with the -h option for details.

### v2.2.0  
- Added an option to set prop values earlier in the boot process.
- Moved module setup from post-fs-data.sh to post-fs-data.d.
- Fixed installing module on a fresh Magisk install.
- Fixed restoring the boot scripts during post-fs-data boot stage.
- Updated and added some new fingerprints (Google Pixel 2 XL, Huawei Honor 9, Samsung Galaxy J5 and Note 8, Xiaomi Mi A1, Mi Max 2 and Redmi Note 5 Pro), list v17.
- As usual, a bunch of improvements. They'll likely not harm any kittens, but might break the module.

### v2.1.6  
- Added some new fingerprints (Sony Xperia Z, Sony Xperia Z1, Xiaomi Redmi 4 Prime, Xiaomi Redmi Note 5/5 Plus), list v15.
- Very minor improvements that doesn't even deserve their own release, or changelog.

### v2.1.5  
- Show what device the currently set fingerprint is from.
- Fixed,updated and added a bunch of fingerprints, list v12.
- Minor updates and improvements.

### v2.1.4  
- Fixed improved hiding.
- Fixed using the configuration file on a clean install.
- Fixed a fault in the AE-35 Unit.
- Improved logging.
- A bunch of optimisations that will probably break stuff.

### v2.1.3  
- Reverted the function to only editing existing fingerprint props.
- Optimised setting the different props.
- Minor updates and improvements.

### v2.1.2  
- Detects and edits only existing device fingerprint props.
- Slightly optimised the boot scripts.
- New fingerprint (Motorola Moto E4), list v10.
- Minor updates and improvements.

### v2.1.1  
- Fixed transferring custom props between module updates.

### v2.1.0  
- "Improved hiding" will now also mask the device fingerprint in build.prop. Please reapply the option to set.
- Fixed colour code issues if running the script with ADB Shell. Huge shout-out to @veez21 over at XDA Developers.
- Fixed typo in A/B device detection. Thank you to @JayminSuthar over at XDA Developers.
- New fingerprints (Samsung Galaxy S4 and Sony Xperia Z3 Tablet Compact), list v8.
- Minor fixes and improvements.

### v2.0.0  
- Added a function for setting your own custom prop values.
- Added a function for setting values by configuration file. Useful if you want to make a ROM pass the ctsProfile check out of the box.
- Added a function for resetting the module by placing a specific file in /cache. Useful if a custom prop is causing issues.
- Added a function to set options for the props script. See the documentation for details.
- Added command option to not check online for new fingerprints list at start. Run `props` with the -h option for details.
- Restructured the fingerprints list menu. Sorted by manufacturer for easier access.
- New and updated fingerprints (lots), list v6.
- Minor fixes and improvements.

### v1.2.1  
- Fixed logic for checking the original prop values.
- New fingerprints (Xiaomi Mi 5, ZTE Axon 7), list v4.
- Minor fixes.

### v1.2.0  
- Now in the Magisk repo. Updated to match.
- New fingerprint (Pixel 2), list v3.
- Show info when checking for fingerprint updates.
- Minor changes and improvements.

### v1.1.0  
- New fingerprint added (Sony Xperia Z3).
- Added the ability to update the fingerprints list automatically

### v1.0.0  
- Initial release. Compatible with Magisk v15+.


## Current fingerprints list
### List v18  
- Asus Zenfone 2 Laser (6.0.1)
- Google Nexus 4 (5.1.1)
- Google Nexus 5 (6.0.1)
- Google Nexus 6 (7.1.1)
- Google Nexus 5X (8.1.0)
- Google Nexus 6P (8.1.0)
- Google Pixel (8.1.0)
- Google Pixel (P DP1)
- Google Pixel XL (8.1.0)
- Google Pixel XL (P DP1)
- Google Pixel 2 (8.1.0)
- Google Pixel 2 (P DP1)
- Google Pixel 2 XL (8.1.0)
- Google Pixel 2 XL (P DP1)
- HTC 10 (6.0.1)
- Huawei Honor 9 (8.0.0)
- Huawei Mate 10 Pro (8.0.0)
- Huawei YAL-L21 (10)
- Infinix X662 (11)
- Motorola Moto E4 (7.1.1)
- Motorola Moto G4 (7.0)
- Motorola Moto G5 (7.0)
- Motorola Moto G5 Plus (7.0)
- Motorola Moto X4 (8.0.0)
- Nvidia Shield K1 (7.0)
- OnePlus 3T (8.0.0)
- OnePlus 5T (8.0.0)
- OnePlus 6 (8.1.0)
- Samsung Galaxy A8 Plus (7.1.1)
- Samsung Galaxy Grand Prime (5.0.2)
- Samsung Galaxy J5 (7.1.1)
- Samsung Galaxy J5 Prime (7.0)
- Samsung Galaxy Note 3 (7.1.1)
- Samsung Galaxy Note 4 (6.0.1)
- Samsung Galaxy Note 5 (7.0)
- Samsung Galaxy Note 8 (8.0.0)
- Samsung Galaxy S3 Neo (4.4.4)
- Samsung Galaxy S4 (5.0.1)
- Samsung Galaxy S6 (7.0)
- Samsung Galaxy S6 Edge (7.0)
- Samsung Galaxy S7 (8.0.0)
- Samsung Galaxy S7 Edge (8.0.0)
- Samsung Galaxy S8 Plus (8.0.0)
- Samsung Galaxy S9 (8.0.0)
- Samsung Galaxy S9 Plus (8.0.0)
- Sony Xperia X (8.0.0)
- Sony Xperia X Performance (8.0.0)
- Sony Xperia XZ (8.0.0)
- Sony Xperia XZ1 Compact (8.0.0)
- Sony Xperia Z (5.1.1)
- Sony Xperia Z1 (5.1.1)
- Sony Xperia Z2 (6.0.1)
- Sony Xperia Z3 (6.0.1)
- Sony Xperia Z3 Compact (6.0.1)
- Sony Xperia Z3 Tablet Compact (6.0.1)
- Sony Xperia Z5 (7.1.1)
- Sony Xperia Z5 Compact (7.1.1)
- Sony Xperia Z5 Dual (7.1.1)
- Vodafone Smart Ultra 6 (5.1.1)
- Xiaomi Mi 3/4 (6.0.1)
- Xiaomi Mi 5/5 Pro (7.0)
- Xiaomi Mi 5S (7.0)
- Xiaomi Mi 5S Plus (6.0.1)
- Xiaomi Mi 6 (7.1.1)
- Xiaomi Mi 6 (8.0.0)
- Xiaomi Mi A1 (8.0.0)
- Xiaomi Mi Max 2 (7.1.1)
- Xiaomi Mi Note 2 (7.0)
- Xiaomi Redmi 4 Prime (6.0.1)
- Xiaomi Redmi 4X (6.0.1)
- Xiaomi Redmi Note 3 Pro (6.0.1)
- Xiaomi Redmi Note 4/4X (7.0)
- Xiaomi Redmi Note 5/5 Plus (7.1.2)
- Xiaomi Redmi Note 5 Pro (8.1.0)
- ZTE Axon 7 (7.1.1)
- ZTE Nubia Z17 (7.1.1)
- Zuk Z2 Pro (7.0)
- 
