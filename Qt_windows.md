# Qt remote deployment and debugging
When create applications on Qt using the raspberry pi 3 platform, Qt creator can remotely deploy the compiled application to the raspberry pi. To do this first a cross-compiler needs to be installed along with a compiled version of Qt.

## Pre-checks
### Mac / Linux
This guide will not work on mac / linux please referr to the mac / linux guide.

### Enable SSH on Pi
Ensure ssh is enabled on the pi, a guide to this can be found in the repo.

### Etherent support on Pi
Ensure the raspberry pi has networking capabilities. A guide to adding ethernet can be found in this repo. 
Boot up the PI and run the below, taking note of the IP address of the device.
```
ifconfig
```


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
To compile Qt source the below is reccommened by Qt
  * Supported compiler (Visual Studio 2012 or later, MinGW-builds gcc 4.9 or later)
  * Perl version 5.12 or later   [http://www.activestate.com/activeperl/]
  * Python version 2.7 or later  [http://www.activestate.com/activepython/]
  * Ruby version 1.9.3 or later  [http://rubyinstaller.org/]

## Install a toolchain
Download the following toolchain and install
http://sysprogs.com/files/gnutoolchains/raspberry/raspberry-gcc4.9.2-r4.exe

Read more here: http://gnutoolchains.com/raspberry/

# Install MinGW
No we will download MinGW:
https://download.qt.io/online/qtsdkrepository/windows_x86/desktop/tools_mingw/qt.tools.win32_mingw492/4.9.2-1i686-4.9.2-release-posix-dwarf-rt_v3-rev1.7z
or similar at https://download.qt.io/online/qtsdkrepository/windows_x86/desktop/tools_mingw/

Extract the content for "tools" directory to "C:/SysGCC/mingw32"

## Update sys-root
The compiling PC needs to have a copy of the some of the pi's content. As such we are going to take a local copy of these file. To do this navigate to "C:\SysGCC\Raspberry\TOOLS"

Run "UpdateSysRoot.bat" and click the "select" button.

Select "New Connection" and fill in the relivent info

  * Host: IP address of raspberry pi
  * Username: pi
  * Password: raspberry
  * Then click connect

Then click "Full Syncronise" then "Syncronise"; This will take awhile!

__Remember to re-synchronize sysroot after installing new libraries onto the pi or after changing the used image.__

## Setup msys2
Go to http://www.msys2.org/ and download msys2. Once download installed the program.

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

### Ensure the compiler is present
Run the below in the terminal we opened before, you should not get an error
```
which arm-linux-gnueabihf-gcc
```
Check we have gcc.exe, you should not get an error
```
C:/SysGCC/mingw32/bin/gcc.exe
```

## Building the Source
This stage is going to actually build the qt source code. The source can also be downloaded from @todo
### Get qt source
We will now download Qt's source code and checkout version 5.9.1.
```
cd C:\SysGCC\Raspberry
git clone https://github.com/qt/qt5.git
cd qt5
git submodules init
git submodule update
git checkout 5.9.1
git submodule update --init
cd qtwebengine
git submodule update --init
cd ../
```

### Setup qmake file 
Qt's qmake.config files need some tweaking to be able to cross compile on windows. The below documents what i did but not all if any may be neccessary. I need to go through this guide form scratch to identify what is required and what was not.

#### Edit 1
Copy the folder "C:\SysGCC\Raspberry\qt5\qtbase\mkspecs\linux-arm-gnueabi-g++f" and past into "C:\SysGCC\Raspberry\qt5\qtbase\mkspecs\linux-arm-gnueabi**hf**-g++"

Open the file "C:\SysGCC\Raspberry\qt5\qtbase\mkspecs\linux-arm-gnueabi**hf**-g++\qmake.conf" and replace all occurences of arm-linux-gnueabi- with arm-linux-gnueabihf-
For Example:
  *  arm-linux-gnueabi-gcc becomes arm-linux-gnueabihf-gcc
  *  arm-linux-gnueabi-g++ becomes arm-linux-gnueabihf-g++

#### Edit 2
Open the "C:\SysGCC\Raspberry\qt5\qtbase\mkspecs\win32-g++\qmake.conf" file and add "-U\_\_STRICT_ANSI\_\_" to QMAKE_CFLAGS.
For Example:
  * QMAKE_CFLAGS += -fno-keep-inline-dllexport **becomes** QMAKE_CFLAGS += -fno-keep-inline-dllexport -U\_\_STRICT_ANSI\_\_

#### Edit 3
This is most likely not required!
Edit "C:\SysGCC\Raspberry\qt5\qtbase\mkspecs\devices\linux-rasp-pi3-g++\qmake.conf"
Replace: 
QMAKE_LIBS_OPENGL_ES2   = $${VC_LINK_LINE} -lGLESv2
With:
QMAKE_LIBS_OPENGL_ES2   = $${VC_LINK_LINE} -lGLESv2 -lEGL

### Build the source
Now we can build the Windows tools for raspberry pi 3. 

Run the configuration script:
@todo update below path with "C:/SysGCC/Raspberry/qt5/configure"
```
"c:/development/qt_versions/qt5/configure" -skip qtscript -platform win32-g++ -release -device linux-rasp-pi3-vc4-g++ -sysroot c:/SysGCC/Raspberry/arm-linux-gnueabihf/sysroot -prefix /usr/local/qt5 -device-option CROSS_COMPILE=c:/SysGCC/Raspberry/bin/arm-linux-gnueabihf- -nomake examples -opensource -confirm-license -qt-xcb
```

### Check build success
The above **may** error with the below
```
Checking for valid makespec... ERROR: Cannot compile a minimal program. The toolchain or QMakeSpec is broken.
```
but just ignore it, so long as we have qmake we're good. To check we have qmake run the below.
```
qtbase/bin/qmake -v
```

### Make the source
Now to compile the source code for the rasberry pi
```
make
```
After the above has finished run the below
```
make install
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




