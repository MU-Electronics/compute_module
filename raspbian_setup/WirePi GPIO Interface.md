# Installing WirePi
This is a GPIO interface for the raspberry pi that makes it easy to access the GPIO.

## Check WirePi is not installed

First check GPIO is not installed by running the below. It should say "Command not found"

```
gpio -v
```

If installed run the below

```
sudo apt-get purge wiringpi
hash -r
```

## Install git

```
sudo apt-get install git-core
```

If you get any errors run the below and re-run the above command

```
sudo apt-get update
sudo apt-get upgrade
```

## Get GPIO from git

Run the below

```
cd
git clone git://git.drogon.net/wiringPi
```

## Build GPIO

Build and install WirePi

```
cd ~/wiringPi
./build
```

## Test WirePi

The below two commands will allow you to test the WirePi installation

```
gpio -v
gpio readall
```