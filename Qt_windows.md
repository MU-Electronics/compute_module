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

## Install opengl on the pi
We need to install opengl on the pi if you have Raspbian Jessie installed
```
sudo su
apt-get install libgl1-mesa-dri xcompmgr
apt-get install libalut-dev libalutO mesa-utils
apt-get install libgl1-mesa-dev
apt-get install libx11-dev libglib2.0-dev
```
Now enable opengl, run the below and select Advanced Options -> GL Driver -> Full
```
raspi-config
```

## Install depends
  * Supported compiler (Visual Studio 2012 or later, MinGW-builds gcc 4.9 or later)
  * Perl version 5.12 or later   [http://www.activestate.com/activeperl/]
  * Python version 2.7 or later  [http://www.activestate.com/activepython/]
  * Ruby version 1.9.3 or later  [http://rubyinstaller.org/]

## Install a toolchain
Download the following toolchain and install
http://sysprogs.com/files/gnutoolchains/raspberry/raspberry-gcc4.9.2-r4.exe

Read more here: http://gnutoolchains.com/raspberry/

# Install MinGW
Download https://download.qt.io/online/qtsdkrepository/windows_x86/desktop/tools_mingw/qt.tools.win32_mingw492/4.9.2-1i686-4.9.2-release-posix-dwarf-rt_v3-rev1.7z
or similar at https://download.qt.io/online/qtsdkrepository/windows_x86/desktop/tools_mingw/

Exstract the content for "tools" directory to "C:/SysGCC/mingw32"

## Update sys-root
Navigate to "C:\SysGCC\Raspberry\TOOLS"

Run "UpdateSysRoot.bat" and click the "select" button.

Select "New Connection" and fill in the relivent info

  * Host: IP address of raspberry pi
  * Username: pi
  * Password: raspberry
  * Then click connect

Then click "Full Syncronise" then "Syncronise"; This will take awhile!

__Remember to re-synchronize sysroot after installing new libraries on your device or after changing the used image.__

## Setup msys2
Open "C:\msys64\msys2.exe" and run
```
pacman -S make perl pkg-config diffutils
export PATH=$PATH:/c/SysGCC/mingw32/bin
export PATH=$PATH:/c/Python27
export PATH=$PATH:/c/SysGCC/Raspberry/bin
mkdir -p /c/SysGCC/Raspberry/qt-build
cd /c/SysGCC/Raspberry/qt-build
```
^^ Keep this terminal open we'll be using it through out.

## Building the Source
### Get qt source
```
cd C:\SysGCC\Raspberry
git clone https://github.com/qt/qt5.git
cd qt5
git submodules init
git submodule update
git checkout 5.9.1
```

### Ensure compile present
Run the below in the terminal we opened before
```
which arm-linux-gnueabihf-gcc
```
Check we have gcc.exe
```
C:/SysGCC/mingw32/bin/gcc.exe
```

### Setup qmake file 
Copy the folder "C:\SysGCC\Raspberry\qt5\qtbase\mkspecs\linux-arm-gnueabi-g++f" and past into "C:\SysGCC\Raspberry\qt5\qtbase\mkspecs\linux-arm-gnueabihf-g++"

Open qmake.conf and replace all occurences of arm-linux-gnueabi- with arm-linux-gnueabihf-
For Example:
  *  arm-linux-gnueabi-gcc becomes arm-linux-gnueabihf-gcc
  *  arm-linux-gnueabi-g++ becomes arm-linux-gnueabihf-g++

Edit "C:\SysGCC\Raspberry\qt5\qtbase\mkspecs\devices\linux-rasp-pi3-g++\qmake.conf"
Replace: 
QMAKE_LIBS_OPENGL_ES2   = $${VC_LINK_LINE} -lGLESv2
With:
QMAKE_LIBS_OPENGL_ES2   = $${VC_LINK_LINE} -lGLESv2 -lEGL


Open the "C:\SysGCC\Raspberry\qt5\qtbase\mkspecs\win32-g++\qmake.conf" file and add "-U\_\_STRICT_ANSI\_\_" to QMAKE_CFLAGS.
For Example:
  * QMAKE_CFLAGS += -fno-keep-inline-dllexport becomes QMAKE_CFLAGS += -fno-keep-inline-dllexport -U\_\_STRICT_ANSI\_\_


### Build the source
Now we can build the Windows tools. Create a directory (e.g. qt-build) and run the configuration script from there:
```
"c:/development/qt_versions/qt5/configure" -skip qtscript -platform win32-g++ -release -device linux-rasp-pi3-g++ -sysroot c:/SysGCC/Raspberry/arm-linux-gnueabihf/sysroot -prefix /usr/local/qt5 -device-option CROSS_COMPILE=c:/SysGCC/Raspberry/bin/arm-linux-gnueabihf- -nomake examples -opensource -confirm-license
```

### Check build success
The above may error with the below
```
ERROR: Cannot specify both -xplatform and -device.
```
but so long as we have qmake we're good. To check we have qmake run the below.
```
qtbase/bin/qmake -v
```

### Make the source
Now to compile the source code for the rasberry pi
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




