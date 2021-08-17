---
title: Simple Tips on Manjaro
tags: [IT, Linux]
redirect_from:
    - /blog/tips-on-manjaro/
---

From my experience of using Manjaro in the last two months.

## About `acer_wmi`

`acer_wmi` is, just as its name implies, something that works for Acer laptops. But the kernel tries to load this module even in other notebooks. Therefore it has to be blacklisted when booting up.

```shell
$ sudo vim /etc/modprobe.d/acer_wmi_blacklist.conf
# add this line
blacklist acer_wmi
```

Done.

## Graphics Card Driver

Only one piece of advice: *NEVER* use proprietary drivers, especially for Nvidia. It will just cause your PC to crash.

Use `mhwd` (short for Manjaro Hardware Detection) to install or remove the drivers.

## Evince does not display FandolSong of LaTeX

FandolSong is both the default font for TeX Live and most of the time designated font for papers as well.

However it met some problems with Evince (default PDF viewer of GNOME) that FandolSong, perhaps along with more fonts, cannot be displayed or rather rendered with Evince.

```shell
$ pacman -S poppler-data
```

Everything goes well now.

## Finally something about Tex Live

I referred to TeX Live above. TexLive is necessary for most students in university. It's obvious that TeX Live is much easier to use on Linux, since you could organize your tex files by makefile and use it by typing a simple command in CLI.

However both on Arch and Manjaro the `texlive` packages in `pacman` are just not satisfactory. Lots of packages in TeX Live cannot be found if you're using pacman to install TeX Live. According to [TeX Live - ArchWiki](https://wiki.archlinux.org/index.php/TeX_Live), with pacman Tex Live is installed in medium scheme, while we always need full scheme. In addition, `tlmgr` is not included in pacman version.

So I recommend you to have a look at [TeX Live - Quick install](https://www.tug.org/texlive/quickinstall.html) and install manually with the upstream installer.

Do not forget to configure the mirror site as TUNA and update packages and `tlmgr` often with

```shell
$ tlmgr update --all --self
```

Have fun with Tex Live.
