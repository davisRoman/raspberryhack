# raspberryhack 
![raspberry](http://wiki.osdev.org/images/3/36/ARM_RaspberryPi_serial.jpg)


# Flash the raspbian image onto the sd card
```
davis@builder1:~/Downloads$ sudo dd bs=4M if=2015-05-05-raspbian-wheezy.img of=/dev/sdb
[sudo] password for davis:
```

Takes a few minutes so go grab some coffee...

When it's complete, you'll see:

```
781+1 records in
781+1 records out
3276800000 bytes (3.3 GB) copied, 789.961 s, 4.1 MB/s
davis@builder1:~/Downloads$
```

You'll want to have minicom handy:

```
davis@builder1:~/Downloads$ sudo apt-get install minicom
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following extra packages will be installed:
  lrzsz
The following NEW packages will be installed:
  lrzsz minicom
0 upgraded, 2 newly installed, 0 to remove and 179 not upgraded.
Need to get 408 kB of archives.
After this operation, 1,503 kB of additional disk space will be used.
Do you want to continue [Y/n]? y
WARNING: The following packages cannot be authenticated!
  lrzsz minicom
Install these packages without verification [y/N]? y
Get:1 http://us.archive.ubuntu.com/ubuntu/ precise/universe lrzsz amd64 0.12.21-5 [111 kB]
Get:2 http://us.archive.ubuntu.com/ubuntu/ precise/universe minicom amd64 2.5-2 [297 kB]
Fetched 408 kB in 0s (1,188 kB/s)
Selecting previously unselected package lrzsz.
(Reading database ... 172444 files and directories currently installed.)
Unpacking lrzsz (from .../lrzsz_0.12.21-5_amd64.deb) ...
Selecting previously unselected package minicom.
Unpacking minicom (from .../minicom_2.5-2_amd64.deb) ...
Processing triggers for man-db ...
Setting up lrzsz (0.12.21-5) ...
Setting up minicom (2.5-2) ...
davis@builder1:~/Downloads$
```


Connect usb debug cable to raspberry pi as shown above

Figure out the device file that maps to the raspbery pi

```
davis@builder1:~/Downloads$ ls -la /dev/ttyUSB*
crw-rw---- 1 root dialout 188, 0 Aug 14 14:34 /dev/ttyUSB0
davis@builder1:~/Downloads$
```

My corresponding device file is /dev/ttyUSB0

Open minicom with /dev/ttyUSB0
