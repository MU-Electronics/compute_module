# Backup SD Card to Img

This guide will demostrate how to backup an SD card to an img.

## Create the SD card img

The stage will walk through the process of creating the sd card img on linux.

### Find the SD Card

First open a terminal and run the below without the SD card plugged in

```bash
df -h
```

The response should be similar to the below

```bash
Filesystem      Size  Used Avail Use% Mounted on
udev            3.9G     0  3.9G   0% /dev
tmpfs           789M  9.3M  780M   2% /run
/dev/sda1       451G  273G  156G  64% /
tmpfs           3.9G   21M  3.9G   1% /dev/shm
tmpfs           5.0M  4.0K  5.0M   1% /run/lock
tmpfs           3.9G     0  3.9G   0% /sys/fs/cgroup
tmpfs           789M   48K  789M   1% /run/user/1000
```

Now plug in the SD card and run the same command again "df -h". The output should be similar to the below

```bash
Filesystem      Size  Used Avail Use% Mounted on
udev            3.9G     0  3.9G   0% /dev
tmpfs           789M  9.4M  780M   2% /run
/dev/sda1       451G  273G  156G  64% /
tmpfs           3.9G   21M  3.9G   1% /dev/shm
tmpfs           5.0M  4.0K  5.0M   1% /run/lock
tmpfs           3.9G     0  3.9G   0% /sys/fs/cgroup
tmpfs           789M   48K  789M   1% /run/user/1000
/dev/sdd2        30G  4.2G   24G  15% /media/electronics/b4ea8e46-fe87-4ddd-9e94-506c37005ac5
/dev/sdd1        42M   21M   21M  51% /media/electronics/boot
```

As can be seen there are two addtional records /dev/sdd1 and /dev/sdd2. Hence /dev/sdd is the location of the SD card.

Note: The number at the end of /dev/sdd{number} is just a partition on the SD card. For raspberry pi there will be two.

### Find the SD Card

Now we have the location we can create the img


## Resize file structure

The img is a byte for byte copy making the img the same size as the SD card.
Meaning even though only 1 GB of 32 GB may be used the img will still be 32GB.

By reisizing the file structure of the img we can reduce the size of the SD the card. 
This proccess is also required if you want to put an img from a 32GB SD card onto a 8GB SD card for example.

@todo

## Restore the SD card img
