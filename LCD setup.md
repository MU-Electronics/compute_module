# Setting up the LCD Display
Setting up the offical raspberry pi 7" LCD can be done simply by following the below. 

### Setup LCD hardware side
Connect these pins together with jumper wires:
```
GPIO0 - CD1_SDA
GPIO1 - CD1_SCL
```
Then connect DISP1 port to the LCD display

#### Login as root
This just makes thing simplier
```
sudo su
```
#### Update the dt-blob.bin file
Run 
```
sudo wget https://goo.gl/iiVxuA -O /boot/dt-blob.bin
```
#### If it failed!
In some cases this will fail and if it does follow the below steps.

First find the IP of your raspberr pi for eth0 by running the below
```
ifconfig
```
Next Download https://goo.gl/iiVxuA to your local machine

Open a terminal and cd to the location where you downloaded the above file.

Then run the below where "192.168.0.195" is the IP address of your raspberry pi
```
scp dt-blob-disp1-only.bin pi@192.168.0.195:~
```

Now on your raspberry pi run the below
```
cp dt-blob-disp1-only.bin /boot/dt-blob.bin
```









