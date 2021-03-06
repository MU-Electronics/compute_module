# Building Qt 5.9.1 from source on Pi

This guide shows how to compile Qt 5.9.1 from source directly on the compute module. 

NOTE: The build proccess will take a long time and the PI/broadcom IC will get very hot to i suggest a fan over the top of the module espcially when using multi-core build.

NOTE: It is highly suggested that you use screen during the below proccess when performing over SSH. This means if the SSH link fails the proccesses will continue in the background. Upon reconnecting oyu can then re enter the screen to see the progress. 

## Screen

Not required but suggested over ssh

```bash
screen
```

## Download Qt

Download Qt 5.9.1 to pi user's home

```bash
wget https://download.qt.io/archive/qt/5.9/5.9.1/single/qt-everywhere-opensource-src-5.9.1.zip
```

You can detach the screen by running the below.

```
Ctrl-A” and “d“.
```

## Unzip Qt

If you disconnected from the screen in the previous process run the below to re-enter the screen

```bash
screen -r
```

Install unzip library

```bash
sudo apt-get install unzip
```

Unzip the Qt source

``` bash
unzip qt-everywhere-opensource-src-5.9.1.zip
```

## Make build folder

Next make a folder to build Qt from

```bash
mkdir native-build
cd native-build
```

## Install dependances

The first step is to ensure we have all the libraries required to build Qt, the the below 

```bash
sudo apt-get install build-essential libfontconfig1-dev libdbus-1-dev libfreetype6-dev libicu-dev libsqlite3-dev libssl-dev libpng12-dev libjpeg9-dev libglib2.0-dev
```

### If you want blue tooth support run the below

```bash
sudo apt-get install bluez libbluetooth-dev
```

### If you want mysq support

```bash
sudo apt-get install libpq-dev libmariadbclient-dev
```

### If you want printing support

```bash
sudo apt-get install libcups2-dev
```

## Configure Qt Build

Now we need to configure Qt, you have to run the configure script twice, accept this and move on.

### First configuration

Run the below

```bash
../qt-everywhere-opensource-src-5.9.1/configure -v -opengl es2 -eglfs -no-gtk -device linux-rasp-pi3-g++ -device-option CROSS_COMPILE=/usr/bin/ -opensource -confirm-license -reduce-exports -force-pkg-config -nomake examples -no-compile-examples -skip qtwayland -skip qtwebengine -release -qt-pcre -ssl -evdev -system-freetype -fontconfig -no-use-gold-linker -prefix /opt/Qt5.9.1 -make libs -I /usr/include/glib-2.0/ -I /usr/lib/arm-linux-gnueabihf/glib-2.0/include/ -I /usr/include/freetype2/ -I /usr/include/dbus-1.0/ -I /usr/lib/arm-linux-gnueabihf/dbus-1.0/include/  -I /usr/include/glib-2.0/ -I /usr/lib/arm-linux-gnueabihf/glib-2.0/include/ -I /usr/include/gstreamer-0.10/ -I /usr/include/glib-2.0/ -I /usr/lib/arm-linux-gnueabihf/glib-2.0/include/ -I /usr/include/libxml2/ -I /usr/include/freetype2/
```

#### Check EGLF is detected

As the Qt build is designed to be ran on a minimal version of rasperbian with no widnows manager we want to make sure EGLF is configured. 

The termninal output should contain the below

```
QPA backends:
 DirectFB ............................... no
 EGLFS .................................. yes
  EGLFS details:
  {LOTS OF OPTIONS}
  EGLFS Rasberry Pi .................... yes
```

Taking note of EGLFS = yes and EGLFS Rasberry Pi = yes

#### Are there any errors?

At the end of the output from the script it will list any ERROR. If there are any MAKE SURE you solve them.

Delete the native-build folder and re-run the above to check there error have gone.

Any errors at this stage will cause issues down the line!

#### Final configuration

Now re-run the configutation below (same as above but with sys-root option)

```bash
../qt-everywhere-opensource-src-5.9.1/configure -v -opengl es2 -eglfs -no-gtk -device linux-rasp-pi3-g++ -device-option CROSS_COMPILE=/usr/bin/ -opensource -confirm-license -reduce-exports -force-pkg-config -nomake examples -no-compile-examples -skip qtwayland -skip qtwebengine -release -qt-pcre -ssl -evdev -system-freetype -fontconfig -no-use-gold-linker  -sysroot / -prefix /opt/Qt5.9.1 -recheck -make libs -I /usr/include/glib-2.0/ -I /usr/lib/arm-linux-gnueabihf/glib-2.0/include/ -I /usr/include/freetype2/ -I /usr/include/dbus-1.0/ -I /usr/lib/arm-linux-gnueabihf/dbus-1.0/include/ -I /usr/include/glib-2.0/ -I /usr/lib/arm-linux-gnueabihf/glib-2.0/include/ -I /usr/include/gstreamer-0.10/ -I /usr/include/glib-2.0/ -I /usr/lib/arm-linux-gnueabihf/glib-2.0/include/ -I /usr/include/libxml2/ -I /usr/include/freetype2/
```

## Make Qt

Run the below to make Qt using 4 tasks

```bash
make -j4
```

## Install Qt

After the build proccess has finished next stage is to install Qt by running the below

```bash
make install
```

## Additional Reading

* http://www.tal.org/building_qt_5_for_raspberrypi_jessie
* https://wiki.qt.io/Native_Build_of_Qt5_on_a_Raspberry_Pi







