## Change boot runlevel

The boot runlevel of Ubuntu can be changed by the "linux_boot_args_systemd" variable in "boot/uEnv.txt".

### Boot Ubuntu with CUI

```text:boot/uEnv.txt

linux_boot_args_systemd=multi-user.target

```

### Boot Ubuntu with GUI

```text:boot/uEnv.txt

linux_boot_args_systemd=graphical.target

```

### Boot Ubuntu with default

```text:boot/uEnv.txt

linux_boot_args_systemd=

```

### Change by bootmenu

The boot runlevel can be changed in the boot menu when u-boot starts.
In this case, add the following to the end of "boot/uEnv.txt".

```text:boot/uEnv.txt

########################################################################
# Boot Menu Example
########################################################################
bootmenu_0=Boot Default=boot
bootmenu_1=Boot CUI on Display=env set linux_boot_args_systemd systemd.unit=multi-user.target && boot
bootdelay=5

```
