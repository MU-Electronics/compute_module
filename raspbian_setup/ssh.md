# Enabling SSH
SSH needs to be enable to communicate with the rasberry pi remotely.

## Etherent support


Ensure the raspberry pi has networking capabilities. Adding etherent docs can be found in this repo.
Take note of the IP address of the device. 

## Login as root

This just makes thing simplier

```
sudo su
```


## Opening SSH

Run the below

```
raspi-config
```

Select "interface options", then select "SSH", then select "Yes".

Double tap "escape" to get out of the gui.


## Adding SSH-Key 

Some times scripts that will be ran will require multiple sudos and therefore by adding a SSH-Key to the raspberry pi we wont have to keep re-typing the pi's user password.

On your local PC run the below, where "192.168.0.108" is the IP of the raspberry pi.

```
ssh-copy-id -i ~/.ssh/id_rsa pi@192.168.0.108
```

## Testing SSH-key

We can test the above has work by running the below, where "192.168.0.108" is the IP of the raspberry pi.

```
ssh -i ~/.ssh/id_rsa pi@192.168.0.108
```

Type "exit" to exit the ssh connection



