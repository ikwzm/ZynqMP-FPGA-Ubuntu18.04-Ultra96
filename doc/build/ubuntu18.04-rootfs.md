## Build Ubuntu 18.04 Desktop RootFS

### Setup parameters 

```console
shell$ apt-get install qemu-user-static debootstrap binfmt-support
shell$ export targetdir=ubuntu18.04-rootfs
shell$ export distro=bionic
```

### Prepare Debian Packages

#### Download ZynqMP-FPGA-Linux

```console
shell$ git clone --depth 1 -b v2019.2.1 git://github.com/ikwzm/ZynqMP-FPGA-Linux
shell$ cd ZynqMP-FPGA-Linux
shell$ git lfs pull
shell$ cd ..
```

### Build the root file system in $targetdir(=ubuntu18.04-rootfs)

```console
shell$ mkdir $targetdir
shell$ sudo debootstrap --arch=arm64 --foreign $distro     $targetdir
shell$ sudo cp /usr/bin/qemu-aarch64-static                $targetdir/usr/bin
shell$ sudo cp /etc/resolv.conf                            $targetdir/etc
shell$ sudo cp scripts/build-ubuntu18.04-rootfs.sh         $targetdir
shell$ sudo cp ZynqMP-FPGA-Linux/linux-*.deb               $targetdir
shell$ sudo cp files/xserver-*.deb                         $targetdir
shell$ sudo cp files/xorg.conf                             $targetdir
````

### Build ubuntu18.04-rootfs with QEMU

#### Change Root to ubuntu18.04

```console
shell$ sudo chroot $targetdir
```

There are two ways

1. run build-ubuntu18.04-rootfs.sh (easy)
2. run this chapter step-by-step (annoying)

#### Setup APT

````console
ubuntu18.04-rootfs# distro=bionic
ubuntu18.04-rootfs# export LANG=C
ubuntu18.04-rootfs# /debootstrap/debootstrap --second-stage
````

```console
ubuntu18.04-rootfs# cat <<EOT > /etc/apt/sources.list
deb http://ports.ubuntu.com/ubuntu-ports bionic main restricted universe multiverse
deb-src http://ports.ubuntu.com/ubuntu-ports bionic main restricted universe multiverse
deb http://ports.ubuntu.com/ubuntu-ports bionic-updates main restricted universe multiverse
deb-src http://ports.ubuntu.com/ubuntu-ports bionic-updates main restricted universe multiverse
deb http://ports.ubuntu.com/ubuntu-ports bionic-security main restricted universe multiverse
deb-src http://ports.ubuntu.com/ubuntu-ports bionic-security main restricted universe multiverse
EOT
```

```console
ubuntu18.04-rootfs# cat <<EOT > /etc/apt/apt.conf.d/71-no-recommends
APT::Install-Recommends "0";
APT::Install-Suggests   "0";
EOT
```

```console
ubuntu18.04-rootfs# apt-get -y update
ubuntu18.04-rootfs# apt-get -y upgrade
```

#### Install Core applications

```console
ubuntu18.04-rootfs# apt-get install -y locales dialog
ubuntu18.04-rootfs# dpkg-reconfigure locales
ubuntu18.04-rootfs# apt-get install -y net-tools openssh-server ntpdate resolvconf sudo less hwinfo ntp tcsh zsh file
```

#### Setup hostname

```console
ubuntu18.04-rootfs# echo ubuntu-fpga > /etc/hostname
```

#### Setup root password

```console
ubuntu18.04-rootfs# passwd
```

This time, we set the "admin" at the root' password.

To be able to login as root from Zynq serial port.

```console
ubuntu18.04-rootfs# cat <<EOT >> /etc/securetty
# Seral Port for Xilinx Zynq
ttyPS0
EOT
```

#### Add a new guest user

```console
ubuntu18.04-rootfs# adduser fpga
```

This time, we set the "fpga" at the fpga'password.

```console
ubuntu18.04-rootfs# echo "fpga ALL=(ALL:ALL) ALL" > /etc/sudoers.d/fpga
```

#### Setup sshd config

```console
ubuntu18.04-rootfs# sed -i -e 's/#PasswordAuthentication/PasswordAuthentication/g' /etc/ssh/sshd_config
```

#### Setup Time Zone

```console
ubuntu18.04-rootfs# dpkg-reconfigure tzdata
```

or if noninteractive set to Asia/Tokyo

```console
ubuntu18.04-rootfs# echo "Asia/Tokyo" > /etc/timezone
ubuntu18.04-rootfs# dpkg-reconfigure -f noninteractive tzdata
```


#### Setup fstab

```console
ubuntu18.04-rootfs# cat <<EOT > /etc/fstab
none		/config		configfs	defaults	0	0
/dev/mmcblk0p1	/mnt/boot	auto		defaults	0	0
EOT
````

#### Setup Network Interface

```console
ubuntu18.04-rootfs# cat <<EOT > /etc/network/interfaces.d/eth0
allow-hotplug eth0
iface eth0 inet dhcp
EOT
````

#### Setup /lib/firmware

```console
ubuntu18.04-rootfs# mkdir /lib/firmware
ubuntu18.04-rootfs# mkdir /lib/firmware/ti-connectivity
ubuntu18.04-rootfs# mkdir /lib/firmware/mchp
```

#### Install Development applications

```console
ubuntu18.04-rootfs# apt-get install -y build-essential
ubuntu18.04-rootfs# apt-get install -y pkg-config
ubuntu18.04-rootfs# apt-get install -y git
ubuntu18.04-rootfs# apt-get install -y kmod
ubuntu18.04-rootfs# apt-get install -y flex bison
ubuntu18.04-rootfs# apt-get install -y u-boot-tools device-tree-compiler
ubuntu18.04-rootfs# apt-get install -y libssl-dev
ubuntu18.04-rootfs# apt-get install -y socat
ubuntu18.04-rootfs# apt-get install -y ruby rake ruby-msgpack ruby-serialport
ubuntu18.04-rootfs# apt-get install -y python  python-dev  python-setuptools  python-wheel  python-pip
ubuntu18.04-rootfs# apt-get install -y python3 python3-dev python3-setuptools python3-wheel python3-pip
ubuntu18.04-rootfs# apt-get install -y python-numpy python3-numpy
ubuntu18.04-rootfs# pip3 install msgpack-rpc-python
```

#### Install Wireless tools and firmware

```console
ubuntu18.04-rootfs# apt-get install -y wireless-tools
ubuntu18.04-rootfs# apt-get install -y wpasupplicant
ubuntu18.04-rootfs# apt-get install -y firmware-realtek
ubuntu18.04-rootfs# apt-get install -y firmware-ralink
```

```console
ubuntu18.04-rootfs# git clone git://git.ti.com/wilink8-wlan/wl18xx_fw.git
ubuntu18.04-rootfs# cp wl18xx_fw/wl18xx-fw-4.bin /lib/firmware/ti-connectivity
ubuntu18.04-rootfs# rm -rf wl18xx_fw/
```

```console
ubuntu18.04-rootfs# git clone git://git.ti.com/wilink8-bt/ti-bt-firmware
ubuntu18.04-rootfs# cp ti-bt-firmware/TIInit_11.8.32.bts /lib/firmware/ti-connectivity
ubuntu18.04-rootfs# rm -rf ti-bt-firmware
```

```console
ubuntu18.04-rootfs# git clone git://github.com/linux4wilc/firmware  linux4wilc-firmware  
ubuntu18.04-rootfs# cp linux4wilc-firmware/*.bin /lib/firmware/mchp
ubuntu18.04-rootfs# rm -rf linux4wilc-firmware  
```

#### Install Other applications

```console
ubuntu18.04-rootfs# apt-get install -y samba
ubuntu18.04-rootfs# apt-get install -y avahi-daemon
```

#### Install haveged for Linux Kernel 4.19

```console
ubuntu18.04-rootfs# apt-get install -y haveged
```

#### Install network-manager for Ubuntu18.04

```console
ubuntu18.04-rootfs# apt-get install -y network-manager
```

#### Move Debian Package to /home/fpga/debian

```console
ubuntu18.04-rootfs# mkdir             home/fpga/debian
ubuntu18.04-rootfs# mv *.deb          home/fpga/debian
ubuntu18.04-rootfs# mv xorg.conf      home/fpga/debian
ubuntu18.04-rootfs#chown fpga.fpga -R home/fpga/debian
```

#### Install Linux Image and Header Debian Packages

```console
ubuntu18.04-rootfs# dpkg -i home/fpga/debian/linux-image-4.19.0-xlnx-v2019.2-zynqmp-fpga_4.19.0-xlnx-v2019.2-zynqmp-fpga-2_arm64.deb
ubuntu18.04-rootfs# dpkg -i home/fpga/debian/linux-headers-4.19.0-xlnx-v2019.2-zynqmp-fpga_4.19.0-xlnx-v2019.2-zynqmp-fpga-2_arm64.deb
```

#### Install Xorg HWE (Option)

```console
ubuntu18.04-rootfs# apt-get install -y xserver-xorg-core-hwe-18.04 xserver-xorg-input-all-hwe-18.04 xserver-xorg-legacy-hwe-18.04
ubuntu18.04-rootfs# apt-get install -y xorg
```

#### Install Ubuntu Desktop

```console
ubuntu18.04-rootfs# apt-get install -y ubuntu-desktop
```

#### Install ZynqMP-FPGA-Xserver

```console
ubuntu18.04-rootfs# dpkg -i home/fpga/debian/xserver-xorg-video-armsoc-xilinx_1.4-2_arm64.deb
ubuntu18.04-rootfs# cp      home/fpga/debian/xorg.conf /etc/X11
```

#### Change Display Manager gdm -> lightdm

```console
ubuntu18.04-rootfs# apt install -y lightdm lightdm-settings slick-greeter
```

#### Disable Sleep/Suspend Mode

```console
ubuntu18.04-rootfs# systemctl mask sleep.target suspend.target hybrid-sleep.target
```

#### Clean Cache

```console
ubuntu18.04-rootfs# apt-get clean
```

#### Create Debian Package List

```console
ubuntu18.04-rootfs# dpkg -l > dpkg-list.txt
```

#### Finish

```console
ubuntu18.04-rootfs# exit
shell$ sudo rm -f $targetdir/usr/bin/qemu-aarch64-static
shell$ sudo rm -f $targetdir/build-ubuntu18.04-rootfs.sh
shell$ sudo mv    $targetdir/dpkg-list.txt files/ubuntu18.04-dpkg-list.txt
```

### Build ubuntu18.04-desktop-rootfs.tgz

```console
shell$ cd $targetdir
shell$ sudo tar cfz ../ubuntu18.04-desktop-rootfs.tgz *
```

