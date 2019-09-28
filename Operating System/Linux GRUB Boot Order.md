# Linux - GRUB Boot Order

Created by : Mr Dk.

2019 / 09 / 15 10:55

Nanjing, Jiangsu, China

---

## About

改一下双系统的默认启动顺序

我的大本本装着 _Windows 10_ + _Ubuntu 18.04_ 的双系统

每次启动时，如果不人为选择，_GRUB_ 超时后自动启动 Ubuntu

有时候我人不在电脑边，开着 Windows，但它因为 Windows Update 自动重启了

结果给我重启到 Ubuntu 里面去了......

人在外面的我，想连 Windows 的远程桌面：？？？网断了？？？ 😂

回到实验室发现，哦，电脑已经在 Ubuntu 的登录界面等着了

---

## GRUB Default Boot Order

理论上应当想到，要改 GRUB 的配置文件

但是 GRUB 的配置文件是生成的，不建议手动修改：

```bash
$ sudo vim /boot/grub/grub.cfg
```

```
#
# DO NOT EDIT THIS FILE
#
# It is automatically generated by grub-mkconfig using templates
# from /etc/grub.d and settings from /etc/default/grub
#

### BEGIN /etc/grub.d/00_header ###
```

所以已经有了提示，需要编辑 `/etc/grub.d` 和 `/etc/default/grub`

```bash
$ cd /etc/grub.d
$ ls
00_header        10_linux      20_memtest86+  30_uefi-firmware  41_custom
05_debian_theme  20_linux_xen  30_os-prober   40_custom         README
$ vim README
```

```

All executable files in this directory are processed in shell expansion order.

  00_*: Reserved for 00_header.
  10_*: Native boot entries.
  20_*: Third party apps (e.g. memtest86+).

The number namespace in-between is configurable by system installer and/or
administrator.  For example, you can add an entry to boot another OS as
01_otheros, 11_otheros, etc, depending on the position you want it to occupy in
the menu; and then adjust the default setting via /etc/default/grub.
```

所以，`/etc/grub.d` 目录下都是些可执行文件，没什么好改的

只需要在 `/etc/default/grub` 中调整默认启动顺序就好了

```bash
$ sudo vim /etc/default/grub
```

```
# If you change this file, run 'update-grub' afterwards to update
# /boot/grub/grub.cfg.
# For full documentation of the options in this file, see:
#   info -f grub -n 'Simple configuration'

GRUB_DEFAULT=2
GRUB_TIMEOUT_STYLE=hidden
GRUB_TIMEOUT=0
GRUB_DISTRIBUTOR=`lsb_release -i -s 2> /dev/null || echo Debian`
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
GRUB_CMDLINE_LINUX=""

# Uncomment to enable BadRAM filtering, modify to suit your needs
# This works with Linux (no patch required) and with any kernel that obtains
# the memory map information from GRUB (GNU Mach, kernel of FreeBSD ...)
#GRUB_BADRAM="0x01234567,0xfefefefe,0x89abcdef,0xefefefef"

# Uncomment to disable graphical terminal (grub-pc only)
#GRUB_TERMINAL=console

# The resolution used on graphical terminal
# note that you can use only modes which your graphic card supports via VBE
# you can see them in real GRUB with the command `vbeinfo'
#GRUB_GFXMODE=640x480

# Uncomment if you don't want GRUB to pass "root=UUID=xxx" parameter to Linux
#GRUB_DISABLE_LINUX_UUID=true

# Uncomment to disable generation of recovery mode menu entries
#GRUB_DISABLE_RECOVERY="true"

# Uncomment to get a beep at grub start
#GRUB_INIT_TUNE="480 440 1"
```

其中，`GRUB_DEFAULT` 原先为 `0`，对应我的本本上的 Ubuntu

Window Boot Manager (即 Windows) 在 GRUB 列表上为第三个，因此将这个值修改为 `2`

根据该文件中的说明，修改该文件后，需要运行 `update-grub` 来生成最终的 GRUB 配置文件 `/boot/grub/grub.cfg`

```bash
$ sudo update-grub
Sourcing file `/etc/default/grub'
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-4.15.0-63-generic
Found initrd image: /boot/initrd.img-4.15.0-63-generic
Found linux image: /boot/vmlinuz-4.15.0-62-generic
Found initrd image: /boot/initrd.img-4.15.0-62-generic
Found linux image: /boot/vmlinuz-4.15.0-60-generic
Found initrd image: /boot/initrd.img-4.15.0-60-generic
Found Windows Boot Manager on /dev/nvme0n1p2@/EFI/Microsoft/Boot/bootmgfw.efi
Adding boot menu entry for EFI firmware configuration
done
```

重启之后，在 GRUB 中默认进入 Windows 了诶 😁

---
