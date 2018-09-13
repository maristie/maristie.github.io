---
title: Set up dual boot
---

It's common for people who do not have multiple PCs to install both Windows and Linux on the same computer. Some of them choose to use Virtual Machines to run Linux on Windows or in turn. Virtualization is becoming a mature technology, whose performance is gradually closer to a real operating system. But there are always times that we'd like to use a real OS rather than a VM.

## Traditional BIOS booting

In traditional BIOS booting, it will be difficult to dual boot Windows and Linux. Windows will *NOT* recognize Linux boot loaders like grub. Though in contrast, grub could identify Windows boot loader, it has to depend on chain loading to boot Windows instead. That's not a perfect solution. Assume that your grub is corrupt, it will be impossible to boot Windows as well, since in MBR all booting should place its codes in the header of MBR, and next jump to boot sector of the partition with bootable flag.

## UEFI booting

Things become different for UEFI. UEFI defines a general interface for booting and it's no longer necessary to put booting code in the MBR header. UEFI works well with GPT, and uses a common partition called 'EFI System Partition', which for short is ESP. Now booting is easy. Just install boot loaders such as 'Windows Boot Manager' or 'grub-efi' to ESP and that's all. Generally speaking, they won't conflict with each other. No worry when you're installing both operating systems. Now the only thing you need to do to select a system when booting is to press the hotkey provided by your PC manufacturer (the same hotkey when you would like to select bootable device, as now different EFI files of OSs are treated as different 'devices' now).

***

*Update*: You may find that if Windows and Linux are dual booted, Windows will usually display a wrong time (UTC, which isn't your time zone). According to Arch Wiki, you need to add an extra entry to Registry Table by regedit.

```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\TimeZoneInformation\RealTimeIsUniversal
```

The value should be set to 1. Be careful that you should set the data type to DWORD on 32-bit OS, and QWORD on 64-bit OS.

Reference: https://wiki.archlinux.org/index.php/time#UTC_in_Windows
