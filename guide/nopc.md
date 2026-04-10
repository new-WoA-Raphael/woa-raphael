<img align="right" src="https://github.com/new-WoA-Raphael/woa-raphael/blob/main/media/raphaelbutnotass.png" width="350" alt="Windows 11 running on a Redmi K20 Pro">

# Running Windows on the Xiaomi Mi 9T Pro / Redmi K20 Pro

## Installing Windows without a PC

### Prerequisites
- A rooted Xiaomi Mi 9T Pro / Redmi K20 Pro

- [Modified TWRP](https://github.com/new-WoA-Raphael/woa-raphael/releases/download/Files/modded-twrp-raphael.img)

- [Raphael WinInstaller](https://github.com/new-WoA-Raphael/woa-raphael/releases/download/Files/RaphaelWinInstaller.zip)

- [Windows on ARM image](https://arkt-7.github.io/woawin/)

- [WOA Helper app](https://github.com/n00b69/woa-helper/releases/tag/APK)

- Optional: a USB stick

### Notes
> [!WARNING]  
> All your data will be erased! Back up now if needed.
> 
> DO NOT REBOOT YOUR PHONE if you think you made a mistake, ask for help in the [Telegram chat](https://t.me/woaraphael).

> [!Important]
> This guide assumes you have already unlocked your bootloader and are already rooted, if this is not the case, you'll still need a PC to do that.

### Flash the modified TWRP
> If you have a custom recovery installed, you can install the modified TWRP through there instead
- Download the modified TWRP, rename it to **TWRP.img**, and leave in the `Download` folder **of your internal storage**.
- Download **Termux**, open it, and grant it root access.
- Run the below command in Termux to flash TWRP:
```cmd
su -c dd if=/sdcard/Download/TWRP.img of=/dev/block/by-name/recovery
```
- Run the below command in Termux to reboot into TWRP:
```cmd
su -c reboot recovery
```

### Checking if decryption works
- Once TWRP boots, enter your password if it asks you to.
- Open the file explorer in TWRP, then navigate to your internal storage.
> If your files are not there and your internal storage is displayed as **(0MB)**, there will be some additional steps which will be explained later in this guide.

#### Opening TWRP terminal
- Once booted into TWRP press the **Advanced** button on the bottom right of the screen, then press **Terminal**.
- Run all future commands in this terminal

#### Resizing the partition table
```cmd
sgdisk --resize-table 64 /dev/block/sda
```

#### Unmount data
> Ignore any possible errors and continue
```cmd
umount /dev/block/by-name/userdata
```

### Fixing the GPT
> If you do not do this, Windows may break your device
```cmd
fixgpt
```

#### Preparing for partitioning
> [!Note]
> If at any time **parted** asks you if you want to continue, or if you want to cancel something, type **yes** or **ignore**
>
> Be very careful with writing in parted. It is not possible to use backspace to fix a typo. If at any point you make any mistakes, use the **print** command to check if any changes were made
```cmd
parted /dev/block/sda
```

#### Printing the current partition table
> Parted will print the list of partitions, userdata should be the last partition in the list.
```cmd
print
```

#### Removing userdata
> Replace **$** with the number of the **userdata** partition, which should be **30**
```cmd
rm $
```

#### Recreating userdata
> Replace **2080MB** with the former start value of **userdata** which we just deleted
>
> Replace **64GB** with the end value you want **userdata** to have. In this example your available usable space in Android will be 64GB-2080MB = **62GB**
```cmd
mkpart userdata ext4 2080MB 64GB
```

#### Creating ESP partition
> Replace **64GB** with the end value of **userdata**
>
> Replace **64.35GB** with the value you used before, adding **0.35GB** to it
```cmd
mkpart esp fat32 64GB 64.35GB
```

#### Creating Windows partition
> Replace **64.35GB** with the end value of **esp**
```cmd
mkpart win ntfs 64.35GB 126GB
```

#### Making ESP bootable
> Use `print` to see all partitions. Replace "$" with your ESP partition number, which should be **31**
```cmd
set $ esp on
```

#### Exit parted
```cmd
quit
```

### Formatting data and rebooting
- Format all data in TWRP, or Android will not boot.
- ( Go to Wipe > Format data > type `yes` ).
- Press the reboot button to reboot into Android.
> [!Note]
> If Android does not start after +- 10 minutes, reboot back into stock recovery and perform a factory reset there.

### Preparing necessary files
- Download the Windows image and make sure it remains in the `Download` folder of your **internal storage**.
> [!Important]
> For performance reasons, it is recommended to use Windows 11 24H2 (builds that start with 261XX, such as 26100.2454)

> If TWRP is not able to read/decrypt your internal storage, create a folder on your **USB stick** named `WOA` and put the **.esd** file in there
>
> Alternatively, find another TWRP image online that can actually decrypt and use it instead
- Download **WinInstaller.zip** and keep it in the `Download` folder as well.
- Download and install the [WOA Helper app](https://github.com/new-WoA-Raphael/woa-helper/releases/tag/APK), then open it and grant it root access. Do not do anything else inside the app yet.

#### Booting into TWRP
- Press the `Reboot to Recovery` button in the top right of Magisk.
- Once booted into TWRP, enter your password if it asks you to.

### Flashing WinInstaller
- In TWRP, select **Install** and then locate **WinInstaller.zip** and flash it.
- Once you're given the option to reboot, do so.
> [!Note]
> Wait until all processes complete and your device boots into Windows. This will take around 15-20 minutes.

> [!Tip]
> If you wish to skip the Microsoft Account login, press the **I don't have internet** button in the WiFi page, then when prompted, press the **Continue with limited setup** button.

#### Booting to Android
- Run the **Android** shortcut on your desktop (you can also pin it to your start menu / taskbar for ease of access).

#### Booting to Windows
> If it says "UEFI NOT FOUND", download the [UEFI image](https://github.com/new-WoA-Raphael/woa-raphael/releases/tag/UEFI) and place it in the `UEFI` folder in your internal storage.
- Press **QUICKBOOT TO WINDOWS** inside the WOA Helper app, or use the newly created toggle in your quick settings panel.

## Finished!

























