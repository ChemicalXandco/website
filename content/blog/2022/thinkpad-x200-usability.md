---
title: "Thinkpad X200 Usability"
date: 2022-01-20T20:28:31Z
---

This is a follow up to [installing Libreboot on the Thinkpad X200]({{< ref "/blog/2021/libreboot-x200" >}}).

It is a surprisingly capable machine despite being over 10 years old however I have some major gripes with it that make it unusable as a daily driver.
The keyboard and trackpoint are excellent, tactile, and efficient.
Battery life is very good, especially with the 9-cell battery which can last up to 9 hours.
It is very easy to swap out parts especially the ram and the hard drive.

# Major gripe #1: graphics

It does not support newer versions of opengl so alacritty has to run in software rendering mode which is not ideal.
While playing 720p youtube works fine, it does drop frames at 1080p

# Major gripe #2: noise

When doing certain tasks it will emit strange chirping noises which is quite irritating but for normal use it is not noticeable.
The fan is fairly quiet but the whole laptop can get quite warm during use.

# Major gripe #3: instability

Currently when using coreboot/libreboot the system is subjected to seemly random crashes during use.
This is the main reason why I can't use it as a daily driver.

# Conclusion

The X200 is a very good option for a completely free laptop, if you're willing to deal with the downsides.
Also, I discovered that the right-side usb port is actually a 'debug' port so is not usable.
