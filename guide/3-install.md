<img align="right" src="https://github.com/new-WoA-Raphael/woa-raphael/blob/main/media/raphaelbutnotass.png" width="350" alt="Windows 11 running on a Redmi K20 Pro">

# Running Windows on the Xiaomi Mi 9T Pro / Redmi K20 Pro

## Installing Windows

### Prerequisites
- [Modified TWRP](https://github.com/new-WoA-Raphael/woa-raphael/releases/download/Files/modded-twrp-raphael.img)

- [Windows on ARM image](https://arkt-7.github.io/woawin/)

- [Drivers](https://github.com/new-WoA-Raphael/woa-raphael/releases/tag/Drivers)

- [UEFI image](https://github.com/new-WoA-Raphael/woa-raphael/releases/tag/UEFI)

### Boot into the modified TWRP
> While in fastboot mode, replace `path\to\modded-twrp-raphael.img` with the actual path of the image
```cmd
fastboot boot path\to\modded-twrp-raphael.img
```

#### Execute the msc script
> If it asks you to run it once again, do so
```cmd
adb shell msc
```

### Diskpart
> [!WARNING]
> DO NOT ERASE, CREATE OR OTHERWISE MODIFY ANY PARTITION WHILE IN DISKPART!!!! THIS CAN ERASE ALL OF YOUR UFS OR PREVENT YOU FROM BOOTING TO FASTBOOT!!!! THIS MEANS THAT YOUR DEVICE WILL BE PERMANENTLY BRICKED WITH NO SOLUTION! (except for flashing it with [EDL](edl.md), which will erase all your data)

```cmd
diskpart
```

#### Select the Windows volume of the phone
> Use `list volume` to find it, replace `$` with the actual number of **WINRAPHAEL**
```diskpart
select volume $
```

#### Assign the letter X
```diskpart
assign letter x
```

#### Select the ESP volume of the phone
> Use `list volume` to find it, replace `$` with the actual number of **ESPRAPHAEL**
```diskpart
select volume $
```

#### Assign the letter Y
```diskpart
assign letter y
```

#### Exit diskpart
```diskpart
exit
```

### Installing Windows
> [!Important]
> Do not install, or update to, Windows 11 25H2 26200.7XXX or higher! You will not be able to boot into this build due to a BSoD issue!

> Replace `path\to\install.esd` with the actual path of install.esd (it may also be named install.wim or 22631.2861.XXXXXXX.esd)

```cmd
dism /apply-image /ImageFile:path\to\install.esd /index:6 /ApplyDir:X:\
```

> If you get `Error 87`, check the index of your image with `dism /get-imageinfo /ImageFile:path\to\install.esd`, then replace `index:6` with the actual index number of **Windows 11 Pro** in your image

### Copying your boot.img into Windows
- Drag and drop the **rooted_boot.img** from the **platform-tools** folder into the **WINRAPHAEL** disk in Windows Explorer, then rename it to **boot.img**.

### Installing Drivers
- Unpack the driver archive, then open the `OfflineUpdater.cmd` file (if an error shows up, run `OfflineUpdaterFix.cmd` instead)

> If it asks you to enter a letter, enter the drive letter of **WINRAPHAEL** (which should be **X**), then press enter

#### Create Windows bootloader files
> If any error shows up, such as "Failure when attempting to copy boot files", open `diskpart` again and assign any new letter to **ESPRAPHAEL**, then replace the letter `Y` in the next command with the letter that you just added.
```cmd
bcdboot X:\Windows /s Y: /f UEFI
```

#### Remove the drive letter for ESP
> If this does not work, ignore it and skip to the next command. This phantom drive will disappear the next time you reboot your PC.
```cmd
mountvol y: /d
```

### Reboot to fastboot
```cmd
adb reboot bootloader
```

#### Boot into the UEFI
> Replace `path\to\raphael-uefi.img` with the actual path of the UEFI image
```cmd
fastboot boot path\to\raphael-uefi.img
```

### Reboot to Android
Your device should reboot by itself after +- 10 minutes of waiting, after which you will be booted into Android, for the last step.

## [Last step: Setting up dualboot](/guide/4-dualboot.md)




















