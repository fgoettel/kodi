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
9. Adjust zoom settings to fit on the screen.

*Optional*: Once the system boots up, make sure that the SSH service is enabled and connect to the device over SSH using a SSH client of your choice. Log in using the default logon credentials: root/libreelec. Default address is "libreelec".

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

## Save user information
User information is saved at 
```
~/.kodi/userdata/
```
## Enable logging
[Enable logging of all keystrokes.](http://kodi.wiki/view/Log_file/Advanced#Turn_on_debugging_using_a_file_.28advancedsettings.xml.29)
Logfiles will be stored in `userdata/temp/`. 


## Control Volume via Samsung Remote Control
Some facts: 
* The volume can be controlled via amixer or kodi
  * Amixer `amixer set Digital 95%`
  * Kodi: eg. remote app: press +/- on audio
* The TV (Samsung) does not forward the volume key presses

*Idea*: Adjust the [keymap](http://kodi.wiki/view/Keymap) to let up/down control the volume.
1. Create `keymaps` folder in userdata: 
```
~/.kodi/userdata/keymaps
```
2. Create a `remote.xml` file with the following content:
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<keymap>
    <FullScreenVideo>
        <remote>
            <down>VolumeDown</down>
            <up>VolumeUp</up>
        </remote>
    </FullScreenVideo>
</keymap>
```
3. reboot


## Configure OSD
Deactivate OSD after 5 seconds.

### Copy skin to user modifiable location
1. Identify skin (e.g. `find / -name DialogSeekBar.xml` leads to it)
2. Copy it to user data. E.g.
```shell
cp -R /usr/share/kodi/addons/skin.estuary ~/.kodi/addons/skin.estuary.bla
```
3. Adjust the `addon.xml` inside the copied dir to fit the dir.
4. Enable inside addons 
5. Restart

### Adjust skin
1. Figure out, which OSDs are loaded (inspect the log).
2. Adjust the `<visible>` tag to let the OSD auto-disappear
E.g. insert system idle condition
```xml
...
<visible> [ condA | condB  | ... ] + !System.IdleTime(5) </visible>
```
Inside `VideoOSD.xml` and `DialogSeekBar.xml`

3. Save


## Stop when TV shutdown
Stop everything when TV is shutdown.
**TODO**
