---
published: false
description: >-
  My notes on setting a development environment on Manjaro for a Tiva TM4C123G
  board
categories:
  - how-to
tags: howto tiva linux manjaro
---
## HOWTO: Setting a development environment in Manjaro for the Tiva TM4C123G

Gettng back, once again, to start using micro-controllers. This time I'm moving away from the AVR family to a Tiva C-Series from Texas Instruments. I'm using a [M4C123G LaunchPad](http://www.ti.com/tool/EK-TM4C123GXL) development board.

This are my notes on how to setup the development enviroment on Manjaro. As a reference, I use this post from chrisrm.com, in there, they were using Ubuntu but I'm hopping that targeting Manjaro shouldn't be too much hassle.

### Dependencies

The toolset requires some x86 libraries, so let's check if /etc/pacman.conf have the *multilib* repositorie enabled:

```bash
gvim /etc/pacman.conf
```

The following section should be uncommented:

```bash
# If you want to run 32 bit applications on your x86_64 system,
# enable the multilib repositories as required here.

[multilib]
SigLevel = PackageRequired
Include = /etc/pacman.d/mirrorlist
```
Next, lets install the 32-bit C libraries:

```bash
sudo pacman -S lib32-glibc lib32-gcc-libs
```
Next, the rest of dependencies:

```bash
sudo pacman -S lib32-glibc lib32-gcc-libs gmp mpfr ncurses libmpc autoconf texinfo base-devel libftdi python-yaml zlib lib32-zlib libtool lib32-glibc libusb
```






