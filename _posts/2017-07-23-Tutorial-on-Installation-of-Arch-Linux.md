---
title: Tutorial on Installation of Arch Linux
tags: [IT, Linux]
redirect_from:
    - /blog/install-arch-linux/
---

Before the introduction, I'd like to redirect everyone who is willing to try Arch to install [Antergos](https://antergos.com/) or [Manjaro](https://manjaro.org/) instead, which both are derived from Arch. They are always coming top in popularity [statistics](https://distrowatch.com/popularity
) of all Linux distros. Or you can even try [Gentoo](https://www.gentoo.org/).

- Antergos: almost the same as Arch except for providing a wrapped GUI installer
- Manjaro: lower rolling speed and more friendly to beginners

Unless you're aiming to become an embedded system engineer or do low-level operating system development, it's **NONSENSE** for you to waste time at installing a system. Perhaps it will help you improve your understanding towards Linux kernel, but it will bother you when you have to install Arch once and once more.

There's no need for any elitism to install Arch manually. Remember OS is just a tool or platform for most of us. Focus on your learning or working purpose.

It's enough to install Arch only once.

## Introduction to Arch

Arch is a famous Linux distro and widely used in desktop environments throughout the world. Of course it's not as known as Ubuntu or CentOS in China since it doesn't have such a user-friendly interface, but that does not affect the fact that Arch is one of the simplest and rolling release Linux distribution in the world.

Distros like Ubuntu, Debian and so on will bring a GUI for users to install the OS with few difficulties. After your installation, the desktop environment such as Unity, Gnome or Cinnamon will be attached and users have no worries about what to do next. However, that's apparently different as for Arch.

## Why is Arch not so famous as Ubuntu or other distros

Arch is famous for its command-line installation interface as well, which stops most people who might be interested in Arch. That's right. It's difficult (or rather, takes quite a lot of time) to understand the mechanism of Linux. When you install Windows or some other distros, the only thing you should have to do during installation is to decide the partition of you HDD or SSD. But from Arch you should set up configurations from zero.

Even for some users of Arch, they just follow the guide and input commands one by one without really understanding them. I'm not saying that it's something shameful. That's convenient and constructive. [Arch Wiki](https://wiki.archlinux.org/index.php/installation_guide) has already provided a mature solution. Perhaps 80 percent or even more users have read about this article when installing Arch for the first time, including me.

## Simple guides<sup>[1]</sup> for first-timers

I believe you could get almost all of your questions to be solved at Arch Wiki. But for beginners let me simply introduce the steps of installing Arch to a Lenovo laptop. My experience is also combined with that.

**Note**: do the following in the order!

### General

To sum up, the process can be divided to the following parts:

1. Prepare USB key with Arch ISO burned
2. Partition and establish file system
3. Install basic packages including booting and desktop and make configurations

The above steps are recommended to be executed in a wired network environment.

### Prepare a USB key for Arch ISO

This is like most operating systems. First download Arch ISO from official website or mirror sites ([Tsinghua TUNA](https://mirrors.tuna.tsinghua.edu.cn/archlinux/iso/latest/) is highly recommended for students in Peking University). Do check architecture of your CPU. If your CPU is ARM, please turn to another directory for ARM specialize ISO.

In Windows, it's recommended to burn ISO into USB stick by [Rufus](https://rufus.akeo.ie/) which is an open-source project. As for Mac or Linux, just use `dd` command<sup>[2]</sup> as root.

```shell
dd bs=4M if=/path/to/archlinux.iso of=/dev/sdx status=progress && sync
```

Use `sync` to ensure burning process since `dd` command is non-blocking.

### BIOS & MBR or UEFI & GPT

Well this is not so important in fact. It's just better to know in which way your system is booted.

MBR is a traditional form of partition table, while GPT is something more advanced. I think most of laptops are perhaps still managing their HDDs with MBR rather than GPT, so it perhaps doesn't matter so much. By the way, BIOS booting is along with MBR for most of the times, but there're also some laptops that using BIOS with GPT.

As for UEFI users, it will perhaps be more complicated for you to install in the following steps, but do not worry. Just a little more steps.

### Boot from USB

Now let's come back to installing Arch. As you install Windows to your disk, select bootable device by pressing Fx (something depending on your laptop) when the laptop manufacturer logo is displayed on screen. Be aware that for UEFI + GPT users it's necessary to configure the BIOS to turn UEFI booting on.

### Partitioning

It's easy before you get into the command-line interface.

For an initial HDD or SSD, it's usually formatted in FAT32 with no partitioning. For original Windows users, NTFS or FAT32 are widely used. In this step, assuming that you want to wipe all other OSs and install Arch only.

**Note** that here you have the chance to transfer from BIOS + MBR to UEFI + GPT. Just use `gdisk`. If not, use `fdisk`.

First list the devices you have with information about file systems.

```shell
lsblk -f
```

In general, `/dev/sda` is your original disk and `/dev/sdb` is USB key, but there may be different occasions. Do check it carefully before next steps.

Now you should have booted from you USB key (providing that it's `/dev/sdb`). Now begin partitioning by

```shell
gdisk /dev/sdb
```

if you are or want to transfer to UEFI + GPT. Next use `n` to create a new partition and `d` to delete one. Input `?` could help you to read instructions. For UEFI, it's necessary to create an *EFI System Partition* in the first place of your disk as 'bootable' partition (this concept is different from the one in MBR). Allocate spaces to partitions as you like. `/` is necessary, and `/home` or `swap` or something else depends on yourself.

*Optional*: use LVM (Logical Volume Manager) to manage and resize your partitions freely even after installation. Because I've not tried that yet, I can't give any instructions. Follow Arch Wiki for more help. Note that ESP can't be included in LVM.

### File system initialization

Remember that partition is different from file system, which might be confusing for beginners. First partition tables, then partitioning, and finally set up file systems on each partition.

`mkfs` will help you, like `mkfs.vfat` and `mkfs.ext4`. It's common to choose *ext4* as Linux file system, but do not forget to format ESP partition as FAT32, and SWAP partition need **NOT** be formatted to a exact file system. Instead use `mkswap` and `swapon`.

### Mounting HDD or SSD

Now you can mount your disks now. Mount `/` at `/mnt`, `/home` at `/mnt/home` (if you allocate independent spaces to `/home`) and so on. Create directories at `/mnt` before mounting.

### Install basic system

Use `vi` or `vim` to edit `/etc/pacman.d/mirrorlist` to select the mirror site fastest to you. I recommend you to uncomment the server of Tsinghua University TUNA since it supports IPv6.

Then

```shell
pacstrap /mnt base base-devel
```

`base-devel` is optional but highly recommended to install as most Arch users will have to develop something more or less on Arch.

### Basic configurations

It's simple so I just give the commands. It's better to understand why it's necessary to run this command rather than just copy it.

```shell
# fstab file records devices that will be mounted when booting (NEVER edit it manually!)
genfstab -U /mnt >> /mnt/etc/fstab

# a script encapsulating chroot, that mounts /proc, /sys and so on for you
arch-chroot /mnt

# locale settings, here I just assume that you only want English (US)
locale-gen

# name your local host as <yourHostName>
echo <yourHostName> > /etc/hostname

# configure hosts (local DNS)
echo "127.0.0.1	localhost.localdomain	localhost
::1		localhost.localdomain	localhost
127.0.1.1	myhostname.localdomain	myhostname" > /etc/hosts
```

### Enable network

As I've mentioned above, it's better that you're using wired connection. Or you'd better prepare `iw` and `wpa_supplicant` for wireless. By default Arch will only provide `netctl` as network management tool, so be sure you've read about instructions about this command by `man netctl`.

Check your network interface is enabled:

```shell
# assuming that your wired interface is named eth0
ip link set eth0 up
```

Given that you're using wired connection, then you can start network connection by

```shell
# default configs are enough if you're sure you're using eth0
cp /etc/netctl/examples/ethernet-static /etc/netctl/wired

# start wired connection
netctl start wired

# set up connection every time you reboot
netctl enable wired
```

Here it will be troublesome if you do not have IPv6 enabled or you have to log in to a gateway to connect to the Internet.

`netctl` only provides command-line tool. You could consider `networkmanager` as GUI especially if you want to use Gnome as desktop environment.

### Boot loader

It's quite difficult to understand the whole mechanism of boot loading including BIOS and UEFI, requiring some Unix file system knowledge. But here for convenience, it's recommend to use `grub` as boot loader, since it supports both BIOS and UEFI, both Windows and Linux. `syslinux` or `rEFInd` are optional boot loaders.

After you've connected to network, the next steps won't be troublesome now.

```shell
# grub-efi for UEFI users, and NEVER use pacman -Sy
pacman -Syu grub

# given that you're using x86_64, UEFI and you've arch-chroot to /mnt, while bootloader-id is as you like
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=grub

# create grub config file
grub-mkconfig -o /boot/grub/grub.cfg
```

Use `os-prober` if you prefer dual booting for Windows, and check `/etc/default/grub` if you use LVM.

That's not the end of boot setting. Install `intel-ucode` by `pacman` and create grub config file again to ensure that microcodes will be loaded when initializing ramdisk before anything else.

### Desktop installation

The official guide ends here, but I think most users will need a GUI desktop environment. I'm quite fond of Gnome so take it as an example:

```shell
# install extra tools if you want
pacman -Syu gnome gnome-extra
```

After you install any desktop environment, if you want to change to tty (command-line mode), press `Ctrl + Alt + Fn`.

### Last steps

Create your own user, and set password for root and your own user by `useradd` and `passwd`.

Use `exit` to come back to USB key, and umount your disk partitions now.

Reboot. Now you're done.

References:

\[1\]: https://wiki.archlinux.org/index.php/installation_guide

\[2\]: https://wiki.archlinux.org/index.php/USB_flash_installation_media
