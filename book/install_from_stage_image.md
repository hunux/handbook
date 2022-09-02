# Hunux GNU/Linux Installation from staged image

Get a Hunux GNU/Linux image [here](https://sourceforge.net/projects/hunux/)

## Preparing partition

Create a partition to install Hunux GNU/Linux on.

```
# cfdisk
# mkswap /dev/sda1
# mkfs.ext4 -L Hunux /dev/sda2
```
Create a directory to mount the created partition then mount it.

```
# mkdir /mnt/hunux
# mount /dev/sda2 /mnt/hunux
```

## Extract Hunux image

Extract Hunux image to the mounted location.

```
# tar xvJpf hunux-rootfs.txz -C /mnt/hunux
```

## Enter chroot

Chroot into the extracted hunux image.

```
# mount -v --bind /dev /mnt/hunux/dev
# mount -vt devpts devpts /mnt/hunux/dev/pts -o gid=5,mode=620
# mount -vt proc proc /mnt/hunux/proc
# mount -vt sysfs sysfs /mnt/hunux/sys
# mount -vt tmpfs tmpfs /mnt/hunux/run
# mkdir -pv /mnt/hunux/$(readlink /mnt/hunux/dev/shm)
# cp -L /etc/resolv.conf /mnt/hunux/etc/
# chroot /mnt/hunux /bin/bash
```

## Configuring system

Configure the system's hostname, timezone, clock, font, keymap and daemon:

```
# vim /etc/rc.conf
```

Configure /etc/fstab:

```
# vim /etc/fstab
```

Configure locales:

```
# vim /etc/locales
```
uncomment required locales, then run:
```
# genlocales
```

Configure root password:
```
# passwd
```

Add a user:
```
# useradd -m -G users,wheel,audio,video -s /bin/bash <your user>
```
then create a password for your user:
```
# passwd <your user>
```

Configure the bootloader, GNU `grub`:
```
# grub-install /dev/sdX
# grub-mkconfig -o /boot/grub/grub.cfg
```
> Note: replace 'X' with your partition drive

Exit chroot environment:
```
# exit
```

Unmount hunux partition you mounted before:
```
# umount -v /mnt/hunux/dev/pts
# umount -v /mnt/hunux/dev
# umount -v /mnt/hunux/run
# umount -v /mnt/hunux/proc
# umount -v /mnt/hunux/sys
# umount /mnt/hunux
```

You can restart your machine now, Hunux GNU/Linux should be bootable.
```
# reboot
```
