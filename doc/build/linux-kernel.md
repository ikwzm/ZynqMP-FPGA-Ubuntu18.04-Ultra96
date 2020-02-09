Build Linux Kernel 
====================================================================================

## Download Repository

```console
shell$ git clone -b v2019.2.1 https://github.com/ikwzm/ZynqMP-FPGA-Linux.git
shell$ cd ZynqMP-FPGA-Linux
shell$ git lfs pull
shell$ cd ..
```

## Copy linux kernel image

### Ultra96

```console
shell$ cp ZynqMP-FPGA-Linux/target/Ultra96/boot/image-*.bin      target/Ultra96/boot/
shell$ cp ZynqMP-FPGA-Linux/target/Ultra96/boot/devicetree-*.bin target/Ultra96/boot/
```

### Ultra96-V2

```console
shell$ cp ZynqMP-FPGA-Linux/target/Ultra96-V2/boot/image-*.bin      target/Ultra96-V2/boot/
shell$ cp ZynqMP-FPGA-Linux/target/Ultra96-V2/boot/devicetree-*.bin target/Ultra96-V2/boot/
```

## Reference

* https://github.com/ikwzm/ZynqMP-FPGA-Linux


