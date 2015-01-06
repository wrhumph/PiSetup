PiSetup
=======

This project contains files to simplify setting up your Raspberry PI.  This was created for a Raspberry PI B+ on January 5, 2015.

Setup Steps
-----------
####Set up your wifi network
View the example network configuration files in this project.

/etc/network/interfaces
/etc/wpa_supplicant/wpa_supplicant.conf.

The interfaces file sets up your WiFi with a static IP at 10.1.10.29. Change this address to correspond to your wifi network.  The wpa_supplicant.conf file identifies your wifi network SSID and passphrase.<br>Once you have edited these files, reboot your PI.  When it comes back up you should be able to ping it from your desktop.  Next try pinging google.com from the PI.  Once you have access to the internet you can start installing components.
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
This copies the files from this Github repositor into a directory named PiSetup under the current directory.  I am assuming you installed it under your home directory in the examples that follow.
####Set up the vnc server boot file
Copy ~/PiSetup/etc/init.d/vncbot into /etc/init.d.  This will run the VNC server at startup.
```
    sudo cp ~/PiSetup/etc/init.d/vncboot /etc/init.d
```
####Install NodeJS
```
    wget http://node-arm.herokuapp.com/node_latest_armhf.deb
    sudo dpkg -i node_latest_armhf.deb
```
You can test it from the command line and initialize the node package manager.
```
    node -v
    npm install
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
Using pi-blaster, you can turn GPIO pin 17 on, for example, with the following command.
```
    echo "17=1" > /dev/pi-blaster
```

That's it.
