# Qt remote deployment and debugging
When create application on Qt using the raspberry pi 3 platform, Qt creator can remotely deploy the compiled application to the raspberry pi.

## Windows
This will not work on windows sadly referr to the winodws guide

## Enable SSH
Ensure ssh is enabled which can be found in the repo

## Etherent support
Ensure the raspberry pi has networking capabilities. Adding etherent docs can be found in this repo.
Take note of the IP address of the device.

## Using Qtrpi
This is a open source project that installs all the required tools for working with the raspberry pi and Qt.

### Clone the project
```
git clone https://github.com/neuronalmotion/qtrpi.git
cd qtrpi
```

### Configure your environment variables
The script supports multiple versions of Qt and Raspberry Pi. We want to use for rasberry pi compute module i.e V3
```
export QTRPI_QT_VERSION='5.9.1'
export QTRPI_TARGET_DEVICE='linux-rpi3-g++'
export QTRPI_TARGET_HOST='pi@192.168.0.108'
```

### Select the required module
Edit the below file qt-modules.txt and add or remove modules

### Run the config script
Once the environment variables are set, execute the following command (from your cloned repository of qtrpi):
```
./init-qtrpi-full.sh
```

### Check your installation
At the end of the execution of the script, you should have a new directory /opt/qtrpi with:
  * bin: that contains qmake-qtrpi, a symbolic link to the pre-configured qmake
  * raspbian: the sysroot
  * raspi: the toolchain, the preconfigured Qt, the precompiled Qt for Raspberry Pi


### Deploy on your Raspberry Pi
Once your Raspberry Pi is reachable within your network, execute:
```
./deploy-qtrpi.sh --prepare-rpi
```
This script will connect to your Raspbery Pi and install / update everything that is needed to run Qt applications.

At the end of the script, your Raspberry Pi should contain the directory /usr/local/qt5pi with the the precompiled Qt for Raspberry Pi.

###  Export the QtRpi bin path (optional)
Add the directory /opt/qtrpi/bin to your PATH.
```
export PATH=/opt/qtrpi/bin:$PATH # Add this line to your /home/user/.bashrc
source ~/.bashrc
```

### Setup Qt creator
QtRPI published a video on Youtube to describe the configuration of Qt Creator with QtRpi. With this configuration you will have:

A custom Qt kit that will let you cross-compile Qt applications (with the speed of your computer's CPUs) for your Raspberry Pi

An automatic deployment configuration that will copy the cross-compiled binary to your Raspberry Pi

A remote debugger that will enable you to execute your program step by step directly on your Raspberry Pi

The only step that you have to do before watching the video is to install the multi-architecture debugger (gdb-multiarch):

```
sudo apt-get install gdb-multiarch
```





