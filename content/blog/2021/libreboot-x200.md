---
title: "My experience installing Libreboot on the Thinkpad X200"
date: 2021-06-21T17:01:04+01:00
---

# Introduction

I am writing this because I have seen a lot of misinformation about how to actually do this so here I will describe what I did, what problems I encountered and how I resolved them. I recommend following [the guide in the Libreboot documentation](https://libreboot.org/docs/install/x200_external.html), but I am going to detail below what I did and what worked and what didn't work to help anyone who might also encounter the same problems.

# Hardware

- Raspberry pi 4B (any version apart from zero should work fine though)
- 6 female to female jumper wires (no longer than 10cm or connection is too bad)
- SOP16/SOIC16 (might need 8 pin version depending on the chip)

# Before installation

As per the Libreboot documentation linked above, run `flashrom -p internal` on the X200 to get the name of the chip. If it is 8MB like mine was then it will output a list of possible chips but it is most likely "MX25L6405D". Also run `ip addr` to get the MAC address of the gigabit ethernet since you will need this later.

# Connecting the programmer

[Do not use CH341A!](https://libreboot.org/docs/install/spi.html#do-not-use-ch341a) I have seen lots of people recommend it but it is not designed for this purpose, if you use it you risk damaging the hardware. I used the cheap clip but in hindsight I should have used the Ponoma clip because it has enough space to attach the wires. I had to take the plastic end off the wires and wrap them with tape in order to make them fit. Do not plug the 3.3v into the pi until the clip is on the chip and all the other wires are connected or you risk damaging the chip. On the raspi there is a graphical interface to configure it but this did not work for me. Enable SPI using `sudo raspi-config` and then run `sudo reboot` and then it is ready.

# Flashing

At first I tried using the version of flashrom in the repository but I kept getting errors. [As noted in the Libreboot documentation](https://libreboot.org/docs/install/spi.html#install-flashrom) you need to use a [patched version](https://vimuser.org/hackrom.tar.xz) and this worked very well for me. The compiled version included in that archive runs on raspberry pi 4, but if it does not work on your cpu you should compile it.

First you should dump the chip contents to a file:

```
sudo ./flashrom -p linux_spi:dev=/dev/spidev0.0,spispeed=32768 -c "MX25L6405D" --workaround-mx -r dump1.bin

sudo ./flashrom -p linux_spi:dev=/dev/spidev0.0,spispeed=32768 -c "MX25L6405D" --workaround-mx -r dump2.bin
```

and then if `diff dump1.bin dump2.bin` outputs nothing, you are safe to proceed. If not, run `rm dump1.bin dump2.bin` and try again. If you got an error while reading the flash, check the wiring and the connections.

I used the rom from [here](https://github.com/JaGoLi/Libreboot-X200-Updated) and changed the mac address using the provided script. Be aware that you must run `sudo apt install bison flex` before you can run that script. I used the microcode version because it is more stable but you might be able to get away with using the free version. Run the script with the MAC address that you previously noted like the example in `README.md`.

Once you have the rom configured to your liking run `sudo ./flashrom -p linux_spi:dev=/dev/spidev0.0,spispeed=32768 -w /path/to/libreboot.rom -c "MX25L6405D" --workaround-mx`. This actually failed the first time for me on the verify stage but I ran the same command again and it worked the second time.

# Installing GNU/Linux

Make sure the battery is plugged in and working or else it might shutdown after a minute or so. For some reason my USB storage was not being detected so I tried a different USB port and it worked. I am not sure why this is but it is something to try if anyone else has this problem. Make sure you install the `intel-ucode` (Arch) package or else it will be unstable.
