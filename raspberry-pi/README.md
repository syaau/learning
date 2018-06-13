# RASPBERRY PI
## Getting Started
* You have to buy all parts separately. Raspberry pi is just a mother board without - SD Card, Power Supply, Monitor cables (HDMI) and casing.
* The raspberry pi doesn't work without a properly installed OS on a SD card.
* There are 2 leds - one is just a power led (red) and the other one (green) lits on activity. Make sure you see the green light and not just red.
* Perform a full format (FAT32) using a standard [SD card formatter](https://www.sdcard.org/downloads/formatter_4/) utility. This way you will know of any problem with SD card during the format process itself. Yup it takes a long time to format.
* The [NOOBS](https://www.raspberrypi.org/downloads/noobs/) OS runs just by copying files on the SD card formatted with FAT32, no need of building from image file.
* The [Raspbian Lite](https://www.raspberrypi.org/downloads/raspbian/) is enough for headless operation.
* [Etcher](https://etcher.io/) is a nice software to copy the OS image.
* You could place a file named `ssh` in the boot partition of the SD card to enable SSH on the raspberry pi.
* The default username is `pi` and password is `raspberry`.

## Deploying your application on Raspberry
There is a chance that you might corrupt your raspberry pi installation by cold boot. And most of the time there is no way you could avoid power cord being hacked without a proper shut down, since its headless. For mitigation you could run the raspberry pi on read only mode. 
* [Follow this article for a detailed run down on this process](https://hallard.me/raspberry-pi-read-only/).
* You can use the following commands to make your raspberry pi 
  fully read only. Taken from [here](https://learn.adafruit.com/read-only-raspberry-pi/):
  > `$ wget https://raw.githubusercontent.com/adafruit/Raspberry-Pi-Installer-Scripts/master/read-only-fs.sh`

  > `$ sudo bash read-only-fs.sh`

  This script will make the file system readonly. Remove some unwanted programs. Move the system files that need to write like dhcp to write to the tmpfs.
* Make sure you make your pi readonly only after making sure your application runs without any issue.
* In case you need to update your application, you can issue the following command to make the file system read/write.
  > `$ sudo mount -o remount,rw /`

  You can then update your application and once then remount it
  back to readonly.
  > `$ sudo mount -o remount,ro /`

