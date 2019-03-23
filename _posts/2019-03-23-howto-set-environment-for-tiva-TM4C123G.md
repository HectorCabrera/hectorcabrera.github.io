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
Now, we need t get a copy of the GNU Tools for ARM Embedded Processors from [here](https://launchpad.net/gcc-arm-embedded/+download). Just go an download te latest package for linux, in my case it was: gcc-arm-none-eabi-5_4-2016q3-20160926-linux.tar.bz2 

Go an untar the file in the working directory for all the tiva projects.

```bash
mv ~/Downloads/gcc-arm-none-eabi-5_4-2016q3-20160926-linux.tar.bz2 .
tar -xvf gcc-arm-none-eabi-5_4-2016q3-20160926-linux.tar.bz2
```

You should have a folder named after the tar file in your working directory. Mine was called gcc-arm-none-eabi-5_4-2016q3.

Add your arm-none-eabi folder to your PATH variable. I'm doing this through my *.bashrc* like this:

```bash
# PATH
export PATH=$PATH:$HOME/embedded/gcc-arm-none-eabi-5_4-2016q3/bin
```
Finally, lets get a copy of the Tiva Software provided by TI from this [link](http://software-dl.ti.com/tiva-c/SW-TM4C/latest/index_FDS.html). Download the *Full Release* package, in my case the file was *SW-TM4C-2.1.4.178.exe*. You will be asked to login into TI to complete the download.

Once the download is completed, create a folder named *tivaware* in your working directory and unzip *SW-TM4C-2.1.4.178.exe* there. Finally, build everything executing *make*.

```bash
mkdir tivaware
cp ~/Downloads/SW-TM4C-2.1.4.178.exe .
unzip SW-TM4C-2.1.4.178.exe
make
```

### Getting ready for a test run

We are using the templates from the [Tiva Template](https://github.com/uctools/tiva-template) project. So lets go and clone the repo:

```bash
git clone https://github.com/uctools/tiva-template.git
```

We are going to use the code examples from Tiva Template, but before that, lets download the tool to flash our development board [lm4tools](https://github.com/utzig/lm4tools).

```bash
git clone https://github.com/utzig/lm4tools.git
cd lm4tools/lm4flash/
make
```

To flash the development board without executing everything as root, add the following udev rule:

```bash
cd  /etc/udev/rules.d/
gvim 99-tiva-launchpad.rules
```

*99-tiva-launchpad.rule* should have the following content:

```bash
SUBSYSTEM=="usb", ATTRS{idVendor}=="1cbe", ATTRS{idProduct}=="00fd", MODE="0660"
```
restart the udev system:

```bash
sudo udevadm control --reload
```


Last, lets add debugging capabilities for our enviroment, lets add [OpenOCD](http://openocd.org/).

```bash
sudo pacman -S openocd
```

### Blinking test

Let's use one of the Tiva Templates examples: tiva-template/src/main.c

Configure the Tiva Template for our target uM:

```bash
gvim  tiva-template/Makefile
```

It should look like this:

```make
# Tiva Makefile
# #####################################
#
# Part of the uCtools project
# uctools.github.com
#
#######################################
# user configuration:
#######################################
# TARGET: name of the output file
TARGET = main
# MCU: part number to build for
MCU = TM4C123GH6PM
# SOURCES: list of input source sources
SOURCES = main.c startup_gcc.c
# INCLUDES: list of includes, by default, use Includes directory
INCLUDES = -IInclude
# OUTDIR: directory to use for output
OUTDIR = build
# TIVAWARE_PATH: path to tivaware folder
TIVAWARE_PATH = $(HOME)/Documents/embedded/tivaware
```

I only changed my TIVAWARE_PATH.

Then:

```bash
cd tiva-template
make
```

Results are placed under *build* folder.

### Flashing the board

To try the compiled bin, execute:

```bash
cd lm4tools/lm4flash/
sudo ./lm4flash ~/Documents/embedded/tiva-template/build/main.bin
```

### Final Notes

At the end, I found another reference for Arch based distros, it can be found [here](https://www.hackster.io/tcss/upload-code-to-ti-tm4c123-using-linux-cmake-and-lm4tools-c33cec).








