# Booting to applications

When developing a product for products it is important that the QuePi based design boots straight into the applications design for it to provide a fluid and safe experience. This guide is going to assume that your application has a GUI with the potential of sound.

To ensure the embedded processor is not over whelmed by the OS alone "Raspbian Jessie Lite" is going to be used. As "Raspbian Jessie Lite" does not provide a window manager one will be installed along with support for audio.

## Dependences

First ensure that you have installed the OS and ran through the following processes

* Attached a monitor or [Serial LCD](LCD%20setup.md)
* A valid network connection wireless or [wired](Ethernet.md)
* [SSH enabled](ssh.md)

## Install addtional software

To give  "Raspbian Jessie Lite" the ability to load GUI interface we need to install some additional software

- X: This is the display server
- Openbox: A light weight window manager
- Your application (of course)
- PulseAudio: For sound output

Run the below to install the above (minus your application). Notices the "--no-install-recommends" tag below, this stops additional un-required packages being installed.

```bash
sudo apt-get update
sudo apt-get install --no-install-recommends xorg openbox pulseaudio
```

## Add user to audio group

We now add our user to the audio group which allows pulse audio to start up and control the sound for applications.

```bash
sudo usermod -G audio -a $USER
```

## Install your program

Place the compiled software files in

> /opt/app

To make the app folder and give the correct ownership run the below

```bash
sudo mkdir /opt/app
sudo chown -R electronics:electronics /opt/app/
```

### Upload your application

The below demonstrates how to update your application to the QuePi. Obviously replace "/path/to/program/local/pc" and "8.8.8.8".

```bash
scp -rp /path/to/program/local/pc/* pi@8.8.8.8:/opt/app
```

## Program loader

This next stage will make sure your program is load with the correct window manager. As such next we will write a small shell script; begin by make the file

```bash
sudo nano /opt/kiosk.sh
```

Then past in the below (press Control+X to exit)

```bash
#!/bin/bash

xset -dpms
xset s off
openbox-session &
start-pulseaudio-x11

while true; do
  /opt/app/GasRig
done
```

Now ensure the permissions are correct for the new shell script

```bash
sudo chmod +x /opt/kiosk.sh
```

### Test program loader shell

We can test that your program is loading correcting by running the below in the terminal

```bash
exec sudo -u pi startx /etc/X11/Xsession /opt/kiosk.sh -- -nocursor
```

Notice the -nocursor flag for touchscreen as you don't wont a curse showing

**Ensure you run this on the QuePi directly and not over SSH unless you have Xorg set up correctly for your session**

**The above command will not run from root, run when logged into the pi user**

## Load program loader shell at boot up



## Remove boot up text





