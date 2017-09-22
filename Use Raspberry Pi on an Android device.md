# Android Raspberry Pi display over USB - Joshua Woehlke

Sometimes you really want to play with a Raspberry Pi, but don’t have a display, keyboard, or mouse handy, and the wifi isn’t configured correctly to just be able to SSH in. Invariably you spend an hour digging around for a keyboard or refreshing a wireless clients list, but this doesn’t have to be the case. After a quick one-time setup, everything you need to use a Raspberry Pi will already be in your pocket.
## The idea

Once configured, if you have an Android phone with USB tethering and a cable, you should be well-equipped to use your Pi. Bonus points if you have a bluetooth mouse and keyboard. We’ll be setting up a USB network interface on the Pi and installing a VNC server to pass a session over that interface, thus making your phone a Raspberry Pi display. By the end you should be able to just power up your Pi, plug in your phone, turn on USB tethering, and open up a full desktop.
## Setting up the network

Log into your Pi via SSH or open up a terminal in its GUI and pull up your network interfaces.

```
sudo nano /etc/network/interfaces
```

Paste the following onto the bottom of the file, then save and exit (ctrl-X, Y):

On your next restart, you should have a new interface when you type `ifconfig`. We’ve set this interface to have a static IP address, always `192.168.42.42`, which you will later use to start your VNC session or connect via SSH on your phone.

## Configuring VNC

VNC, or Virtual Network Computing, is a way of sharing a graphical desktop environment over a network, which in this case happens to be your phone’s USB cable. First, we need to install a VNC server onto the Raspberry Pi. We’ll be using TightVNC since raspberrypi.org has a tutorial for it and it’s easy to find help on forums.

```
sudo apt-get install tightvncserver
```

Next, use the command `tightvncserver` to configure VNC for your Pi. It should ask you for a password–be aware that TightVNC will truncate your password to eight characters. It does tell you this in the terminal, but it can be easy to miss and lead to many failed login attempts.

Lastly, we need the VNC server to start up every time the Pi starts so that you really do only need your phone. First, change into your /etc/init.d directory.

```
cd /etc/init.d
```

Create a new file called vncboot. You’ll need root privileges to change anything in this directory.

```
sudo nano vncboot
```

Paste the following into the file (change export USER=’pi’ to your username if not pi, and edit the screen resolution in the start) block if necessary):

```
 
#! /bin/sh
### BEGIN INIT INFO
# Provides:          vncboot
# Required-Start:    $local_fs
# Required-Stop:     $local_fs
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: Run tightvnc on boot
### END INIT INFO
 
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/opt/bin
export USER='pi'
 
eval cd ~$USER
 
. /lib/init/vars.sh
. /lib/lsb/init-functions
 
case "$1" in
  start)
    log_begin_msg "Starting VNC server"
    su $USER -c '/usr/bin/vncserver :1 -geometry 1680x1050 -depth 24'
    log_end_msg $?
    exit 0
    ;;
  stop)
    pkill Xtightvnc
    log_begin_msg "Stopping VNC server"
    log_end_msg $?
    exit 0
    ;;
  *)
    echo "Usage: /etc/init.d/vncboot {start|stop}"
    exit 1
    ;;
esac
 
```

Save this file, then update its permissions:

```
sudo chmod 755 vncboot
```

Finally, run the following command to add it to your startup:

```
sudo update-rc.d vncboot defaults
```

Reboot your Pi and it should be ready to rock.

## Getting connected

Now that one side of your setup is complete, you’ll need a VNC client on your phone. VNC Viewer seems plenty quick for this purpose and you can’t argue with the price. Optionally, you may also download an SSH client like JuiceSSH for those times when a GUI just isn’t necessary.

With your app downloaded, power up your Pi and connect your phone via a data USB cable. As your Pi boots up, you should get a notification that the phone is now connected as a media device. Go into your phone’s settings and turn on USB tethering.

Assuming you’ve given the Pi enough time to boot, you should now be ready to pull up your desktop. Open your VNC viewer app, connect to `192.168.42.42:1` (the `:1` is important here), and provide your password. If you just need to SSH, open up your SSH app and connect to `192.168.42.42`.

Congratulations! You now have a Raspberry Pi display, keyboard, and mouse even when you don’t physically have those items available.
