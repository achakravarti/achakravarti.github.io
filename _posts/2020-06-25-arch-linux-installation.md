---
layout: post
title: Arch Linux Installation
subtitle: Custom installation of Arch Linux
tags: [arch linux, installation]
comments: false
---


## Prepare live USB

Download the ISO from <https://www.archlinux.org/download/> and prepare live USB
with `dd`.
```
dd bs=4M if=/path/to/archlinux.iso of=/dev/sdx status=progress && sync
```

## Boot into live environment
  * Check if installation supports EFI: `ls /sys/firmware/efi/efivars`. There
    should be a listing with no errors.
  * Update the system clock: `timedatectl set-ntp true`.
  * Select closest mirror list. `nano /etc/pacman.d/mirrorlist` and move the
    entry for the closest mirror list to the top of the file.
  
## Partition disks
I have two disks: one SSD and one conventional. As is a popular approach now, I
chose to mount `/` on the SSD and `/home` on the conventional hard disk. On my
system, running `lsblk` shows the SSD as `/dev/sdb` and the conventional hard
disk as `/dev/sda`.

  * `gdisk /dev/sdb`: remove existing partitions (option o); create new
    partition table (option n); set first partition to size +512M as type EF00 
    (EFI System) and the second partition to the default as type 8304 
    (Linux x86-64 root); verify (option p) and write changes (option w).
  * `gdisk /dev/sda`: repeat as above, creating a swap partition (code 8200) 
    equal to RAM size, and the remainder as the home partition (code 8302).

## Format partitions
```
mkfs.fat -F32 /dev/sdb1
mkfs.ext4 /dev/sdb2
mkswap /dev/sda1
swapon /dev/sda1
mkfs.ext4 /dev/sda2
```

## Mount file systems
```
mount /dev/sdb2 /mnt
mkdir /mnt/boot
mount /dev/sdb1 /mnt/boot
mount /dev/sda2 /home
```

## Install Arch Linux
```
pacstap /mnt base linux linux-firmware nano
genfstab -U /mnt >> /mnt/etc/fstab
```


## Chroot into new installation
`arch-chroot /mnt`

TODO

References:
  * <https://wiki.archlinux.org/index.php/installation_guide>
  * <https://medium.com/@mudrii/arch-linux-installation-on-hw-with-i3-windows-manager-part-1-5ef9751a0be>
  * <https://www.rodsbooks.com/gdisk/walkthrough.html>
  * <https://itsfoss.com/install-arch-linux/>

