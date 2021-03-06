PiSetup
=======

This project contains files to simplify setting up your Raspberry PI.  This was created for a Raspberry PI B+ on January 5, 2015.

Setup Steps
-----------
####Set up your wifi network
View the example network configuration files in this project.

#####/etc/network/interfaces
```
auto lo

iface lo inet loopback
iface eth0 inet dhcp

allow-hotplug wlan0
auto wlan0


iface wlan0 inet static 
address 10.1.10.29
netmask 255.255.255.0
gateway 10.1.10.1
wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf

iface default inet dhcp
```

#####/etc/wpa_supplicant/wpa_supplicant.conf.
```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
	ssid="YOURSSID"
	psk="yourpassphrase"
	proto=RSN
	key_mgmt=WPA-PSK
	pairwise=CCMP
	auth_alg=OPEN
}
```

The *interfaces* file sets up your WiFi with a static IP at 10.1.10.29. Change this address to correspond to your wifi network.  The *wpa_supplicant.conf* file identifies your wifi network SSID and passphrase.<br>Once you have edited these files, reboot your PI.  When it comes back up you should be able to ping it from your desktop.  Next try pinging google.com from the PI.  Once you have access to the internet you can start installing components.
####Install TightVNC
Use apt-get to install TightVNC.
```
sudo apt-get install tightvncserver
```
Once it is installed, start it up.
```
tightvncserver
```
The first time the VNC Server runs it will ask you for a general password and an optional view-only password.
####Start the VNC server.
```
vncserver :0 -geometry 1920x1080 -depth 24
```
At this point you should be able to connect to you PI from a VNC client on your desktop.  I use Jump Desktop on my Macs, but there are plenty of free VNC clients for all platforms.
####Get the files from this repository
```
git clone https://github.com/wrhumph/PiSetup.git
```
This copies the files from this Github repository into a directory named PiSetup under the current directory.  I am assuming you installed it under your home directory in the examples that follow.  
In the PiSetup directory you will find a script file named "setup".  To use it, you need to make it executable.
```
cd ~/PiSetup
chmod a+x setup
```
This script will automate the steps below.  In each, I show the setup command to use as well as the various commands that the setup script will run for you, in case you want to go through the installations step-by-step.
####Set up the vnc server boot file
Do the following to configure the system to start the VNC server at startup.  
```
sudo cp ~/PiSetup/etc/init.d/vncboot /etc/init.d
sudo chmod 755 /etc/init.d/vncboot
sudo update-rc.d vncboot defaults
```
Or do it with the setup script.
```
./setup vnc
```
Reboot.  You will see a message during boot up that the VNC server is starting. should be able to connect from your VNC client.
####Install NodeJS
```
wget http://node-arm.herokuapp.com/node_latest_armhf.deb
sudo dpkg -i node_latest_armhf.deb
```
Or do it with the setup script.
```
./setup node
```
You can test NodeJs from the command line as follows.
```
node -v
```
####Install pi-blaster
Pi-blaster is a device driver that lets you control the PI GPIO by writing to /dev/pi-blaster.
```
git clone https://github.com/sarfata/pi-blaster.git
cd pi-blaster
sudo apt-get install autoconf
./autogen.sh
./configure
make
sudo make install
```
Or do it with the setup script.
```
./setup pi-baster
```
Using pi-blaster, you can turn GPIO pin 17 on, for example, with the following command.
```
echo "17=1" > /dev/pi-blaster
```

##Resources
Another Node library for GPIO: [buzzer](https://www.npmjs.com/package/onoff)
