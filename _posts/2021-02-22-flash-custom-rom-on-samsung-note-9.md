---
layout: post
title: How to flash a custom ROM on a Samsung Note 9
tags: [android]
---

Learn how to install a custom ROM on a Samsung Note 9.

## Pre-requisites

1. Make sure your computer has `adb`. Setup instructions can be found [here](https://wiki.lineageos.org/adb_fastboot_guide.html).
2. Enable [USB debugging](https://wiki.lineageos.org/adb_fastboot_guide.html#setting-up-adb) on your device.

## Flash TWRP recovery

### Preparing for installation

Samsung devices come with a unique boot mode called “Download mode”, which is very similar to “Fastboot mode” on some devices with unlocked bootloaders. Heimdall is a cross-platform, open-source tool for interfacing with Download mode on Samsung devices. The preferred method of installing a custom recovery is through Download Mode – rooting the stock firmware is neither necessary nor required.


1. Enable Developer Options by pressing the “Build Number” option in the “Settings” app within the “About” menu
  * From within the Developer options menu, enable OEM unlock.
2. Download and install the appropriate version of the Heimdall suite for your machine’s OS
  * Linux: Extract the Heimdall suite zip and take note of the new directory containing heimdall. Now copy heimdall into a directory in $PATH, a common one on most distros will be /usr/local/bin. For example cp heimdall /usr/local/bin. You can verify Heimdall is functioning by opening a Terminal and running heimdall version.
  * macOS: Mount the Heimdall suite DMG. Now drag heimdall down into the /usr/local/bin symlink provided in the DMG. You can verify Heimdall is functioning by opening a Terminal and running heimdall version.
    * OR run `sudo cp /Volumes/Heimdall/heimdall /usr/local/bin/` from a terminal
3. Power off the device, and boot it into download mode:
  * With the device powered off, hold Volume Down + Bixby.
  * After a few seconds, plug in the USB-C cable.
  * Now, click "Volume Up" to continue.
4. On your machine, open a Terminal (Linux or macOS) window, and type:
   ```heimdall print-pit```
5. If the device reboots that indicates that Heimdall is installed and working properly. If it does not, please refollow these instructions to verify steps weren’t missed, try a different USB cable, and a different USB port.

### Install TWRP

1. Download the latest version of [TWRP](https://twrp.me/) (Note: Download the *.img file - for example `twrp-3.5.0_9-1-crownlte.img`)
2. Power off the device, and boot it into download mode:
  * With the device powered off, hold `Volume Down` + `Bixby`.
  * After a few seconds, plug in the USB-C cable.
  * Now, click `Volume Up` to continue.
3. On your machine, open a Terminal window, and type:
  ```
  heimdall flash --RECOVERY <recovery_filename>.img --no-reboot
  ```
4. A blue transfer bar will appear on the device showing the recovery image being flashed.
5. Unplug the USB cable from your device.
6. Manually reboot into recovery:
  * Hold `Volume Down` + `Power key` until the phone turns off
  * Immediately press `Volume Up` + `Bixby` + `Power` until the TWP recovery screen appears

## Flash a Custom ROM

1. Format data using TWRP
2. Download required ROM, GApps and Magisk as required
3. On TWRP, go to `Advanced` -> `ADB Sideload` and then `Swipe to Start Sideload`
4. On your machine, open a Terminal window, and type:
  ```adb sideload romFilename.zip```
5. Repeat step four again for any additional add-ons (Magisk, GApps etc)
6. Reboot system


## Update Custom ROM

1. Flash ROM
2. Reboot

Do not wipe anything!


## References

* [Lineage Wiki](https://wiki.lineageos.org/devices/crownlte/install)
