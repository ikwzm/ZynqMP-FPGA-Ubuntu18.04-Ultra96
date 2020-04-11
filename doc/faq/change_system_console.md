## Change system console

To change the system console, change the "linux_boot_args_console" variable in "boot/uEnv.txt".

### Change to serial port

```text:boot/uEnv.txt

linux_boot_args_console=console=ttyPS0,115200

```

### Change to Motitnor+Keyboard

```text:boot/uEnv.txt

linux_boot_args_console=console=tty1

```

### Change by bootmenu

The system console can be changed in the boot menu when u-boot starts.
In this case, add the following to the end of "boot/uEnv.txt".

```text:boot/uEnv.txt

########################################################################
# Boot Menu Example
########################################################################
bootmenu_0=Boot Default=boot
bootmenu_1=Boot ttyPS0=env set linux_boot_args_console console=ttyPS0,115200 && boot
bootdelay=5

```
