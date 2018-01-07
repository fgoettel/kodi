# kodi
Individualize Kodi

## Hardware Setup 
Raspberry Pi3 + Hifiberry Amp2. 

[HifiBerry Amp2 has the same S/W config as the DAC+.](https://www.hifiberry.com/shop/boards/hifiberry-amp2/) 

## Installation
[Inspired by the HifiBerry instructions.](https://www.hifiberry.com/build/documentation/libreelec-installation-and-configuration/)

### Install LibreElec / Kodi on the Raspberry
1. Download installer from libreelec.tv
2. Launch the installer.
3. Select your RPi model and the LibreELEC version you want to install.
4. Click Download and specify the download location.
5. Once the download is complete, select the target device and click Write.
6. Confirm writing the image onto the selected SD card.
7. Once the image is written onto the card, disconnect it from your computer and plug it into the RPi.
8. Connect all the cables and power it.
9. Once the system boots up, make sure that the SSH service is enabled and connect to the device over SSH using a SSH client of your choice. Log in using the default logon credentials: root/libreelec. Default address is "libreelec".

### Setup the HifiBerry
1. Gain write privileges: 
```bash
mount -o remount,rw /flash
```
2. Edit the config.txt file using nano: 
```bash
nano /flash/config.txt
```
Add the following line:
```
dtoverlay=hifiberry-dacplus
```
3. Save and exit.
4. Reboot the system.
5. Go to System and enter Settings, select Systems, navigate to Audio output device.
6. Select ALSA:Default(snd_rpi_hifiberry_dacplus Analog) as the output device.

In same cases the volumer mixer level is set to 100% by default. This can result in distortions of the music output. Therefore you should set this to around 80%. Login via SSH and use the following command:
```
amixer -c 0 set Digital 80%
```
You may want to experiment with the percentage volume in order to tune it to your desired preference.


## Configure OSD
Deactivate OSD after 5 seconds.
**TODO**

## Control Value via Remote Control
Adjust the [keymap](http://kodi.wiki/view/Keymap) to control the volume.
**TODO**
