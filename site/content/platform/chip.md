+++
title = "C.H.I.P."
description = "NextThing Co's board"
picture = "/img/chip.jpg"
+++

# Overview

The NextThing Co's C.H.I.P. uses an Allwinner R8 or G8 processor. The following
functionality is supported:

- 3x I²C buses
- 1x SPI bus with 1x chip-enable
- 2x high performance edge detection enabled memory-mapped GPIO pins
- 41x high performance memory-mapped GPIO pins (the "LCD" and "CSI" pins and a
  few more)
- 8x low performance GPIO pins via pcf8574 I²C I/O extender ("XIO" pins)


# Warning

Sadly NextThing Co [shut down its
activities](https://hackaday.com/2018/04/03/is-this-the-end-for-the-c-h-i-p/).

The main drawback is that the web flasher doesn't work anymore, and the server
to host the binary files is down.

http://www.chip-community.org/ and https://github.com/NextThingCo are still good
sources.


# Support

The NextThing Co's C.H.I.P. board is supported using sysfs drivers as well as
using high performance memory-mapped I/O for gpio pins.

- `periph` is tested with NTC provided 4.4.13+ kernel Debian It should work with
  any flavor, [file a bug](https://github.com/google/periph/issues) otherwise.
- C.H.I.P. is fully supported
- C.H.I.P. Pro and PocketCHIP specific headers are not defined, just use the CPU
  gpio number instead.


## Drivers

- CPU driver lives in
  [periph.io/x/host/v3/allwinner](https://periph.io/x/host/v3/allwinner).
- Headers driver lives in
  [periph.io/x/host/v3/chip](https://periph.io/x/host/v3/chip). It
  defines both `U13` and `U14` with the aliases as printed on the actual
  headers.
- sysfs driver lives in
  [periph.io/x/host/v3/sysfs](https://periph.io/x/host/v3/sysfs).


# Configuration

For in-depth information about the hardware the best reference is in the
[community wiki](http://www.chip-community.org/index.php/Hardware_Information),
which also has a section on [building kernels and device
drivers](http://www.chip-community.org/index.php/Kernel_Hacking).


## GPIO

Most GPIO are supported at extremely high speed via memory mapped GPIO
registers.

Interrupt based GPIO edge detection is only supported on a few of the
processor's pins: AP-EINT1(PG1), AP-EINT3(PB3), CSIPCK(PE0), and CSICK(PE1).

Edge detection is also supported on the XIO pins, but this feature is
rather limited due to the device and the driver (for example, the driver
interrupts on all edges).


## I²C

The recent images released by NTC have the I²C driver loaded by default and
exposes all three I²C buses but only 2 are usable. If running a stale image,
update with `sudo apt update && sudo apt upgrade`

- I²C #0: not available on the headers but has axp209 power control chip
- I²C #2: U13 pins 9 & 11
- I²C #2: U14 pins 25 & 26, has pcf8574 I/O extender


## SPI

The SPI driver is included on recent images but a DTBO (Device Tree Binary
Overlay) is required in order to create the `/dev/spi32766.0` device and connect
it to the pins.

- SPI2.0 or SPI32766.0: U14 pins 27, 28, 29, 30; only a single
  chip-select is supported

To enable, run the following:
```
mkdir -p /sys/kernel/config/device-tree/overlays/spi
cat /lib/firmware/nextthingco/chip/sample-spi.dtbo > /sys/kernel/config/device-tree/overlays/spi/dtbo
```

This needs to be done at each boot. A good location is to add the above into
`/etc/rc.local` before the `exit 0` statement.


# Buying

- The C.H.I.P. was directly distributed by NextThing Co but is not available
  anymore.

_The periph authors do not endorse any specific seller. These are only provided
for your convenience._
