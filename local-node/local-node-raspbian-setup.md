## Install and configure Raspbian
[2020.06.03]

Location: Follow these instructions on your **LOCAL NODE**

Download (Raspbian Buster Lite at the time of this tutorial)

Create the bootable sdcard

**Be careful with this step!  Don't delete things you don't want to delete.  Be sure of your of=/dev/(yourdevhere) link.**

These commands may help you locate your target.
```
$ df -h
$ lsblk
```

Copy the raspbian zip file onto sdcard (this will erase everything on the sdcard)  dd is one option, use a disk imaging tool you're comfortable with.
```
$ unzip 2020-02-13-raspbian-buster-lite.zip
$ sudo dd if=2020-02-13-raspbian-buster-lite.img of=/dev/(yourdevicehere) bs=4M status=progress conv=fsync
```
**or** the same thing in one step
```
$ unzip -p 2020-02-13-raspbian-buster.zip | sudo dd of=/dev/(yourdevhere) bs=4M status=progress conv=fsync
```
Note `2020-02-13-raspbian-buster.zip` file name may be different for you.


### setup headless
**Enable ssh:** place a file named ssh, without any extension, onto the boot partition before booting.

(optionally, enable ssh)
Open a terminal to the boot partion then...
```
$ touch ssh
```

**WiFi** (hardwired ethernet recomended)
(optionally, preconfigure wifi)
Open a terminal to the **boot partion** then...

```
$ vim wpa_supplicant.conf
```
Fill in your details.. 
```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=US

network={
 ssid="Name-of-your-wireless-LAN"
 psk="Password-for-your-wireless-LAN"
}
```
Be sure to change `US` to your country code.  And use your `Name-of-your-wireless-LAN` and `Password-for-your-wireless-LAN`.

Move your sdcard into your pi and power it up.


(after booting, optionally to find the ip address of your pi to access ssh, or you can use a keyboard and monitor, or your router may have a feature you'd be able to find your PI's IP address from.)
```
$ sudo arp-scan 192.168.1.0/24 -I eth0
```
Assuming you have `arp-scan` installed and your nework is `192.168.1.0/24` and your pi is connected on the same network as your `eth0` connetion.


 
login

**user** pi

**password** raspberry

Initial Configuration
```
$ sudo raspi-config
```
- set password
- set localization / keyboard
- set wifi connection (if not using ethernet)
- enable ssh [if not already enabled from boot ssh file]

If you're using ssh it's a good idea to set only key based ssh.

note: once you're connected with wired or wireless you may find ssh easier to continue from here for copy/paste of commands.

Update system
```
$ sudo apt update
$ sudo apt upgrade
```

### helpful links
https://www.raspberrypi.org/documentation/configuration/wireless/headless.md
