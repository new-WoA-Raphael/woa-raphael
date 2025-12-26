<img align="right" src="https://github.com/new-WoA-Raphael/woa-raphael/blob/main/media/raphaelbutnotass.png" width="350" alt="Windows 11 running on a Redmi K20 Pro">

# Running Windows on the Xiaomi Mi 9T Pro / Redmi K20 Pro

## Partitioning your device (alternative method, if recovery does not boot)

### Prerequisites
- Unlocked bootloader & root access

- [Termux](https://play.google.com/store/apps/details?id=com.termux)

- [WOA Helper app](https://github.com/n00b69/woa-helper/releases/tag/APK)

### Notes
> [!WARNING]
> All your data will be erased! Back up now if needed.
> 
> Do not run the same command twice.
> 
> DO NOT REBOOT YOUR PHONE if you think you made a mistake, ask for help in the [Telegram chat](https://t.me/woaraphael).
> 
> Do not run all commands at once, execute them in order!

### Backing up important partitions
- Download and install the **WOA Helper** app, then open it and grant it root access.
- Click on **BACK UP BOOT.IMG** and select **ANDROID**.
- Navigate to the `WOAHelper/backups` folder in your internal storage and copy its contents to your PC.

### Setting up Termux
- Download Termux if you haven't already.
- In Termux, run the following commands:
> If there are any Y/N prompts, type Y
```cmd
apt upgrade
```
```cmd
pkg install root-repo
```
```cmd
pkg install parted
```
```cmd
pkg install gptfdisk
```
```cmd
pkg install ntfs-3g
```
```cmd
pkg install dosfstools
```
> After running the below command, accept the root access popup if you haven't already done so earlier.
```cmd
su
```

#### Resizing the partition table
```cmd
/data/data/com.termux/files/usr/bin/sgdisk --resize-table 64 /dev/block/sda
```

### Preparing for partitioning
> [!Note]
>
> If at any moment in parted you see an error prompting you to type "Yes/No" or "Ignore/Cancel", type `Yes` or `Ignore` depending on the situation to ignore the errors and continue.
>
> If you see any **udevadm** errors, you can ignore these as well.
```cmd
/data/data/com.termux/files/usr/bin/parted /dev/block/sda
```

#### Printing the current partition table
> Parted will print the list of partitions, userdata should be the last partition in the list
```cmd
print
``` 

#### Removing userdata
> Replace **$** with the number of the **userdata** partition, which should be **31**
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
> Replace **64.3GB** with the value you used before, adding **0.3GB** to it
```cmd
mkpart esp fat32 64GB 64.3GB
``` 

#### Creating Windows partition
> Replace **64.3GB** with the end value of **esp**
```cmd
mkpart win ntfs 64.3GB -0MB
``` 

#### Making ESP bootable
> Use `print` to see all partitions. Replace "$" with your ESP partition number, which should be **32**
```cmd
set $ esp on
```

#### Exit parted
```cmd
quit
```

### Fixing the GPT
> If you do not do this, Windows may break your device
- Run the below command to open gdisk in **sda** (later this command will be reused to open **sdb**, **sdc** etc.).
```cmd
/data/data/com.termux/files/usr/bin/gdisk /dev/block/sda
```
- Run the below commands (each letter is an individual command).
```cmd
x
```
```cmd
j
```
```cmd
enter (do not actually write enter)
```
```cmd
k
```
```cmd
enter (do not actually write enter)
```
```cmd
w
```
```cmd
y
```
- Repeat this process for sdb, sdc, sdd, sde, sdf, sdg etc. until it throws an error that the disk does not exist.

### Formatting Windows and ESP drives
```cmd
/data/data/com.termux/files/usr/bin/mkfs.ntfs -f /dev/block/sda33 -L WINRAPHAEL
``` 
```cmd
/data/data/com.termux/files/usr/bin/mkfs.fat -F32 -s1 /dev/block/sda32 -n ESPRAPHAEL
``` 

#### Check if Android still starts
- Just restart the phone, and see if Android still works.
- If it doesn't, boot into (stock) recovery to perform a factory reset.

## [Next step: Installing Windows](3-install.md)















