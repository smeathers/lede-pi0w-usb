# lede-pi0w-usb
Enabling USB Ethernet on the Pi0w with an install of LEDE.
It’s a little convoluted and I’m open to learning about optimisations.

## Requirements
* Raspberry Pi Zero W
* 512mb or greater MicroSD card
* Micro USB Cable
* Keyboard and display with adapters to connect up with micro USB and mini HDMI
* https://downloads.lede-project.org/releases/17.01.4/targets/brcm2708/bcm2708/lede-17.01.4-brcm2708-bcm2708-rpi-ext4-sdcard.img.gz (LEDE Disk image compatible with Raspberry Pi Zero W, this is .gz zipped and will need extracting to the .img before flashing to the MicroSD card).
## Flash micro SD card with LEDE image
I use Win32 Disk Imager, I assume you're already familiar with the image flashing process.
<img src="https://github.com/smeathers/lede-pi0w-usb/raw/master/images/flash-sd.png" width="300">
## Boot Raspberry Pi hooked up to display and with keyboard
Running the commands.
```bash
uci set wireless.radio0.disabled=0
uci commit
reboot
```
<img src="https://github.com/smeathers/lede-pi0w-usb/raw/master/images/IMG_9301.jpg" width="300"> <img src="https://github.com/smeathers/lede-pi0w-usb/raw/master/images/IMG_9300.jpg" width="300"><br/>
(I originally tried this with the aluminium Apple keyboard (A1243) but it refused to work.  The older plastic Apple keyboard worked fine (A1048)).


## Connect to the new LEDE WiFi Network
The previous commands should have enabled WiFi on the LEDE install and created a new unsecure wireless network called LEDE to connect to.
1. Once connected to the LEDE WiFi your device should have an IP address in the 192.168.1.x range.  If so open your web browsers and navigate to http://192.168.1.1
2. Follow the instructions to set a password for the root user (so you can ssh into the pi later)
3. c.	Navigate to [Network > Wireless] then click the “Scan” button.  Select your local WiFi with an active internet connection and provide the key needed for the router to connect.<br/>
<img src="https://github.com/smeathers/lede-pi0w-usb/raw/master/images/join_wifi.png" width="400"> <img src="https://github.com/smeathers/lede-pi0w-usb/raw/master/images/join_wifi_2-2.png" width="400"><br/>
4. Navigate to [System > Reboot] then click the “Perform reboot” button.  When the Pi comes back up it should now connect to your local WiFi network and the LEDE network will no longer be visible.
## Connecting to the Pi0w on your host WiFi network

## 
