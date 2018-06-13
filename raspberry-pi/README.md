# RASPBERRY PI
* You have to buy all parts separately. Raspberry pi is just a mother board without - SD Card, Power Supply, Monitor cables (HDMI) and casing.
* The raspberry pi doesn't work without a properly installed OS on a SD card.
* There are 2 leds - one is just a power led (red) and the other one (green) lits on activity. Make sure you see the green light and not just red.
* Perform a full format (FAT32) using a standard [SD card formatter](https://www.sdcard.org/downloads/formatter_4/) utility. This way you will know of any problem with SD card during the format process itself. Yup it takes a long time to format.
* The [NOOBS](https://www.raspberrypi.org/downloads/noobs/) OS runs just by copying files on the SD card formatted with FAT32, no need of building from image file.
* The [Raspbian Lite](https://www.raspberrypi.org/downloads/raspbian/) is enough for headless operation.
* [Etcher](https://etcher.io/) is a nice software to copy the OS image.
* You could place a file named `ssh` in the boot partition of the SD card to enable SSH on the raspberry pi.
* The default username is `pi` and password is `raspberry`.
