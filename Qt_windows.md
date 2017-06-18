# Qt remote deployment and debugging
When create application on Qt using the raspberry pi 3 platform, 

Qt creator can remotely deploy the compiled application to the 

raspberry pi.

## Pre-checks
### Mac / Linux
This will not work on mac / linux please referr to the mac / linux guide.

### Enable SSH on Pi
Ensure ssh is enabled which can be found in the repo

### Etherent support on Pi
Ensure the raspberry pi has networking capabilities. Adding 

etherent docs can be found in this repo.
Take note of the IP address of the device.

## Install depends
  * Supported compiler (Visual Studio 2012 or later, MinGW-builds gcc 4.9 or later)
  * Perl version 5.12 or later   [http://www.activestate.com/activeperl/]
  * Python version 2.7 or later  [http://www.activestate.com/activepython/]
  * Ruby version 1.9.3 or later  [http://rubyinstaller.org/]

## Install a toolchain
Download the following toolchain and install
http://sysprogs.com/files/gnutoolchains/raspberry/raspberry-gcc4.9.2-r4.exe

Read more here: http://gnutoolchains.com/raspberry/

## Update sys-root
Navigate to "C:\SysGCC\Raspberry\TOOLS"

Run "UpdateSysRoot.bat" and click the "select" button.

Select "New Connection" and fill in the relivent info

  * Host: IP address of raspberry pi
  * Username: pi
  * Password: raspberry
  * Then click connect

Then click Syncronise; This will take awhile!

__Remember to re-synchronize sysroot after installing new libraries on your device or after changing the used image.__

## Building the Source
### Get qt source
```
cd C:\SysGCC\Raspberry
git clone https://github.com/qt/qt5.git
cd qt5
git submodules init
git submodule update
```

### Ensure compile present
open "C:\MinGW\msys\1.0\msys.bat" and run
```
which arm-linux-gnueabihf-gcc
```

### Setup qmake file
Open the "C:\SysGCC\Raspberry\qt5\qtbase\mkspecs\linux-arm-gnueabi-g++\qmake.conf" file and replace all occurences of arm-linux-gnueabi- with arm-linux-gnueabihf-
For Example:
  *  arm-linux-gnueabi-gcc becomes arm-linux-gnueabihf-gcc
  *  arm-linux-gnueabi-g++ becomes arm-linux-gnueabihf-g++

Open the "C:\SysGCC\Raspberry\qt5\qtbase\mkspecs\win32-g++\qmake.conf" file and add "-U\_\_STRICT_ANSI\_\_" to CXXFLAGS.
For Example:
  * $$QMAKE_CFLAGS becomes $$QMAKE_CFLAGS -U\_\_STRICT_ANSI\_\_

### Build the source
Now we can build the Windows tools. Create a directory (e.g. qt-build) and run the configuration script from there:
```
mkdir ../qt-build

cd ../qt-build

../qt5/configure -platform win32-g++ -xplatform linux-arm-gnueabi-g++ -release -opengl es2 -device linux-rasp-pi3-g++ -sysroot C:/SysGCC/Raspberry/arm-linux-gnueabihf/sysroot -prefix /usr/local/qt5
```

```
./configure -skip qtscript -platform win32-g++ -xplatform linux-arm-gnueabihf-g++ -release -device linux-rpi3-g++ -sysroot C:/SysGCC/Raspberry/arm-linux-gnueabihf/sysroot -prefix /usr/local/qt5 -device-option CROSS_COMPILE=C:/SysGCC/Raspberry/bin/arm-linux-gnueabihf- -nomake examples -opensource -confirm-license
```

### Check build success
run the below
```
qtbase/bin/qmake -v
```

### Make the source
```
make && make install
```

## Configuring Qt creator

### Add tool chain
Qt -> Tools -> Options -> Build & Run -> Tool Chain
Add-> GCC -> Select C:\SysGCC\Raspberry\bin\arm-linux-gnueabihf-g++.exe

### Adding Qt version

### Add linux (rasberry pi) device 
Qt -> Tools -> Options -> Devices 
Add -> Generic Linux Device

  * Identify: Raspberry Pi
  * IP: 192.168.0.108
  * Username: pi
  * Password: raspberry

## Configuring project

### Setup build

### Setup deploy




