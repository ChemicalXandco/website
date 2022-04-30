---
title: "Fixing Nokia G10 Boot Loop"
date: 2022-04-29T21:29:11+01:00
---

# Introduction

Nokia G10 (Android 11) crashed under strange conditions, possibly a race condition, and started boot looping.
I was able to reset it and resumble normal operation using the following steps:

# Prepare PC

Arch:
```
sudo pacman -S android-tools
```

Debian:
```
sudo apt install android-tools-fastboot
```

(Optional) configure udev to avoid using `sudo`:
```
sudo sh -c "echo '# Nokia G10' >> /etc/udev/rules.d/51-android.rules"

sudo sh -c "echo 'SUBSYSTEM==\"usb\", ATTR{idVendor}==\"0e8d\", MODE=\"0664\", GROUP=\"plugdev\"' >> /etc/udev/rules.d/51-android.rules"

sudo udevadm control --reload
```

# Connect device

1. power off
2. hold volume down + power button until fastboot prompt shows
3. plug it in

Check device is connected:
```
$ fastboot devices
PT9[rest of serial]	fastboot
```

# Reset

Enter fastbootd mode:
```
fastboot reboot fastboot
```

Wait for reboot to finish.

Then you can enter recovery mode:
```
fastboot reboot recovery
```

Select 'Wipe data/factory reset' (use volume up/down to scroll and power button to select) and wait.

# Finishing up

At this stage booting normally yields a screen that states 'No valid operating system could be found'.
Strangely enough I was able to get it to boot normally by holding the volume up button.
