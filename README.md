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

Open minicom with /dev/ttyUSB0 with flow control disabled

```
    +-----------------------------------------------------------------------+
    | A -    Serial Device      : /dev/ttyUSB0                              |
    | B - Lockfile Location     : /var/lock                                 |
    | C -   Callin Program      :                                           |
    | D -  Callout Program      :                                           |
    | E -    Bps/Par/Bits       : 115200 8N1                                |
    | F - Hardware Flow Control : No                                        |
    | G - Software Flow Control : No                                        |
    |                                                                       |
    |    Change which setting?                                              |
    +-----------------------------------------------------------------------+
            | Screen and keyboard      |
            | Save setup as dfl        |
            | Save setup as..          |
            | Exit                     |
            | Exit from Minicom        |
            +--------------------------+
```

In the following to login:

raspberrypi login: pi                   
Password: rasbperry


If login is correct, you'll see the following:

```
Linux raspberrypi 3.18.11-v7+ #781 SMP PREEMPT Tue Apr 21 18:07:59 BST 2015 armv7l
                                        
The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.

NOTICE: the software on this Raspberry Pi has not been fully configured. Please run 'sudo raspi-config'

pi@raspberrypi:~$ 
```

## Connect usb wifi adapter

![trendnet](http://www.trendnet.com/images/products/photos/TEW-648UBM/TEW-648UBM_d01_2.jpg)

Check that the system correctly recognizes the device:


```
pi@raspberrypi:~$ lsusb
Bus 001 Device 002: ID 0424:9514 Standard Microsystems Corp. 
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 003: ID 0424:ec00 Standard Microsystems Corp. 
Bus 001 Device 004: ID 20f4:648b TRENDnet TEW-648UBM 802.11n 150Mbps Micro Wireless N Adapter [Realtek RTL8188CUS]
pi@raspberrypi:~$
```


Cool trendnet appears on the list

typing `ifconfig wlan0` reveals that the wifi interface is up.

```
pi@raspberrypi:~$ ifconfig wlan0
wlan0     Link encap:Ethernet  HWaddr d8:eb:97:2e:34:48  
          UP BROADCAST MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

pi@raspberrypi:~$
```

## Use the wpa_supplicant daemon to connect to a wifi access point using WPA-PSK security.

Using your editor of choice, modify wpa_supplicant config file:

pi@raspberrypi:~$ sudo nano /etc/wpa_supplicant/wpa_supplicant.conf

append the following settings:

```
network={
ssid="INSERT_ACCESSPOINT_NAME_HERE"
psk="INSERT_ACCESSPOINT_PASSWORD"
proto=RSN
key_mgmt=WPA-PSK
pairwise=CCMP
auth_alg=OPEN
}
```
