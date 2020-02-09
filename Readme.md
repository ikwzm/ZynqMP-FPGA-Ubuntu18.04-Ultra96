ZynqMP-FPGA-Ubuntu18.04-Ultra96
====================================================================================

Overview
------------------------------------------------------------------------------------

### Introduction

This Repository provides a Linux Boot Image(U-boot, Kernel, Ubuntu 18.04 Desktop) for Ultra96/Ultra96-V2.

### Note

Ubuntu and Linux Kernel, etc. provided in this repository are experimentally built.
No warranty, not intended for end product use, use at your own risk.

### Features

* Hardware
  + Ultra96    : Xilinx Zynq UltraScale+ MPSoC development board based on the Linaro 96Boards specification. 
  + Ultra96-V2 : updates and refreshes the Ultra96 product that was released in 2018.
* Boot Loader
  + FSBL(First Stage Boot Loader for ZynqMP)
  + PMU Firmware(Platform Management Unit Firmware)
  + BL31(ARM Trusted Firmware Boot Loader stage 3-1)
  + U-Boot xilinx-v2019.2 (customized)
* Linux Kernel Version v4.19.0
  + [linux-xlnx](https://github.com/Xilinx/linux-xlnx) tag=xilinx-v2019.2
  + Enable Device Tree Overlay with Configuration File System
  + Enable FPGA Manager
  + Enable FPGA Bridge
  + Enable FPGA Reagion
  + Enable ATWILC3000 Linux Driver for Ultra96-V2
* Ubuntu18.04(bionic) Desktop Root File System
  + Installed build-essential
  + Installed ubuntu-desktop
  + Installed lightdm
  + Installed ruby python python3
  + Installed Other package list -> [files/ubuntu18.04-dpkg-list.txt](files/ubuntu18.04-dpkg-list.txt)

Install
------------------------------------------------------------------------------------

* Install to SD-Card
  + [Ultra96](doc/install/ultra96.md)
  + [Ultra96-V2](doc/install/ultra96v2.md)

Build 
------------------------------------------------------------------------------------

* Build Boot Loader
  + [Ultra96](doc/build/ultra96-boot.md)
  + [Ultra96-V2](doc/build/ultra96v2-boot.md)
* [Build Linux Kernel](doc/build/linux-kernel.md)
* [Build Ubuntu 18.04 Desktop RootFS](doc/build/ubuntu18.04-rootfs.md)
