# Setting up the Ethernet
Below goes through how to setup the ethernet interface

## Setting up te hardware
We can setup the ethernet hardware by connect the following pins to the following ports.

| ENC28j60 | Compute Module IO Board |
| ------------- |:-------------:|
| CE0 | GPIO8 |
| MISO | GPIO9 |
| MOSI | GPIO10 |
| SCK | GPIO11 |
| Interrupt | GPIO25 |
| 3.3V | 3.3V |
| GND | GND |


## Setting up the OS

Now the os needs to be setup

### Login as root
This just makes thing simplier
```
sudo su
```

### Setting up the IO ports
Run the below then select "interface options", then select "SPI", then select "Enable"
```
raspi-config
```

### Setting up the ethernet driver
Run the below
```
nano /boot/config.txt	
```
Then add to the top of the file
```
dtoverlay=enc28j60 
```

#### Set static mac address (iface eth0 inet manual)
The ethernet phy does not come with a its own mac address therefore we will set a static mac address. 
NOTE: This is not the best method but will work for now

#### Login as root
This just makes thing simplier
```
sudo su
```
#### Find the current dynamic mac address
Run the below to find the current mac address which was created randomly as we will use this for the new static mac address. For example 32:23:eb:76:e1:45

```
ifconfig -a | awk '/HWaddr/ {print "Interface: " $1 "\t MAC: " $NF}'
```

We'll cheat and use a tools to change the mac address. Run the below and when prompted select yes.

```
apt-get install macchanger
```

Now ensure the mac address is changed on every boot up. Run the below

```
nano /etc/rc.local 
```

Then at the top of the file past the below

```
ifconfig eth0 down
macchanger -m 32:23:eb:76:e1:45 eth0
ifconfig eth0 up
```






