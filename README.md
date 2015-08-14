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
