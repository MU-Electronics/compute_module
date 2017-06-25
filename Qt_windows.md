# Qt remote deployment and debugging

When create applications on Qt for the raspberry pi 3 platform compute module 3 in particular is what we use; Qt creator can locally compile the application and remotely deploy the compiled application to the raspberry pi. This speeds up development massively especially when working with hardware (ADC, DAC etc).

This guide will take you step by step how to setup your windows environment to cross comply and remotely deploy to your pi3.

## Pre-checks

### Mac / Linux

This guide will not work on mac / linux please refer to the mac / linux guide which will be wrote and stored in this repo.

### Enable SSH on Pi

Ensure ssh is enabled on the pi, a guide to this can be found in the repo.

### LCD Display

If your application ha a gui you'll need an lcd, this repo has a guide to getting the official pi 7" touch screen working.

###  Networking capabilities on Pi

Ensure the raspberry pi has networking capabilities. A guide to adding Ethernet to the CM3 can be found in this repo. 
Boot up the PI and run the below, taking note of the IP address for the device.

```bash
ifconfig
```

## 1.0 Prepping the Pi

Before starting we need to prep the pi ready for the tasks ahead.
First run the command below and perform the following

- Advanced Options -> Memory split -> 256
- Advanced Options -> GL Driver -> G3 No opengl (Most stable atm)

```bash
sudo raspi-config
```

 Next run the below code to prep your pi.

```bash
# uncomment deb-src line
sudo nano /etc/apt/sources.list

# Linker tries to link again installed qt 5.3 so remove it we want 5.8
sudo su
apt-get remove libqt5*
exit

# remove gstreamer
sudo su
apt-get remove gstreamer*
exit

# update
sudo apt-get update
sudo apt-get upgrade

# install qt dependencies @todo remove packages that are not required!
# Possable not required: sudo apt-get build-dep qt4-x11
# Possable not required: sudo apt-get build-dep libqt5gui5
# Possable not required: libraspberrypi-dev qtdeclarative5-dev libxcb1 libxcb1-dev
# Possable not required: sudo apt-get install -y apt-transport-https
# libgl1-mesa-dev
sudo apt-get install libudev-dev libinput-dev libts-dev libxcb-xinerama0-dev libxcb-xinerama0 libsmbclient-dev libssh-dev libv4l-dev libboost1.55-all-dev libbz2-dev libruby2.1 libscsynth1 libxcb-icccm4 libxcb-keysyms1 libxcb-randr0 libxcb-render-util0 libxcb-xkb1 libxkbcommon-x11-0 ruby2.1 rubygems-integration supercollider-server xfonts-100dpi xfonts-75dpi xfonts-scalable libraspberrypi-dev

sudo apt-get install libfontconfig1-dev libdbus-1-dev libfreetype6-dev libudev-dev libicu-dev libsqlite3-dev libxslt1-dev libssl-dev libasound2-dev libavcodec-dev libavformat-dev libswscale-dev libgstreamer0.10-dev libgstreamer-plugins-base0.10-dev gstreamer-tools gstreamer0.10-plugins-good gstreamer0.10-plugins-bad libraspberrypi-dev libpulse-dev libx11-dev libglib2.0-dev libcups2-dev freetds-dev libsqlite0-dev libpq-dev libiodbc2-dev libmysqlclient-dev firebird-dev libpng12-dev libjpeg9-dev libgst-dev libxext-dev libxcb1 libxcb1-dev libx11-xcb1 libx11-xcb-dev libxcb-keysyms1 libxcb-keysyms1-dev libxcb-image0 libxcb-image0-dev libxcb-shm0 libxcb-shm0-dev libxcb-icccm4 libxcb-icccm4-dev libxcb-sync1 libxcb-sync-dev libxcb-render-util0 libxcb-render-util0-dev libxcb-xfixes0-dev libxrender-dev libxcb-shape0-dev libxcb-randr0-dev libxcb-glx0-dev libxi-dev libdrm-dev libssl-dev libxcb-xinerama0 libxcb-xinerama0-dev libxkbcommon-dev libxcb-xkb-dev libxcb-xkb1 libfontconfig1-dev

sudo apt-get dist-upgrade
sudo apt-get install raspi-gpio
sudo apt-get install xcompmgr libgl1-mesa-dri 


# fix old egl
#sudo rm /usr/lib/arm-linux-gnueabihf/libEGL.so.1.0.0 /usr/lib/arm-linux-gnueabihf/libGLESv2.so.2.0.0
#sudo ln -s /opt/vc/lib/libEGL.so /usr/lib/arm-linux-gnueabihf/libEGL.so.1.0.0
#sudo ln -s /opt/vc/lib/libGLESv2.so /usr/lib/arm-linux-gnueabihf/libGLESv2.so.2.0.0
```

## 2.0 Install depends on local machine

To compile Qt source the below is reccommened by Qt

- Supported compiler (Visual Studio 2012 or later, MinGW-builds gcc 4.9 or later)
- Perl version 5.12 or later   [http://www.activestate.com/activeperl/]
- Python version 2.7 or later  [http://www.activestate.com/activepython/]
- Ruby version 1.9.3 or later  [http://rubyinstaller.org/]

## 3.0 Install a toolchain

Download the following toolchain and install
http://sysprogs.com/files/gnutoolchains/raspberry/raspberry-gcc4.9.2-r4.exe

Read more here: http://gnutoolchains.com/raspberry/

## 4.0 Install MinGW

Now we will download MinGW:
https://download.qt.io/online/qtsdkrepository/windows_x86/desktop/tools_mingw/qt.tools.win32_mingw492/4.9.2-1i686-4.9.2-release-posix-dwarf-rt_v3-rev1.7z
or similar at https://download.qt.io/online/qtsdkrepository/windows_x86/desktop/tools_mingw/

Extract the content for "tools" directory to "C:/SysGCC/mingw32"

## 5.0 Update sys-root

The compiling PC needs to have a copy of the some of the pi's libs. As such we are going to take a local copy of these file. To do this navigate to:

> "C:\SysGCC\Raspberry\TOOLS"

Run "UpdateSysRoot.bat" and click the "select" button.

Select "New Connection" and fill in the relevant info

- Host: IP address of raspberry pi
- Username: pi
- Password: raspberry
- Then click connect

Then click then "Syncronise"; This will take awhile!

**Note:**  The above program can be very slow and sometimes crash half way through your hour download. You can use other programs such as WinSCP (remembering to use scp over ftp etc). WinSCP can download multiple files at once making it quite quick at downloading the required files. WinSCP also has a synchronise feature that can compare timestamps on your pi files with your locally sync files, this means when u update you pi you dont have to re-sync everything, P.S massive time saver!

 Synchronise the below folders:

> * /lib to C:\SysGCC\Raspberry\arm-linux-gnueabihf\sysroot\lib
> * /usr/include -> C:\SysGCC\Raspberry\arm-linux-gnueabihf\sysroot\lib\include 
> * /usr/lib -> C:\SysGCC\Raspberry\arm-linux-gnueabihf\sysroot\lib\lib
> * /usr/local/include -> C:\SysGCC\Raspberry\arm-linux-gnueabihf\sysroot\usr\local\include 
> * /usr/local/lib -> C:\SysGCC\Raspberry\arm-linux-gnueabihf\sysroot\usr\local\lib
> * /opt to C:\SysGCC\Raspberry\arm-linux-gnueabihf\sysroot\opt

__Remember to re-synchronize sysroot after installing new libraries onto the pi or after changing the used image.__

## 6.0 Setup msys2

Go to http://www.msys2.org/ and download msys2. Once downloaded installed the program.

Open "C:\msys64\msys2.exe" and run. 

```bash
pacman -S make perl pkg-config diffutils
export PATH=$PATH:/c/SysGCC/mingw32/bin
export PATH=$PATH:/c/Python27
export PATH=$PATH:/c/SysGCC/Raspberry/bin
export PATH=$PATH:/c/Qt/Tools/QtCreator/bin


export QT_XKB_CONFIG_ROOT=C:\SysGCC\Raspberry\arm-linux-gnueabihf\sysroot\usr\share\X11\xkb
```

^^ Keep this terminal open we'll be using it through out.

### 6.1 Ensure the compiler is present

Run the below in the terminal we opened before, you should not get an error

```bash
which arm-linux-gnueabihf-gcc
```

Check we have gcc.exe, you should not get an error

```bash
C:/SysGCC/mingw32/bin/gcc.exe
```

## 7.0 Building the Source

This stage is going to build and install the qt source code. 

### 7.1 Get qt source

The source can also be downloaded from https://download.qt.io/archive/qt/5.8/5.8.0/single/qt-everywhere-opensource-src-5.8.0.tar.gz.mirrorlist
Extract the code to any location in this case I’ll be used C:\development\qt\qt_versions\qt-everywhere-opensource-src-5.8.0

```bash
cd C:\development\qt\qt_versions\
mkdir qt5.8_build
```

### 7.2 Setup qmake file

Qt's qmake.config files need some tweaking to be able to cross compile on windows. The below documents what I did but may not be necessary. 

#### 7.2.1 Edit 1

Go to:

> C:\SysGCC\Raspberry\qt5\qtbase\mkspecs\linux-arm-gnueabi-g++f 

Open file qmake.conf and eeplace all occurrences of **arm-linux-gnueabi-** with **arm-linux-gnueabihf-** 

For Example:

> - arm-linux-gnueabi-gcc becomes arm-linux-gnueabihf-gcc
> - arm-linux-gnueabi-g++ becomes arm-linux-gnueabihf-g++

#### 7.2.2 Edit 2

Open the below file

> "C:\SysGCC\Raspberry\qt5\qtbase\mkspecs\win32-g++\qmake.conf" 

Add **"-U\_\_STRICT_ANSI\_\_"** to QMAKE_CFLAGS.

For Example:

> QMAKE_CXXFLAGS = $$QMAKE_CFLAGS **-U\_\_STRICT_ANSI\_\_**

We can also add the c11 tags to the above to ensure compilation in c11

> QMAKE_CXXFLAGS = $$QMAKE_CFLAGS -U\_\_STRICT_ANSI\_\_ **-std=c++11**

#### 7.2.3 Edit 3

Now we need to make sure that the device configuration file links are relative to your PC and not the pi for compiling.

Go to file: 

> C:\development\qt\qt_versions\qt-everywhere-opensource-src-5.8.0\\**qtbase\mkspecs\devices\linux-rpi3-g++\qmake.conf**

Edit the following lines, NOTE: Ensure VC_LIBRARY_PATH and VC_INCLUDE_PATH lines are above QMAKE_LFLAGS. Commenting out the existing lines

> VC_LIBRARY_PATH         = $$[QT_SYSROOT]/opt/vc/lib
> VC_INCLUDE_PATH         = $$[QT_SYSROOT]/opt/vc/include
>
> QMAKE_LFLAGS           += -L$${VC_LIBRARY_PATH}
>
> VC_LINK_LINE            = -L$${VC_LIBRARY_PATH}

### 7.3 Prep the source / environment

Now we can build the Windows tools for raspberry pi 3 most noticeable "qmake", run the configuration script:

```bash
"../qt-everywhere-opensource-src-5.8.0/configure" -platform win32-g++ -xplatform linux-arm-gnueabi-g++ -release -opengl es2 -device linux-rpi3-g++ -sysroot C:/SysGCC/Raspberry/arm-linux-gnueabihf/sysroot -prefix /usr/local/qt5
```

#### 7.3.1 Check qmake

The above **may** error but just ignore it, so long as we have qmake we're good. To check we have qmake run the below:

```bash
qtbase/bin/qmake -v
```

### 7.4 Reconfigure the project

Now we re-run the configuration script but now we add the device information; rum the below command:

```bash
"../qt-everywhere-opensource-src-5.8.0/configure" -platform win32-g++ -xplatform linux-arm-gnueabi-g++ -debug -qt-zlib -qt-libpng -qt-libjpeg -qt-freetype -device linux-rpi3-g++ -sysroot C:/SysGCC/Raspberry/arm-linux-gnueabihf/sysroot -prefix /usr/local/qt5 -device-option CROSS_COMPILE=C:/SysGCC/Raspberry/bin/arm-linux-gnueabihf- -qt-xcb -skip qtscript -opengl es2 -no-pch -no-use-gold-linker -qt-pcre -qpa xcb -opensource
```

"../qt-everywhere-opensource-src-5.8.0/configure" -platform win32-g++ -xplatform linux-arm-gnueabi-g++ -release -static -nomake tools -make libs -qt-zlib -qt-libpng -qt-libjpeg -qt-freetype -device linux-rpi3-g++ -sysroot C:/SysGCC/Raspberry/arm-linux-gnueabihf/sysroot -prefix /usr/local/qt5 -device-option CROSS_COMPILE=C:/SysGCC/Raspberry/bin/arm-linux-gnueabihf- -qt-xcb -skip qtscript -opengl es2 -no-pch -no-use-gold-linker -qt-pcre -qpa xcb

### 7.5 Build and install the source

Now to compile and install the source code for the raspberry pi, this will take a long time 3-4+ hours; 

```bash
make -j4 && make install
```

The -j4 section instructs make to use 4 separate processes to compile your code making it quicker than using just 1; be aware though this will really task your PC. No mid-compilation gaming will be happening unless you love lag ;). 

I compiled on a mid-range laptop with 8GB of ram and dual core I7 processor and my laptop was HOT using 5GB of ram and CPU usage at 50% constant (obviously not all that was used by the build process). 

### 7.6 Upload the compiled libraries to the Pi

#### 7.6.1 Select example projects

Navigate to and remove any examples you do not want on the pi.

> C:\SysGCC\Raspberry\arm-linux-gnueabihf\sysroot\usr\local\qt5\examples

#### 7.6.2 Upload the compiled code

Navigate to the below and run SmarTTY.exe.

> "C:\SysGCC\Raspberry\TOOLS\PortableSmartty" 

Then select the raspberry pi that you want to run qt. After than run the following in the terminal shown

```bash
# create qt install dir
sudo mkdir -p /usr/local/qt5
sudo chown pi:pi /usr/local/qt5

# register the lib directory in ld
echo /usr/local/qt5/lib | sudo tee /etc/ld.so.conf.d/qt5.conf

# Ensure qt plugins load to be set in /etc/profile
export QT_PLUGIN_PATH=/usr/local/qt5/plugins
export QT_XKB_CONFIG_ROOT=/usr/share/X11/xkb

# give permissions to opt so qt can upload to it
sudo chown pi:pi /opt

# Advanced -> Expand file options
sudo raspi-config

# make font dir
sudo mkdir -p /usr/local/qt5/lib/fonts
```

The select SCP -> Upload directory

> - Local directory: C:\SysGCC\Raspberry\arm-linux-gnueabihf\sysroot\usr\local\qt5
> - Remote directory: /usr/local/qt5
> - Select "Invoke SCP file-by-file(slow)" as I had trouble with files such as wav, bin etc when using "file on fly TAR". You could however just remove the example projects with these in and upload using the faster method then upload these later if required.

Then click upload! This will take awhile as the Qt libraries are rather large even when compiled the examples along are 100mb!

### 7.7 Upload fonts for Qt

Qt no longer get shipped with fonts therefore your application wont be able to load any. SSH in you pi and upload all your font files to:

> /usr/local/qt5/lib/fonts

Initial fonts you could install are listed below

* https://dejavu-fonts.github.io/
* https://material.io/guidelines/resources/roboto-noto-fonts.html

Don't separate the fonts into there own folder just put all the ttf files into the directory above.

## 8.0 Configuring Qt creator

This stage is going to look into setting up Qt creator so you can compile your project on your windows PC and automatically upload the compiled program to your pi.

### 8.1 Set the compiler in mk

The below will remove some warning messages from QtCreator about QMakes compiler links and QCreator kits compilers being different. Go to file:

> C:\SysGCC\Raspberry\arm-linux-gnueabihf\sysroot\usr\local\qt5\mkspecs\devices\linux-rpi3-g++\qmake.conf

Add the following to the file:

> QMAKE_XSPEC = C:\SysGCC\Raspberry\bin\arm-linux-gnueabihf-g++

### 8.2 Add Compiler

Load Qt creator and navigate to:

>  Tools -> Options -> Build & Run -> Compilers

Next we will add the c compiler, click Add-> GCC -> C and fill in the form as below. If the field is not mentioned below then leave it empty or as it is.

> *  Name: C Raspberry Pi
> *  Compiler Path: C:\SysGCC\Raspberry\bin\arm-linux-gnueabihf-gcc.exe

Next we will add the c++ compiler, click Add-> GCC -> C++ and fill in the form as below. If the field is not mentioned below then leave it empty or as it is.

> * Name: C++ Raspberry Pi
> * Compiler Path:  C:\SysGCC\Raspberry\bin\arm-linux-gnueabihf-g++.exe

### 8.3 Adding Qt version

While in the same window as the previous step (8.1) go to the "Qt Version" tab and click "Add"

> - Version Name: Qt 5.8.0 Raspberry Pi 3
> - qMake Location (click browse): C:\SysGCC\Raspberry\arm-linux-gnueabihf\sysroot\usr\local\qt5\bin\qmake.exe 

### 8.4 Add  Raspberry pi device

- In Qt creator and navigate to:

  >  Tools -> Options -> Devices

  Follow the below

  > Add -> Generic Linux Device

  Fill in the form / wizard

  > - Name: Raspberry Pi Workshop    <--  for example
  > - IP address: 192.168.0.108  <-- for example 
  > - Username: pi
  > - Auth type: password
  > - Password: raspberry

  Then 

  > "Next -> Finished"

### 8.5 Adding Debugger

While in the same window as the previous step (8.1) go to the "Debuggers" tab and click "Add" and fill in the form as below. If the field is not mentioned below then leave it empty or as it is.

> * Name: Raspberry Debugger
> * Path: C:\SysGCC\mingw32\bin\gdb.exe

### 8.6 Adding Raspberry Pi 3 Kit

While in the same window as the previous step (8.1) go to the "Kits" tab and click "Add" and fill in the form as below. If the field is not mentioned below then leave it empty or as it is.

> -  Name: rasberry pi 5.8.0
> -  Device Type: Generic Linux Device
> -  Device: Raspberry Pi  <-- We added this in step 8.3
> -  Sysroot: C:\SysGCC\Raspberry\arm-linux-gnueabihf\sysroot\usr\local\qt5
> -  Compiler C: C++ Raspberry Pi
> -  Compiler C++: C Raspberry Pi
> -  Debugger: Raspberry Debugger
> -  Qt Version: Qt 5.8.0 Raspberry Pi 3

## 9.0 Configuring project

### 9.1 Setup build

Simply go to the project tab and double click "raspberry pi 5.8.0"

### 9.2 Run Settings

Open you project and go to 

> Project -> raspberry pi 5.8.0 -> Run 

Under the run section add the below to the "Arguments" text box

> -platform eglfs

### 9.3 Setting up pro file 

For your Qt projects you'll have to add the below to the .pro file. This specifics where your compiled project will be installed on the pi when deployed.

```cmake
target.path = /opt/$${TARGET}/bin
INSTALLS += target
```

## 10.0 Disclaimer 

This guide was initially wrote as notes and not as a step by step guide, therefore there may be some stages and libs that are not required. 

Feel free to play around with the configurations of the Qt and repot back on better methods and configurations.

This guide also takes snippets of knowledge and code from all over the internet. I suggest reading the below links if you have any errors, want to customise the above or just learn more.

* https://visualgdb.com/tutorials/raspberry/qt/embedded/
* https://forum.qt.io/topic/68381/cross-compile-qt-windows-to-raspberry-3
* https://github.com/sdt/docker-raspberry-pi-cross-compiler
* https://www.jaredwolff.com/toolchains/
* https://forum.qt.io/topic/75682/has-anyone-successfully-built-qt-from-source-recently-on-a-pi
* https://github.com/Kukkimonsuta/rpi-buildqt





