# lede-pi0w-usb
Enabling USB Ethernet on the Pi0w with an install of LEDE.<br/>
It’s a little convoluted and I’m open to learning about optimisations.

## Requirements
* Raspberry Pi Zero W
* 512mb or greater MicroSD card
* Micro USB Cable
* Keyboard and display with adapters to connect up with micro USB and mini HDMI
* https://downloads.lede-project.org/releases/17.01.4/targets/brcm2708/bcm2708/lede-17.01.4-brcm2708-bcm2708-rpi-ext4-sdcard.img.gz (LEDE Disk image compatible with Raspberry Pi Zero W.  This is .gz zipped and will need extracting to the .img before flashing to the MicroSD card).

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
*I originally tried this with the aluminium Apple keyboard (A1243) and the Pi0w refused to recognise it.

## Connect to the new LEDE WiFi network
The previous commands should have enabled WiFi on the LEDE install and created a new unsecure wireless network called LEDE to connect to.
1. Once connected to the LEDE WiFi your device should have an IP address in the 192.168.1.x range.  Open your web browsers and navigate to http://192.168.1.1
2. Follow the instructions to set a password for the root user (so you can ssh into the Pi later)
3. c.	Navigate to [Network > Wireless] then click the “Scan” button.  Select your local WiFi with an active internet connection and provide the key needed for the router to connect.<br/>
<img src="https://github.com/smeathers/lede-pi0w-usb/raw/master/images/join_wifi.png" width="400"> <img src="https://github.com/smeathers/lede-pi0w-usb/raw/master/images/join_wifi_2-2.png" width="400"><br/>
4. Navigate to [System > Reboot] then click the “Perform reboot” button.  When the Pi comes back up it should now connect to your local WiFi network and the LEDE network will no longer be visible.

## With the Pi0w connected to your host WiFi network
The following commands can either be entered on the keyboard connected to the Pi or via a SSH session.  This loads the drivers for USB Ethernet then shut down the Pi.
```bash
opkg update
opkg install kmod-usb-gadget-eth
opkg install kmod-usb-dwc2
halt
```
At this point the keyboard and monitor should no longer be needed.

## Editing the config.txt file
With the MicroSD card back in the computer open the config.txt file in the root of the SD card.<br/>
<img src="https://github.com/smeathers/lede-pi0w-usb/raw/master/images/config-txt-diff.png" width="400"><br/>
At the end of the file add ```dtoverlay=dwc2``` to a new line, save, eject the MicroSD card and reinsert it into the Pi0w.

## Power the Pi0w via the USB port connected to a computer
<img src="https://github.com/smeathers/lede-pi0w-usb/raw/master/images/IMG_9306.jpg" width="400"><br/>
At this point the Computer should detect the Pi0w as a USB Ethernet device.  Once the correct drivers are loaded it should receive an IP address from the DHCP server running on the Pi0w allowing further SSH and LuCI configuration.

## Done
<img src="https://github.com/smeathers/lede-pi0w-usb/raw/master/images/IMG_9304.jpg" width="400"><br/>
Before and after.  The original Raspberry Pi Model B made a great portable router but the smaller size and single cable to the computer should make the Raspberry Pi Zero W much more practical.  Also configured with a USB WiFi adapter you can choose between USB Ethernet and a WiFi hotspot.

## References
#### USB Gadget Support<br/>
https://lede-project.org/docs/user-guide/usb_gadget<br/>
http://www.circuitbasics.com/raspberry-pi-zero-ethernet-gadget/<br/>
https://gist.github.com/gbaman/975e2db164b3ca2b51ae11e45e8fd40a<br/>
https://forum.lede-project.org/t/raspberry-pi-zero-w-ethernet-gadget-issue/2609<br/>
https://lede-project.org/packages/pkgdata/kmod-usb-dwc2

### Additional Reading
I use my Pi-Router as an ad-blocker and a WiFi hotspot, I found these links useful. 
#### Adblocking
https://github.com/openwrt/packages/tree/master/net/adblock/files<br/>
https://forum.lede-project.org/t/adblock-support-thread/507
#### USB WiFi Adaptor
https://www.andrewklau.com/openwrt-and-a-4-usb-wifi-adapter/

#### Static mac addresses
By default when the device boots the usb mac address is randomly generated.  This link explains how to add static mac addresses.
https://github.com/adron-s/QCA953x-usb-device-mode
The file you need to edit is /etc/modules.d/52-usb-gadget-eth
