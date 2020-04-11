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

