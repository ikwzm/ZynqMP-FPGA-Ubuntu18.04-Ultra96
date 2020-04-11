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

