## Ultra96

### Downlowd from github

```console
shell$ git clone git://github.com/ikwzm/ZynqMP-FPGA-Ubuntu18.04-Ultra96
shell$ cd ZynqMP-FPGA-Ubuntu18.04-Ultra96
shell$ git checkout v2019.2.1
shell$ git lfs pull
```

### File Description

 * target/Ultra96
   + boot/
     - boot.bin                                                    : Stage 1 Boot Loader
     - uEnv.txt                                                    : U-Boot environment variables for linux boot
     - image-4.19.0-xlnx-v2019.2-zynqmp-fpga                       : Linux Kernel Image       (use Git LFS)
     - devicetree-4.19.0-xlnx-v2019.2-zynqmp-fpga-ultra96.dtb      : Linux Device Tree Blob   
     - devicetree-4.19.0-xlnx-v2019.2-zynqmp-fpga-ultra96.dts      : Linux Device Tree Source
 * ubuntu18.04-desktop-rootfs.tgz                                  : Ubuntu 18.04 Desktop Root File System (use Git LFS)
 
### Format SD-Card

[./doc/install/format-disk.md](format-disk.md)

### Write to SD-Card

#### Mount SD-Card

```console
shell# mount /dev/sdc1 /mnt/usb1
shell# mount /dev/sdc2 /mnt/usb2
```
#### Make Boot Partition

```console
shell# cp target/Ultra96/boot/*                  /mnt/usb1
```

#### Make RootFS Partition

```console
shell# tar xfz ubuntu18.04-desktop-rootfs.tgz -C /mnt/usb2
```

#### Unmount SD-Card

```console
shell# umount /mnt/usb1
shell# umount /mnt/usb2
```

