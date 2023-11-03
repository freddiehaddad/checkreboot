# Checkreboot

A script to check if your computer requires a reboot or if processes need
restarting after updating packages.

## How it works

Two types of system updates are detected: Library and kernel updates. For kernel
updates, a reboot IS required, but for library updates, running processes
linking to old libraries can be restarted instead.

In this example, three packages have updates, but only `sqlite` requires further
action. We can avoid rebooting by restarting the processes depending on the
previous version of the library (before the update).

### Update system

```bash
$ sudo pacman -Syyu

Packages (3)

linux-firmware-20231030.2b304bfe-1
linux-firmware-whence-20231030.2b304bfe-1
sqlite-3.44.0-1
```

### Check if reboot needed

```bash
$ checkreboot

#
# LIBS
#
1password: /usr/lib/libsqlite3.so.0.8.6
firefox: /usr/lib/libsqlite3.so.0.8.6
Isolated\x20Servic: /usr/lib/libsqlite3.so.0.8.6
Isolated\x20Web\x20Co: /usr/lib/libsqlite3.so.0.8.6
Privileged\x20Cont: /usr/lib/libsqlite3.so.0.8.6
RDD\x20Process: /usr/lib/libsqlite3.so.0.8.6
Socket\x20Process: /usr/lib/libsqlite3.so.0.8.6
Utility\x20Process: /usr/lib/libsqlite3.so.0.8.6
waybar: /usr/lib/libsqlite3.so.0.8.6
WebExtensions: /usr/lib/libsqlite3.so.0.8.6
```

### Close listed programs

Programs can be closed using pkill, through the program itself, etc.

#### Closing the program directly

Close Firefox by pressing Ctrl+Q, clicking close button, Menu > Quit, etc.

```bash
$ checkreboot

#
# LIBS
#
1password: /usr/lib/libsqlite3.so.0.8.6
waybar: /usr/lib/libsqlite3.so.0.8.6
```

#### Closing programs with pkill

```bash
$ pkill waybar
$ pkill 1password
```

```bash
$ checkreboot
```

Viola! Restarting the computer prevented.

## Installation

Clone the repository and copy `checkreboot` into a directory in your `PATH`. I
personally add `$HOME/.local/bin` to my `PATH` and copy scripts into that
directory.

Be sure to verify the script is executable (`chmod 0755 checkreboot`).
