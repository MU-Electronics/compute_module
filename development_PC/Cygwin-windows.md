# Cygwin

When running commands on your local machine which OS is windows cygwin has to be used.


## Install

Navigate to https://cygwin.com/install.html and down either the 32 or 64 bit version depending on your OS.

Download and run the exe ensure you select the below as they are not by default not selected:

  * rsync 
  * openssh
  * curl
  * wget
  * unzip
  * xmessage

## Add enviroment vars

Add the below to your system enviroment varibles

```
C:\cygwin64\bin
```

## Install apt-cyg

Open Cygwin and run

```
wget rawgit.com/transcode-open/apt-cyg/master/apt-cyg -P /bin/
chmod +x /bin/apt-cyg

```

## Install addtional packages 
Run the below
```
apt-cyg install bash-completion
apt-cyg install ca-certificates
apt-cyg install curl
apt-cyg install git
apt-cyg install git-svn
apt-cyg install python
apt-cyg install python-setuptools
apt-cyg install rsync
apt-cyg install ncurses
apt-cyg install gdb
```

