# What It Does #

The eventlircd daemon provides four functions for [LIRC](http://www.lirc.org) devices:

  1. converting multiple Linux input event devices into an lircd socket,
  1. separating keyboard and mouse/joystick functionality,
  1. mapping keyboard shortcut key code sequences to single key codes, and
  1. hotplugging using [udev](http://en.wikipedia.org/wiki/Udev).

# How It Does What It Does #

When an lircd supported device is added/removed, udev rules start/stop an LIRC daemon (lircd) to convert the device to a Linux input event device. When an eventlircd supported device is added/removed, udev rules signal eventlircd using eventlircd specific environment variables. An init script starts/stops the eventlircd daemon. In addition, an init script starts/stops any lircd daemons for lircd supported devices that are not detected by udev.

# Why It Exists #

I maintain [MiniMyth](http://www.minimyth.org/). Over time, there were requests to support

  1. Multiple remote controls,
  1. Remote controls with mouse/joystick functionality,
  1. Bluetooth remote controls that may not be paired at boot,
  1. Media Center Edition remote controls that appear as USB HID devices and send keyboard shortcuts, and
  1. Remote control hotplugging.

While I was able to configure lircd to support 1, 2 (post 0.8.6) and 3, I was not able to configure lircd to support 4 without device specific lircrc files, and I was not able to configure lircd to support 5. While it is likely that I could have patched lircd to support 4 and 5, I was uneasy with the changes that appeared be required. Therefore, I decided to implement the functionality as a separate daemon used in conjunction with lircd.

# Overlap With Other Software #

The eventlircd functionality has some overlap with the [inputlircd](http://svn.sliepen.eu.org/inputlirc/trunk/) functionality. Both enable converting multiple Linux input event devices into an lircd socket. However, they differ in several ways. The inputlircd daemon does not support hotplugging using udev, separating keyboard and mouse/joystick functionality, or mapping keyboard shortcut key code sequences to single key codes. The eventlircd daemon does not support manually configuring the Linux input event devices, or mapping key codes to non key code names.

The evenlircd functionality has some overlap with the lircd functionality. Both enable converting multiple Linux input event devices into an lircd socket and separating keyboard and mouse/joystick functionality. However, they differ in several ways. The lircd daemon does not support hotplugging using udev, or mapping keyboard shortcut key code sequences to single key codes. The eventlircd daemon does not support manually configuring the Linux input event devices, mapping key codes to non key code names, using devices that are not Linux input event devices, or transmitting IR signals.