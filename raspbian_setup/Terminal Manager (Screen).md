# Terminal Manager (Screen)
    
When performing long proccess over ssh such as
    * Large file downloads
    * Compiling 
    * Updating* etc
    
There can be a major issue of when the SSH link is broken the proccess is also terminated. To stop this from happing we can install screen
which will all the commands to be ran in the backgroung even if the SSH link is broken.
    
## Usage Guide

Screen us very simple to you the below show the basic functions
    
### Create a screen
    
When you SSH into the device and want to run a long task, first thing to do is to create a screen by running the below.

This will create a new screen (terminal) session where by you can run all your required tasks.

```bash
screen
```

### Detaching a screen
    
Once your tasks are running in a screen you can detach a screen, this is like minimising the terminal window. It will continue to run in
the background but not in view. Detach a screen by holding down the following keys

```bash
 “Ctrl-A” and “d“

```

### Re-attaching a screen

If you de-attached a screen or your ssh session failed etc, then you can re enter the screen by running the below

```bash
screen -r
```

If you have multiple screens running you many have to speicif the ID, for example if the responce from the terminal is

```bash
10508.pts-0.mint (09/10/2018 11:58:45 AM)        (Detached)
3451.pts-0.mint (09/10/2018 10:09:05 AM)        (Detached)
```

Then to enter screen 10508 you would run

```bash 
screen -r 10508
```


## Installation

@todo

## Furture reading

Screen also has the ability to run more than one screen which this guide does not cover although there are many resources avaliable online 
to show you how. Some are listed below

* https://www.tecmint.com/screen-command-examples-to-manage-linux-terminals/
