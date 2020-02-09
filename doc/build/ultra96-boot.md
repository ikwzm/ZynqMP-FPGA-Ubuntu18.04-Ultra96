Build Boot Loader for Ultra96
====================================================================================

## Download Repository

```console
shell$ git clone -b v2019.2.1 https://github.com/ikwzm/ZynqMP-FPGA-Linux.git
shell$ cd ZynqMP-FPGA-Linux
shell$ git lfs pull
shell$ cd ..
```

## Copy boot.bin

```console
shell$ cp ZynqMP-FPGA-Linux/target/Ultra96/boot/boot.bin                 target/Ultra96/boot/
shell$ cp ZynqMP-FPGA-Linux/target/Ultra96/boot/boot_outer_shareable.bin target/Ultra96/boot/
```

## Reference

* https://github.com/ikwzm/ZynqMP-FPGA-Linux
* https://github.com/ikwzm/ZynqMP-U-Boot-Ultra96

